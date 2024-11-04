
## [20+ LINQ Concepts with .Net Code](https://dotnet-fullstack-dev.blogspot.com/2024/09/20%20LINQ%20Concepts%20with%20.Net%20Code.html)


## LINQ (Language Integrated Query) 
is one of the most powerful features in .NET, providing a unified syntax to query collections, databases, XML, and other data sources. 

Below are 20+ important LINQ concepts, their explanations, and code snippets to help you understand their usage.

## 1. Where (Filtering)
The Where() method is used to filter a collection based on a given condition.
```
var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
var evenNumbers = numbers.Where(n => n % 2 == 0).ToList();
// Output: [2, 4, 6]
```
 
### 2. Select (Projection)

The Select() method projects each element of a sequence into a new form, allowing transformation of data.

```
var employees = new List<Employee> { /* ... */ };
var employeeNames = employees.Select(e => e.Name).ToList();
// Output: List of employee names
```
## 3. OrderBy (Sorting in Ascending Order)

The OrderBy() method sorts the elements of a sequence in ascending order.
```
var employees = new List<Employee> { /* ... */ };
var sortedEmployees = employees.OrderBy(e => e.Name).ToList();
// Output: Employees sorted by Name in ascending order
```
## 4. OrderByDescending (Sorting in Descending Order)
OrderByDescending() sorts the elements of a sequence in descending order.
```
var sortedEmployeesBySalary = employees.OrderByDescending(e => e.Salary).ToList();
// Output: Employees sorted by Salary in descending order
```
### 5. First (Get First Element)
First() returns the first element of a collection that satisfies a condition or throws an exception if none are found.
```
var firstEmployee = employees.First(e => e.Salary > 50000);
// Output: First employee with Salary > 50000
```
## 6. FirstOrDefault (First or Default Value)
FirstOrDefault() returns the first element or a default value (null or default of type) if none match the condition.
```
var employee = employees.FirstOrDefault(e => e.Salary > 100000);
// Output: First employee with Salary > 100000, or null if none found
```
## 7. Single (Return Exactly One Element)
Single() returns the only element that satisfies a condition and throws an exception if more than one or no elements are found.
```
var singleEmployee = employees.Single(e => e.Id == 1);
// Output: Employee with Id 1
```
## 8. SingleOrDefault (Single or Default Value)
SingleOrDefault() returns the only element that satisfies a condition, or a default value if no elements match.
```
var singleEmployeeOrNull = employees.SingleOrDefault(e => e.Id == 100);
// Output: Employee with Id 100, or null if none found
```
## 9. Take (Take First N Elements)
Take() returns the first n elements from a collection.
```
var topThreeEmployees = employees.Take(3).ToList();
// Output: First 3 employees from the list
```
## 10. Skip (Skip First N Elements)
Skip() skips the first n elements and returns the remaining elements.
```
var remainingEmployees = employees.Skip(2).ToList();
// Output: Employees after skipping the first 2
```
## 11. Distinct (Remove Duplicates)
Distinct() removes duplicate elements from a collection.
```
var uniqueDepartments = employees.Select(e => e.Department).Distinct().ToList();
// Output: List of unique departments
```
## 12. GroupBy (Grouping Data)
GroupBy() groups elements of a collection by a specified key.
```
var employeesByDepartment = employees.GroupBy(e => e.Department)
                                     .Select(g => new { Department = g.Key, Employees = g.ToList() })
                                     .ToList();
// Output: Groups employees by their department
```
## 13. Join (Inner Join)
Join() is used to combine two collections based on a common key, similar to an SQL inner join.
```
var employeesWithDepartments = from e in employees
                               join d in departments on e.DepartmentId equals d.Id
                               select new { e.Name, d.Name };
// Output: List of employees with their respective departments
```
### 14. GroupJoin (Left Outer Join)
GroupJoin() performs a left outer join between two collections, combining each element from the first collection with a group of matching elements from the second collection.
```
var departmentsWithEmployees = departments.GroupJoin(employees,
    d => d.Id,
    e => e.DepartmentId,
    (d, empGroup) => new { Department = d.Name, Employees = empGroup.ToList() });
// Output: List of departments with employees in each
```
## 15. Any (Check if Any Element Matches Condition)
Any() returns true if any elements in a collection satisfy a condition.
```
bool hasHighEarners = employees.Any(e => e.Salary > 100000);
// Output: True if any employee has a salary greater than 100000
```
## 16. All (Check if All Elements Match Condition)
All() returns true if all elements in a collection satisfy a condition.
```
bool allHaveHighSalary = employees.All(e => e.Salary > 30000);
// Output: True if all employees have a salary greater than 30000
```
## 17. Count (Count Elements in a Collection)
Count() returns the total number of elements or the number of elements matching a condition.
```
int totalEmployees = employees.Count();
int highSalaryCount = employees.Count(e => e.Salary > 50000);
// Output: Total employees and employees with salary > 50000
```
## 18. Max (Maximum Value)
Max() returns the maximum value from a collection.
```
var maxSalary = employees.Max(e => e.Salary);
// Output: Maximum salary in the employee list
```
## 19. Min (Minimum Value)
Min() returns the minimum value from a collection.
```
var minSalary = employees.Min(e => e.Salary);
// Output: Minimum salary in the employee list
```
## 20. Sum (Sum of Values)
Sum() computes the sum of values in a collection.
```
var totalSalary = employees.Sum(e => e.Salary);
// Output: Total sum of salaries
```
## 21. Average (Average Value)
Average() computes the average value of a sequence.
```
var averageSalary = employees.Average(e => e.Salary);
// Output: Average salary in the employee list
```
## 22. Zip (Combining Two Sequences)
Zip() applies a specified function to the corresponding elements of two sequences.
```
var list1 = new List<int> { 1, 2, 3 };
var list2 = new List<int> { 4, 5, 6 };
var result = list1.Zip(list2, (a, b) => a + b).ToList();
// Output: [5, 7, 9] (sum of corresponding elements)
```
## 23. SelectMany (Flattening Collections)
SelectMany() flattens a collection of collections into a single collection.
```
var departments = new List<Department> { /*...*/ };
var allEmployees = departments.SelectMany(d => d.Employees).ToList();
// Output: A flat list of all employees from all departments
```
## 24. Aggregate (Custom Accumulation Operation)
Aggregate() applies an accumulator function over a sequence.
```
var numbers = new List<int> { 1, 2, 3, 4 };
var product = numbers.Aggregate((total, next) => total * next);
// Output: Product of all numbers (1 * 2 * 3 * 4 = 24)
```
## 25. Union (Combine and Remove Duplicates)
Union() combines two collections and removes duplicates.
```
var list1 = new List<int> { 1, 2, 3 };
var list2 = new List<int> { 3, 4, 5 };
var unionList = list1.Union(list2).ToList();
// Output: [1, 2, 3, 4, 5]
```
## 26. Intersect (Common Elements)
Intersect() returns elements that are present in both collections.
```
var commonElements = list1.Intersect(list2).ToList();
// Output: [3] (common elements in both lists)
```
## 27. Except (Difference between Two Collections)
Except() returns elements from the first collection that are not in the second collection.
```
var difference = list1.Except(list2).ToList();
// Output: [1, 2] (elements in list1 but not in list2)
```
## Conclusion
LINQ is an essential feature in .NET that greatly simplifies data querying across various data sources. 

It promotes cleaner, more readable code, and helps with both in-memory and database
