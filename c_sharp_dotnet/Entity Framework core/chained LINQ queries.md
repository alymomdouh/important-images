

## [Conditional application of chained LINQ queries](https://dev.to/veloek/conditional-application-of-chained-linq-queries-560p?bb=184572)

I write C# and I'm a big fan of using LINQ to create nice, readable chains of operations to filter and transform data in a functional style.

```
int HighestEvenNumber(IEnumerable<int> numbers)
    => numbers
        .Where(x => x % 2 == 0)
        .OrderByDescending(x => x)
        .FirstOrDefault();
```
Sometimes though, there are conditions where parts of the chain should be included or omitted, but breaking the chain to add an if-statement feels wrong and disruptive.

Let's say our previous example should throw an exception if there are no even numbers in the sequence instead of returning the default int value of 0.

```
int HighestEvenNumber(IEnumerable<int> numbers)
{
    int[] evenNumbers = numbers
        .Where(x => x % 2 == 0)
        .ToArray();

    if (evenNumbers.Length == 0)
    {
        throw new ArgumentException("No even numbers in sequence");
    }

    return evenNumbers
        .OrderByDescending(x => x)
        .First();
}
```
Thankfully it can be improved with a simple extension method (thanks, C#):
```
public static T If<T>(this T source, Func<T, bool> condition, Func<T, T> then)
    => condition(source) ? then(source) : source;
```
With this extension method in place, we can now write the LINQ query like this:
```
int HighestEvenNumber(IEnumerable<int> numbers)
    => numbers
        .Where(x => x % 2 == 0)
        .ToArray()
        .If(evenNumbers => evenNumbers.Length == 0,
            _ => throw new ArgumentException("No even numbers in sequence"))
        .OrderByDescending(x => x)
        .First();
```
And it just feels so much better, right?

This example might not seem very impressive, so let's try another:
```
string[] QueryMovieTitles(IEnumerable<Movie> movies, string? search = null, bool orderDesc = false, int limit = 10)
    => movies
        .Select(movie => movie.Title)
        .If(!string.IsNullOrEmpty(search),
            titles => titles.Where(s => s.Contains(search, StringComparison.OrdinalIgnoreCase)))
        .If(orderDesc,
            titles => titles.OrderByDescending(s => s),
            titles => titles.OrderBy(s => s))
        .Take(limit)
        .ToArray();
```
Of course, this is not simply an aesthetic improvement. The whole LINQ query is now a single expression which has some nice benefits, including:

You don't have to introduce temporary variables to store intermediate results

It can be used in an Expression lambda

The complete set of extension methods to cover most use-cases:

```

public static class IfExtensions
{
    public static T If<T>(this T source, bool condition, Func<T, T> then)
        => condition ? then(source) : source;

    public static TOut If<TIn, TOut>(this TIn source, bool condition, Func<TIn, TOut> then, Func<TIn, TOut> @else)
        => condition ? then(source) : @else(source);

    public static T If<T>(this T source, Func<T, bool> condition, Func<T, T> then)
        => condition(source) ? then(source) : source;

    public static TOut If<TIn, TOut>(this TIn source, Func<TIn, bool> condition, Func<TIn, TOut> then, Func<TIn, TOut> @else)
        => condition(source) ? then(source) : @else(source);
}
```
Enjoy!
