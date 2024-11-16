
## [How To Increase EF Core Performance for Read Queries in .NET](https://antondevtips.com/blog/how-to-increase-ef-core-performance-for-read-queries-in-dotnet)
![image](https://github.com/user-attachments/assets/ac1cf911-11e5-49e1-bc8d-f505f0f264a9)

Optimizing read queries in EF Core can significantly enhance the performance of your .NET applications.

In this blog post, I will share with you actionable techniques to enhance the performance of read queries in EF Core.

## Understanding EF Core Query Execution
Before diving into optimization techniques, it's essential to understand how EF Core processes queries:

## 1.LINQ Query Definition: you define queries using LINQ.
## 2.Translation to SQL: EF Core translates LINQ queries into SQL queries compatible with the underlying database.
## 3.Query Execution: SQL query is executed against the database.
## 4.Materialization: results are mapped back to .NET objects.
Performance issues can arise at any of these stages, so optimization strategies may target different parts of this process.

## Performance Tip 1: Add Database Indexes
Ensure your database tables are properly indexed based on query patterns. EF Core doesn't create indexes automatically, so you need to define them manually in your database schema.

Here is how you can create an index using SQL:

``` 
CREATE INDEX IX_Books_Title ON Books(Title);
```
You can also create an index in EF Core Mapping with HasIndex method, and it will be added to the database migration:

```

public class BookConfiguration : IEntityTypeConfiguration<Book>
{
    public void Configure(EntityTypeBuilder<Book> builder)
    {
        builder.ToTable("books");

        builder.HasKey(x => x.Id);
        builder.HasIndex(x => x.Title);
    }
}
```
Create indexes on columns that are frequently used in WHERE, ORDER BY, and JOIN clauses.
## Performance Tip 2: Optimize Query Projections
Retrieve only the necessary fields rather than the entire entity.

Let's imagine that you need to show a Book Title, Author and Year of publication on your website in the book preview. Instead of retrieving a full Book entity together with Author:

```

var book = await context.Books
    .Include(b => b.Author)
    .FirstOrDefaultAsync(b => b.Id == id, cancellationToken);
```
You can retrieve only the needed properties:

```

var book = await context.Books
    .Include(b => b.Author)
    .Where(b => b.Id == id)
    .Select(b => new BooksPreviewResponse
    {
        Title = b.Title, Author = b.Author.Name, Year = b.Year
    })
    .FirstOrDefaultAsync(cancellationToken);
```
This reduces the amount of data transferred from the database and the time spent when materializing entities.

### Performance Tip 3: Use AsNoTracking for Read-Only Queries
By default, EF Core tracks entities to detect changes. For read-only operations, this tracking is unnecessary and adds performance overhead.

You can use AsNoTracking method to disable change tracking for the query, reducing memory usage and CPU overhead.

```

var book = await context.Books
    .AsNoTracking()
    .Include(b => b.Author)
    .Select(b => new BooksPreviewResponse
    {
        Title = b.Title, Author = b.Author.Name, Year = b.Year
    })
    .FirstOrDefaultAsync();
```
You can use AsNoTracking method when you only need to retrieve the entity which will be not further updated or deleted during DbContext lifetime. 

In other words, you should be using AsNoTracking method when you don't plan to call the SaveChanges method.

In some scenarios, you might have a readonly DbContext. So you can disable change tracking for all queries:

```

builder.Services.AddDbContext<ApplicationDbContext>((provider, options) =>
{
    var interceptor = provider.GetRequiredService<AuditableInterceptor>();

    options.EnableSensitiveDataLogging()
        .UseNpgsql(connectionString)
        .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
});
```
When using global QueryTrackingBehavior.NoTracking you can call the .AsTracking() method to enable change tracking for a given query.

### Performance Tip 4: Use Eager Loading Wisely
Fetching related data can result in multiple database queries (N+1 problem). Use eager loading to include related entities.

While Include and ThenInclude are useful for eager loading, they can lead to performance issues if not used carefully.

```

var book = await context.Books
    .AsNoTracking()
    .Include(b => b.Author)
        .ThenInclude(a => a.Publisher)
    .ToListAsync();
```
Ensure you're not loading unnecessary related data.

You can use filters in Include and ThenInclude methods to limit data that is loaded together with the main entity. For example:

```

var authors = await context.Authors
    .Include(a => a.Books.Where(b => b.Year >= 2023))
    .ToListAsync();
```
### Performance Tip 5: Use Pagination for Large Datasets
Prefer using pagination instead of retrieving the whole large dataset to avoid loading too much data into memory.

```

int pageSize = 50;
int pageNumber = 1;

var books = context.Books
    .AsNoTracking()
    .OrderBy(p => p.Title)
    .Skip((pageNumber - 1) * pageSize)
    .Take(pageSize)
    .ToList();
```
If possible, prefer cursor-based pagination to offset-based pagination as it is much faster.

```

var books = context.Books
    .AsNoTracking()
    .Where(b => b.Id > lastBookId)
    .OrderBy(b => b.Title)
    .Take(pageSize)
    .ToList();
```
Offset-based pagination is slower because it needs to go through a given numbers of database rows to skip them.

While, cursor-based pagination uses a WHERE clause on the indexed field (often primary key) that discards a set of rows.

## Performance Tip 6: Use Compiled Queries
For frequently executed queries, compiled queries can improve performance by caching the translation of LINQ queries to SQL.

Here is how you can create and use a compiled query:

```

public class ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : DbContext(options)
{
    private static readonly Func<ApplicationDbContext, string, Book?> BookByTitle
        = EF.CompileQuery((ApplicationDbContext context, string title) =>
            context.Books
                .Include(b => b.Author)
                .FirstOrDefault(b => b.Title == title)
        );

    public Book? GetBookByTitle(string title)
        => BookByTitle(this, title);
}

var book = context.GetBookByTitle("Clean Code");
```
You can also create an async compiled query:

```

public class ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : DbContext(options)
{
    private static readonly Func<ApplicationDbContext, string, CancellationToken, Task<Book?>> BookByTitleAsync
        = EF.CompileAsyncQuery((ApplicationDbContext context, string title, CancellationToken cancellationToken) =>
            context.Books
                .Include(b => b.Author)
                .FirstOrDefault(b => b.Title == title)
        );

    public async Task<Book?> GetBookByTitleAsync(string title, CancellationToken cancellationToken)
        => await BookByTitleAsync(this, title, cancellationToken);
}

var book = await context.GetBookByTitleAsync("Clean Code", cancellationToken);
```
### Performance Tip 7: Use SplitQuery to Avoid Cartesian Explosion
For queries that include related data, EF Core can split the query into multiple SQL statements to avoid cartesian explosion.

By default, EF Core generates a single SQL query with JOINs to retrieve all the data.

```

var book = await context.Books
    .AsNoTracking()
    .Include(b => b.Author)
        .ThenInclude(a => a.Publisher)
    .AsSplitQuery()
    .ToListAsync();
```
With .AsSplitQuery(), EF Core will generate multiple queries: one for Book data, one for Author data, and one for Publisher data.

You should use this method with cautious, as it may decrease performance in a lot of scenarios when you don't have a big set of joined data.

### Performance Tip 8: Use Raw SQL Queries for Complex Operations
In some cases, it might be more efficient to write an SQL query yourself.

```

var year = 2020;
var newBooks = await context.Database
    .SqlQuery<Book>($"SELECT * FROM Books WHERE Year > {year}")
    .ToListAsync();
```
You can also write queries that are not supported by EF Core:

```

var sql = @"
    SELECT * FROM ""Books""
    WHERE ""Year"" < @Year
    FOR UPDATE
";
```
```
var yearParameter = new NpgsqlParameter("@Year", NpgsqlTypes.NpgsqlDbType.Integer)
{
    Value = 2020
};

var books = await context.Books
    .FromSqlRaw(sql, yearParameter)
    .ToListAsync();
```
### Performance Tip 9: Introduce Caching
You can introduce caching for frequently used queries to increase performance and reduce a database load. You can use in-memory or distributed caching systems like Redis.

```

public async Task<Book?> GetBookAsync(Guid id, CancellationToken cancellationToken)
{
    var cacheKey = $"Book_{id}";

    if (_cache.TryGetValue(cacheKey, out Book? book))
    {
        return book;
    }
    
    book = await _context.Books
        .Include(b => b.Author)
        .FirstOrDefaultAsync(b => b.Id == id, cancellationToken);
        
    return book;
}
```
In .NET 9 I highly recommend using new HybridCache that solves the Cache Stampede problem.

### Summary
In this blog post, you have learned various techniques that allow increasing the performance of your read queries in EF Core.

Before taking any significant actions, I recommend the following:

#### .Measure the performance of your queries with BenchmarkDotNet
#### .Use tools like SQL Server Management Studio to view database execution plans
#### .Scan your indexes for fragmentation and take actions if needed 

 


