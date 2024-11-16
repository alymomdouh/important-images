
## [Calling Views, Stored Procedures and Functions in EF Core](https://antondevtips.com/blog/calling-views-stored-procedures-and-functions-in-ef-core)

While EF Core provides a robust API for interacting with the database, there are scenarios where you need to call existing database views, stored procedures, or functions.
In this blog post, we will explore how to call views, stored procedures, and functions using EF Core.

## How To Call Database Views in EF Core
A database view is a virtual table that contains data from one or multiple database tables. Unlike a physical table, a view does not store data itself.

It contains a set of predefined SQL queries to fetch data from the database.

Let's explore an application that stores Customers, Products, Orders, and OrderDetails in the Postgres database. Here is what our database looks like:

```

CREATE TABLE customers (
    id INT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    name VARCHAR(255),
    phone VARCHAR(20),
    email VARCHAR(255),
    is_active BOOLEAN
);

CREATE TABLE products (
    id INT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    name VARCHAR(255),
    price DECIMAL
);

CREATE TABLE orders (
    id INT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    customer_id INT,
    date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

CREATE TABLE order_details (
    id INT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    order_id INT,
    product_id INT,
    quantity INT,
    price DECIMAL(10, 2),
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```
And we have a EF Core DbContext that maps to this database:

```

public class ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : DbContext(options)
{
    public DbSet<Customer> Customers { get; set; }
    public DbSet<Product> Products { get; set; }
    public DbSet<Order> Orders { get; set; }
    public DbSet<OrderDetail> OrderDetails { get; set; }
}
```
Our database has a view that returns total sales per each Customer:

```

CREATE VIEW IF NOT EXISTS total_sales_per_customer AS
SELECT customer.name, SUM(order_details.quantity * order_details.price) AS total_sales
FROM customers customer
         JOIN orders "order" ON customer.id = "order".customer_id
         JOIN order_details order_details ON "order".id = order_details.order_id
GROUP BY customer.name
ORDER BY total_sales desc;
```
In this Postgres database, you can call the view to get the results:

```

select * from total_sales_per_customer;
```
To call a view in EF Core, you need to map it to a model class and add to the DbContext as keyless entity:

```
public class TotalSalesPerCustomer
{
    public string Name { get; set; }
    public decimal TotalSales { get; set; }
}

public class ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : DbContext(options)
{
    public DbSet<TotalSalesPerCustomer> TotalSalesPerCustomers { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<TotalSalesPerCustomer>()
            .HasNoKey()
            .ToView("total_sales_per_customer");
    }
}

```
We map entity as HasNoKey because we are calling the database view that doesn't have a primary key. In the ToView method you need to specify the name of the database view.

And here is how you can call the view in the minimal API endpoint:

```
app.MapGet("/api/total-sales", async (ApplicationDbContext dbContext) =>
{
    var totalSales = await dbContext.TotalSalesPerCustomers
        .ToListAsync();

    return Results.Ok(totalSales);
});
```
As a result, you will receive something like this:

```

[
    {
        "name": "Virginia Champlin",
        "totalSales": 153486.75
    },
    {
        "name": "Mary Kessler",
        "totalSales": 132898.65
    },
    {
        "name": "Louis Stark",
        "totalSales": 112785.54
    },
    {
        "name": "Kirk Renner",
        "totalSales": 104335.26
    },
    {
        "name": "Jan Keebler",
        "totalSales": 78566.76
    }
]

```
## How To Call Stored Procedures in EF Core
A stored procedure in SQL is a group of SQL queries that can be saved and reused multiple times. Unlike a database view, stored procedure executes SQL statements that don't return data.

Let's assume that our database has the following stored procedure to update price in the order_details price:

```

CREATE OR REPLACE PROCEDURE update_order_details_price(
    orderDetailId INT,
    newPrice DECIMAL
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE order_details
    SET price = newPrice
    WHERE id = orderDetailId;
    COMMIT;
END;
$$;
```
EF Core allows you to call stored procedures directly.

```

public class ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : DbContext(options)
{
    public async Task UpdateOrderDetailPriceAsync(int orderDetailId, decimal newPrice)
    {
        await Database.ExecuteSqlAsync($"CALL update_order_details_price({orderDetailId}, {newPrice})");
    }
}
```
Despite using interpolated string, ExecuteSqlAsync method is safe as the string is actually a FormattableString.

If you don't want to use string interpolation, you can use ExecuteSqlRawAsync method and pass arguments:

```

public async Task UpdateOrderDetailPriceAsync(int orderDetailId, decimal newPrice)
{
    await Database.ExecuteSqlRawAsync("CALL update_order_details_price({0}, {1})", orderDetailId, newPrice);
}
```
Now, let's define a POST minimal API endpoint to update the price:

```

public record UpdateRequest(int OrderDetailId, decimal NewPrice);

app.MapPost("/update-order-detail-price",
    async ([FromBody] UpdateRequest request, ApplicationDbContext context) =>
{
    await context.UpdateOrderDetailPriceAsync(request.OrderDetailId, request.NewPrice);
    return Results.Ok();
});
```
### How To Call Database Functions in EF Core
A function in SQL is a group of SQL queries that can be saved and reused multiple times. It is similar to stored procedure but return data.

A function can be one of the following types:

## 1.table-valued function
## 2.scalar function.
## How To Call Table-Valued Function in EF Core
Table-Valued function is a function that returns data of a table type.

Let's assume that our database has the following function that returns total sales of the customer:

```

CREATE OR REPLACE FUNCTION get_customer_total_sales(customerId INT)
RETURNS TABLE (
    name VARCHAR,
    total_sales DECIMAL
) AS $$
BEGIN
    RETURN QUERY
    SELECT c.name, SUM(od.quantity * od.price) AS total_sales
    FROM customers c
    JOIN orders o ON c.id = o.customer_id
    JOIN order_details od ON o.id = od.order_id
    WHERE c.id = customerId
    GROUP BY c.name;
END;
$$ LANGUAGE plpgsql;
```
You can map this function by using the HasDbFunction method in the DbContext:

```

public class CustomerTotalSales
{
    public string Name { get; set; }
    public decimal TotalSales { get; set; }
}

public class ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : DbContext(options)
{
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDbFunction(() => GetCustomerTotalSales(default))
            .HasName("get_customer_total_sales");
    }
    
    public IQueryable<CustomerTotalSales> GetCustomerTotalSales(int customerId) =>
        FromExpression(() => GetCustomerTotalSales(customerId));
}
```
Here we create a public function, that returns a SQL expression, by wrapping GetCustomerTotalSales method using FromExpression method from the base DbContext class.

Then we map it to the database function "get_customer_total_sales" using the HasDbFunction method.

Now we can call this function inside a minimal API endpoint:

```

app.MapGet("/get-customer-total-sales/{customerId}",
    async (int customerId, ApplicationDbContext context) =>
{
    var customerTotalSales = await context.GetCustomerTotalSales(customerId)
        .ToListAsync();
        
    return Results.Ok(customerTotalSales);
});
```
When calling this endpoint, you'll get the following response:

```

[
  {
    "name": "Kirk Renner",
    "totalSales": 34778.42
  }
]
```
## How To Call Scalar Function in EF Core
Scalar function is a function that returns a single value.

Let's assume that our database has the following function that returns total order detail price:

```

CREATE OR REPLACE FUNCTION get_order_detail_price(productId INT)
RETURNS DECIMAL AS $$
BEGIN
    RETURN (SELECT SUM(price * quantity) FROM order_details WHERE product_id = productId);
END;
$$ LANGUAGE plpgsql;
```
This function returns a sum of all order details of the particular product.

We can map it in EF Core using the HasDbFunction method:

```

public class ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : DbContext(options)
{
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDbFunction(
            typeof(ApplicationDbContext).GetMethod(nameof(GetOrderDetailPrice), 
            new[] { typeof(int) })!
        ).HasName("get_order_detail_price");
    }
    
    public static decimal GetOrderDetailPrice(int productId)
        => throw new NotImplementedException();
}
```
Just never mind throw new NotImplementedException(); as this method won't be called directly and is proxied by EF Core to call the actual database function.

Now you can use this function inside a LINQ query to get products that have total order details more than 20 000:

```

app.MapGet("/get-expensive-products-by-orders",
    async (ApplicationDbContext context) =>
{
    var expensiveProducts = await context.Products
        .Where(x => ApplicationDbContext.GetOrderDetailPrice(x.Id) > 20_000m)
        .ToListAsync();

    return Results.Ok(expensiveProducts);
});
```
When calling this endpoint, you'll get the following response:

```

[
  {
    "id": 7,
    "name": "Fantastic Soft Pizza",
    "price": 60.46,
    "orderDetails": []
  },
  {
    "id": 9,
    "name": "Handmade Wooden Shoes",
    "price": 893.77,
    "orderDetails": []
  },
  {
    "id": 10,
    "name": "Awesome Soft Bike",
    "price": 256.69,
    "orderDetails": []
  }
]
````
