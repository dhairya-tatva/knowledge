# C# and .NET Developer Interview Guide (Junior Level)

## ✅ What is C#?

C# (pronounced "C-sharp") is a modern, object-oriented, and type-safe programming language developed by Microsoft. It is widely used for:

* Building desktop applications.
* Developing web applications and APIs.
* Creating mobile apps (via Xamarin).
* Game development (via Unity).

### **Key Features:**

* Simple and easy to learn.
* Supports object-oriented programming (OOP).
* Strongly typed with compile-time checking.
* Rich library support (via .NET).

---

## ✅ What is .NET?

.NET is a free, cross-platform, open-source development platform developed by Microsoft. It provides a runtime environment and libraries for building applications.

### **Key Features:**

* Supports multiple languages (C#, F#, VB.NET).
* Provides tools, libraries, and runtime (CLR - Common Language Runtime).
* Used for web, desktop, cloud, mobile, gaming, IoT, and AI apps.
* Includes ASP.NET Core for building modern web apps and APIs.

### **.NET Components:**

* **CLR (Common Language Runtime):** Executes code, handles memory, and more.
* **BCL (Base Class Library):** Provides essential classes for strings, files, collections, etc.
* **SDKs and Tools:** Build and manage projects.

### **Why Use .NET?**

* Cross-platform.
* High performance.
* Secure and scalable.
* Active community and support.

---

This guide is designed for junior-level C# and .NET developers preparing for interviews. It covers essential topics from the basics to foundational advanced topics required in interviews for both service-based and product-based companies.

---

## ✅ .NET Core vs .NET Framework – Detailed Description

### 🔷 Difference between .NET Core and .NET Framework

| Feature               | .NET Core                                              | .NET Framework                                           |
| --------------------- | ------------------------------------------------------ | -------------------------------------------------------- |
| Platform              | Cross-platform (Windows, Linux, macOS)                 | Windows-only                                             |
| Performance           | High performance, optimized for modern workloads       | Moderate, older architecture                             |
| App Types             | Web, console, microservices, cloud, APIs, gRPC, Blazor | Web (ASP.NET), desktop (WinForms, WPF), Windows services |
| Deployment            | Flexible (self-contained or framework-dependent)       | Machine-wide installation                                |
| Modularity            | Modular, NuGet-based packages                          | Monolithic and bulky                                     |
| Open Source           | Fully open-source (.NET Foundation)                    | Partially open-source                                    |
| CLI Support           | Powerful CLI tools (`dotnet`)                          | Limited or complex tooling                               |
| Latest Development    | Actively developed (.NET 6/7/8 and beyond)             | Legacy, only minor updates (latest is 4.8)               |
| Microservices Support | Built with containers, Docker, and microservices       | Difficult to adapt for cloud-native scenarios            |
| Cross-Compatibility   | ARM, Linux, macOS                                      | x86/x64 on Windows                                       |

### 🔷 Cross-platform Capabilities

* Designed to be platform-independent; runs on Windows, Linux, and macOS.
* Same codebase can be used across all supported platforms.
* Easily package and deploy in Docker containers for maximum portability.

### 🔷 CoreCLR and CoreFX

#### **CoreCLR**

* The engine that runs .NET Core applications.
* Responsibilities:

  * Just-In-Time (JIT) compilation: converts IL to native code.
  * Memory management (Garbage Collection).
  * Threading and task scheduling.
  * Exception handling.
  * Type safety and security enforcement.

#### **CoreFX**

* The Base Class Library for .NET Core.
* Provides commonly used libraries:

  * Collections (`List<T>`, `Dictionary<K,V>`, etc.)
  * File I/O and networking (`HttpClient`)
  * Serialization (JSON, XML)
  * LINQ, DateTime, Math, Regex, etc.

#### **How They Work Together**

```
Your Code (C#)
    ↓
Compiler → Intermediate Language (IL)
    ↓
CoreCLR → Executes the IL
    ↕️
CoreFX → Provides standard libraries
```

> **Note:** As of .NET 5+ (including .NET 6, 7, 8), the platform is unified. CoreCLR and Mono runtimes are merged, and CoreFX is part of the unified .NET Base Class Library (BCL).

| Component | Role               | .NET Framework Equivalent          |
| --------- | ------------------ | ---------------------------------- |
| CoreCLR   | Runtime engine     | CLR                                |
| CoreFX    | Standard libraries | .NET Framework Class Library (FCL) |

### 🔷 Typical Project Structure of a .NET Core Application

```
/MyApp
├── Controllers/
├── Models/
├── Services/
├── wwwroot/
├── appsettings.json
├── Program.cs
├── Startup.cs       (only in .NET Core 3.1 / 5)
├── MyApp.csproj
├── Properties/
└── obj/ & bin/
```

| File                             | Role                                              |
| -------------------------------- | ------------------------------------------------- |
| `*.csproj`                       | Project metadata and dependencies                 |
| `Program.cs`                     | Entry point and application bootstrap             |
| `Startup.cs`                     | Middleware and service configuration (pre-.NET 6) |
| `appsettings.json`               | Configuration values (e.g., connection strings)   |
| `Controllers/`                   | MVC/Web API controllers                           |
| `Models/`                        | Data models (DTOs, entities)                      |
| `Services/`                      | Business logic and DI-registered classes          |
| `wwwroot/`                       | Static files (JS, CSS, images)                    |
| `Properties/launchSettings.json` | Local debug settings                              |
| `obj/` & `bin/`                  | Build outputs (compiled code)                     |

## ✅ Program.cs and Startup.cs (ASP.NET Core Basics) (ASP.NET Core Basics)

### **Program.cs**

* Entry point of ASP.NET Core applications.
* Contains `Main` method.
* Builds and runs the web host.
* Typically uses `CreateHostBuilder` to initialize the app.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder => 
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

### **Startup.cs**

* Configures services and the app’s request pipeline.
* Has two important methods:

  * `ConfigureServices`: Registers services for Dependency Injection.
  * `Configure`: Defines how HTTP requests are handled (middleware pipeline).

---

## ✅ Dependency Injection (DI) in .NET Core

### **Purpose**

* Removes tight coupling between components.
* Makes applications easier to test and maintain.

### **Lifetimes**

* **Transient**: A new instance every time requested.

  **Real-life Example:**
  Imagine a random number generator service. Every time you ask for a number, it gives you a new random value. Each call gets a new, unrelated result.

  ```csharp
  services.AddTransient<IRandomNumberService, RandomNumberService>();
  ```

* **Scoped**: One instance per HTTP request.

  **Real-life Example:**
  Consider a shopping cart service in a web app. During a user's single visit (one HTTP request), all operations work on the same cart instance, but once the request completes, the cart data is discarded.

  ```csharp
  services.AddScoped<IShoppingCartService, ShoppingCartService>();
  ```

* **Singleton**: A single instance for the entire app lifetime.

  **Real-life Example:**
  Think of an application-wide configuration or logging service. You only need one instance of this throughout the app's lifecycle, and all components share it.

  ```csharp
  services.AddSingleton<IAppConfigService, AppConfigService>();
  ```

---

## ✅ Ways of Object Creation in C\#

* Using `new` keyword:

```csharp
var obj = new MyClass();
```

* Factory pattern (using factory methods).
* Dependency Injection (let the DI container create objects).
* Reflection (less common for interviews):

```csharp
var obj = Activator.CreateInstance(typeof(MyClass));
```

---

## ✅ Middleware (ASP.NET Core)

* Software components in the HTTP request/response pipeline.
* Each middleware can:

  * Handle requests.
  * Call the next middleware.
  * Short-circuit the pipeline.

### Example:

```csharp
app.Use(async (context, next) => {
    // Before next middleware
    await next.Invoke();
    // After next middleware
});
```

---

## ✅ Filters (ASP.NET Core MVC)

Filters allow you to execute code before or after specific stages in the MVC request pipeline. They help with cross-cutting concerns such as logging, exception handling, authorization, and response formatting.

### **Types of Filters:**

* **Authorization Filters**: Run first to check whether a user is authorized.
* **Action Filters**: Run before and after action methods. Used for logging, validation, etc.
* **Result Filters**: Run before and after action results are executed, often used to modify the response.
* **Exception Filters**: Handle unhandled exceptions from actions.

### **Example of Action Filter:**

```csharp
public class LogActionFilter : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext context)
    {
        Console.WriteLine("Action is about to execute.");
    }

    public void OnActionExecuted(ActionExecutedContext context)
    {
        Console.WriteLine("Action executed.");
    }
}
```

**Usage:**

```csharp
[ServiceFilter(typeof(LogActionFilter))]
public class HomeController : Controller
{
    public IActionResult Index()
    {
        return View();
    }
}
```

### **Real-life Example:**

Imagine an airport security checkpoint:

* **Authorization Filter**: Check ticket and ID.
* **Action Filter**: Inspect baggage and carry-ons.
* **Result Filter**: Print boarding pass after successful inspection.
* **Exception Filter**: Handle unexpected situations like system failures.

### **Key Benefits:**

* Code reuse across multiple actions or controllers.
* Cleaner and more maintainable code.
* Easy to apply cross-cutting concerns globally or at specific levels.

### **Global Filter Registration:**

```csharp
services.AddControllersWithViews(options =>
{
    options.Filters.Add(typeof(LogActionFilter));
});
```

---

## ✅ Interfaces vs Abstract Classes

| Feature     | Interface                                            | Abstract Class                         |
| ----------- | ---------------------------------------------------- | -------------------------------------- |
| Inheritance | Multiple                                             | Single                                 |
| Members     | Method Signatures Only (can have defaults in new C#) | Methods with or without implementation |
| Fields      | Not Allowed                                          | Allowed                                |
| Use Case    | Defines contract                                     | Provides base functionality            |

---

## ✅ OOP Principles in C\#

* **Encapsulation**: Hide internal details, expose via properties/methods.

  **Real-life Example:**
  Think of a television. You use buttons or a remote to control it, but you don't need to know the complex circuits inside. In C#, you may create private fields and expose them through public properties:

  ```csharp
  public class BankAccount
  {
      private decimal balance;
      public decimal Balance => balance;
      public void Deposit(decimal amount)
      {
          if (amount > 0) balance += amount;
      }
  }
  ```

* **Abstraction**: Expose only essential features.

  **Real-life Example:**
  When driving a car, you only interact with the steering wheel and pedals, not the engine internals.
  In C#:

  ```csharp
  public abstract class Vehicle
  {
      public abstract void Drive();
  }

  public class Car : Vehicle
  {
      public override void Drive()
      {
          Console.WriteLine("Driving a car");
      }
  }
  ```

* **Inheritance**: Derive new classes from base classes.

  **Real-life Example:**
  A dog and a cat are both animals but have specific behaviors.

  ```csharp
  public class Animal
  {
      public void Eat() => Console.WriteLine("Eating");
  }

  public class Dog : Animal
  {
      public void Bark() => Console.WriteLine("Barking");
  }
  ```

* **Polymorphism**: Same method behaves differently in different contexts.

  **Real-life Example:**
  The "draw" action differs for different shapes.

  ```csharp
  public class Shape
  {
      public virtual void Draw() => Console.WriteLine("Drawing shape");
  }

  public class Circle : Shape
  {
      public override void Draw() => Console.WriteLine("Drawing circle");
  }

  public class Rectangle : Shape
  {
      public override void Draw() => Console.WriteLine("Drawing rectangle");
  }
  ```

---

## ✅ Value Types vs Reference Types

* **Value Types**:

  * Directly hold data.
  * Stored in stack.
  * Examples: `int`, `float`, `bool`, `struct`.

* **Reference Types**:

  * Hold reference to data on heap.
  * Examples: `class`, `array`, `string`.

---

## ✅ Garbage Collection Basics

* Automatic memory management in .NET.
* Frees memory for unused objects.
* No need for manual memory allocation/deallocation.

---

## ✅ Async and Await (Asynchronous Programming)

Asynchronous programming allows tasks to run without blocking the main thread, improving application responsiveness and scalability.

### **Key Benefits:**

* Prevents UI freezing in desktop apps.
* Handles high I/O operations efficiently (e.g., database queries, API calls, file I/O).
* Improves application scalability.

### **How It Works:**

* **async** keyword marks a method as asynchronous.
* **await** pauses execution until the awaited task completes.

### **Basic Example:**

```csharp
public async Task MyMethodAsync()
{
    Console.WriteLine("Starting...");
    await Task.Delay(1000); // Simulates an asynchronous delay
    Console.WriteLine("Completed");
}
```

### **Real-life Example:**

Imagine ordering food at a restaurant:

* You place the order and continue chatting (non-blocking).
* The chef prepares the food in the background (background task).
* Once the food is ready, it is delivered to you (task completion).

### **Common Use Case Example (API Call):**

```csharp
public async Task<string> FetchDataAsync()
{
    using (HttpClient client = new HttpClient())
    {
        string result = await client.GetStringAsync("https://api.example.com/data");
        return result;
    }
}
```

### **Important Points:**

* Avoid using `async void` unless for event handlers.
* Always await asynchronous calls to avoid unobserved exceptions.
* Methods with `async` must return `Task`, `Task<T>`, or `void` (only for events).

### **Best Practices:**

* Prefer `ConfigureAwait(false)` in library code to avoid deadlocks in certain contexts:

```csharp
await Task.Delay(1000).ConfigureAwait(false);
```

* Avoid blocking async methods with `.Result` or `.Wait()`. Always use `await`.

Async/await simplifies writing asynchronous code and helps avoid complex callbacks or manual thread management.

---

## ✅ Access Modifiers in C\#

Access modifiers control the visibility and accessibility of classes, methods, properties, and other members.

### **1. public**

* Accessible from anywhere, inside or outside the assembly.
* Typically used for API classes and methods intended for public use.

**Example:**

```csharp
public class Calculator
{
    public int Add(int a, int b) => a + b;
}
```

### **2. private**

* Accessible only within the same class.
* Used to encapsulate internal details.

**Example:**

```csharp
public class BankAccount
{
    private decimal balance;
    public void Deposit(decimal amount)
    {
        balance += amount;
    }
}
```

### **3. protected**

* Accessible within the same class and derived classes.
* Used to allow child classes to reuse or override members.

**Example:**

```csharp
public class Animal
{
    protected void Breathe()
    {
        Console.WriteLine("Breathing");
    }
}

public class Dog : Animal
{
    public void StartBreathing()
    {
        Breathe();
    }
}
```

### **4. internal**

* Accessible within the same assembly (project).
* Useful for grouping functionality within a project while hiding it from external consumers.

**Example:**

```csharp
internal class Helper
{
    public void Assist() => Console.WriteLine("Assisting");
}
```

### **5. protected internal**

* Accessible within the same assembly **or** in derived classes in other assemblies.

**Example:**

```csharp
public class BaseClass
{
    protected internal void Display()
    {
        Console.WriteLine("Display called");
    }
}
```

### **Real-life Example Analogy:**

Think of an office building:

* **public**: Reception area - accessible to everyone.
* **private**: Manager's personal office - only the manager has access.
* **protected**: Restricted meeting room - accessible by certain staff members (derived classes).
* **internal**: Employee-only areas - accessible to employees of the same company (assembly).
* **protected internal**: Shared resources accessible to employees **and** authorized external partners (derived classes from outside).

### **Best Practice:**

* Use the most restrictive access modifier possible.
* Start with `private` and increase accessibility only as needed to reduce unintended usage and maintain encapsulation.

---

## ✅ Exception Handling

Exception handling allows you to gracefully handle errors or unexpected situations in your application.

### **Basic Structure:**

```csharp
try
{
    // Code that may cause an exception
}
catch (Exception ex)
{
    // Code to handle the exception
}
finally
{
    // Code that always executes (optional)
}
```

### **Key Components:**

* **try**: Contains the code that might throw an exception.
* **catch**: Handles the exception and provides error recovery.
* **finally**: Executes cleanup code regardless of whether an exception occurred.

### **Example:**

```csharp
try
{
    int[] numbers = { 1, 2, 3 };
    Console.WriteLine(numbers[5]); // Index out of range
}
catch (IndexOutOfRangeException ex)
{
    Console.WriteLine("An error occurred: " + ex.Message);
}
finally
{
    Console.WriteLine("Execution finished.");
}
```

### **Real-life Example:**

Imagine a payment system:

* **try**: Attempt to process the payment.
* **catch**: If payment fails (e.g., insufficient funds), notify the user.
* **finally**: Log the transaction attempt, whether it succeeded or failed.

### **Best Practices:**

* Catch specific exceptions before generic ones.
* Avoid catching general `Exception` unless necessary.
* Use `finally` to release resources (like closing a file or database connection).
* Avoid swallowing exceptions silently; always log them.

### **Multiple Catch Blocks:**

```csharp
try
{
    // Risky operation
}
catch (IOException ex)
{
    // Handle file-related errors
}
catch (UnauthorizedAccessException ex)
{
    // Handle permission errors
}
catch (Exception ex)
{
    // Handle all other exceptions
}
```

This ensures your app can respond appropriately to different types of errors.

---

## ✅ Constructors and Static Constructors

### **Constructors**

* Called automatically when an object is created.
* Used to initialize fields or execute startup logic.
* Can be default (no parameters) or parameterized.

**Example:**

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    // Default Constructor
    public Person()
    {
        Name = "Unknown";
        Age = 0;
    }

    // Parameterized Constructor
    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }
}

