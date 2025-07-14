# .NET Interview Guide - Part 3: Data Types & Language Features

## ✅ Assemblies and Namespaces

* **Assembly**: Compiled code (.dll or .exe).
* **Namespace**: Logical grouping of classes.

---

## ✅ Nullable Types

* Allow value types to hold `null`.
* Syntax: `int?`, `bool?`

```csharp
int? age = null;
```

---

## ✅ Enums

Enums are strongly typed constants that improve code readability.

### **Example:**

```csharp
public enum OrderStatus
{
    Pending,
    Processing,
    Completed,
    Cancelled
}

OrderStatus status = OrderStatus.Processing;
Console.WriteLine(status); // Output: Processing
```

### **Real-life Example:**

Used to represent order statuses, user roles, error codes, etc.

---

## ✅ Structs vs Classes

| Feature             | Struct                       | Class                         |
| ------------------- | ---------------------------- | ----------------------------- |
| Type                | Value Type                   | Reference Type                |
| Memory Allocation   | Stack                        | Heap                          |
| Inheritance         | Cannot inherit other types   | Can inherit other classes     |
| Default Constructor | Not allowed (auto-generated) | Allowed                       |
| Performance         | Faster for small data types  | Slower, but flexible          |
| Use Case            | Small, lightweight objects   | Complex objects with behavior |

### **Example of Struct:**

```csharp
public struct Point
{
    public int X;
    public int Y;
}

Point p = new Point { X = 10, Y = 20 };
```

### **Best Practice:**

* Use structs for simple, short-lived data.
* Use classes for complex objects with behavior and inheritance.

---

## ✅ Introduction to LINQ

LINQ (Language Integrated Query) provides a consistent way to query and manipulate data from various sources (collections, databases, XML, etc.) in C#.

### **Key Features:**

* Integrates queries directly into C# syntax.
* Strongly typed and checked at compile-time.
* Supports both query syntax and method syntax.

### **Example (Method Syntax):**

```csharp
int[] numbers = { 1, 2, 3, 6, 8, 9 };
var result = numbers.Where(n => n > 5).ToList();

foreach (var num in result)
{
    Console.WriteLine(num);
}
```

### **Example (Query Syntax):**

```csharp
var query = from n in numbers
            where n > 5
            select n;

foreach (var num in query)
{
    Console.WriteLine(num);
}
```

### **Common LINQ Methods with Examples:**

* `Where`: Filters elements based on condition

```csharp
var evenNumbers = numbers.Where(n => n % 2 == 0);
```

* `Select`: Projects each element into a new form

```csharp
var squares = numbers.Select(n => n * n);
```

* `OrderBy` / `OrderByDescending`: Sorts elements

```csharp
var sorted = numbers.OrderBy(n => n);
var descending = numbers.OrderByDescending(n => n);
```

* `First`, `FirstOrDefault`, `Last`, `Single`

```csharp
var firstEven = numbers.First(n => n % 2 == 0);
var maybeValue = numbers.FirstOrDefault(n => n > 100); // returns 0 if not found
```

* `Any`, `All`, `Contains`

```csharp
bool hasOdd = numbers.Any(n => n % 2 != 0);
bool allPositive = numbers.All(n => n > 0);
bool containsFive = numbers.Contains(5);
```

* `Count`, `Sum`, `Average`, `Max`, `Min`

```csharp
int count = numbers.Count();
int total = numbers.Sum();
double average = numbers.Average();
```

* `GroupBy`: Groups elements by a key

```csharp
var peopleByCity = people.GroupBy(p => p.City);

foreach (var group in peopleByCity)
{
    Console.WriteLine($"City: {group.Key}");
    foreach (var person in group)
        Console.WriteLine($" - {person.Name}");
}
```

* `ToList`, `ToArray`, `ToDictionary`

```csharp
List<int> list = numbers.ToList();
Dictionary<int, string> dict = list.ToDictionary(n => n, n => $"Number: {n}");
```

### **Real-life Example:**

Imagine you have a list of customers, and you want to find those from a particular city:

```csharp
var customersInCity = customers.Where(c => c.City == "New York").ToList();
```

This is like searching through a phone book to find people living in New York.

### **Best Practices:**

* Use LINQ for readability and maintainability.
* Prefer method syntax for complex queries.
* Use deferred execution (queries are executed only when enumerated).

LINQ simplifies data manipulation and querying, making code easier to read and maintain.

---

*Continue to Part 4: Summary & Best Practices*