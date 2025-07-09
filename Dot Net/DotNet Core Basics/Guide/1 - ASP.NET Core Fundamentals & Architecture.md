# .NET Interview Guide - Part 1: ASP.NET Core Fundamentals & Architecture

## ✅ Program.cs and Startup.cs (ASP.NET Core Basics)

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

* Configures services and the app's request pipeline.
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

## ✅ Ways of Object Creation in C#

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

*Continue to Part 2: C# Core Concepts & OOP Principles*