var person1 = new Person(); // Uses default constructor
var person2 = new Person("John", 25); // Uses parameterized constructor
```

**Real-life Example:**
Imagine filling out a form:

* Default constructor creates a blank form.
* Parameterized constructor pre-fills form fields with existing data.

---

### **Static Constructors**

* Used to initialize static fields or perform actions that need to run only once.
* Cannot take parameters.
* Called automatically before the first instance is created or any static member is accessed.

**Example:**

```csharp
public class Configuration
{
    public static string AppName;

    // Static Constructor
    static Configuration()
    {
        AppName = "My Application";
        Console.WriteLine("Static constructor called");
    }
}

Console.WriteLine(Configuration.AppName); // Triggers static constructor
```

**Real-life Example:**
Think of loading an app's settings from a configuration file. This only needs to happen once for the entire application.

---

---

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

### **Common LINQ Methods:**

* `Where`: Filters elements.
* `Select`: Projects/Transforms elements.
* `OrderBy` / `OrderByDescending`: Sorts elements.
* `GroupBy`: Groups elements.
* `Sum`, `Count`, `Average`: Aggregate operations.

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

# ✅ Conclusion

This guide covers the most commonly asked foundational topics for junior-level .NET interviews. Review each section carefully and practice with code examples to solidify your understanding.

---
