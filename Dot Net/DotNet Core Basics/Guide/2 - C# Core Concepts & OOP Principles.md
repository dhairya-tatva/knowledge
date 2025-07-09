# .NET Interview Guide - Part 2: C# Core Concepts & OOP Principles

## ✅ Interfaces vs Abstract Classes

| Feature     | Interface                                            | Abstract Class                         |
| ----------- | ---------------------------------------------------- | -------------------------------------- |
| Inheritance | Multiple                                             | Single                                 |
| Members     | Method Signatures Only (can have defaults in new C#) | Methods with or without implementation |
| Fields      | Not Allowed                                          | Allowed                                |
| Use Case    | Defines contract                                     | Provides base functionality            |

---

## ✅ OOP Principles in C#

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

  ### ✅ Types of Inheritance in C\#

    Inheritance allows a class to acquire members (fields, methods) of another class.

    #### **1. Single Inheritance:**

    One class inherits from a single base class.

    ```csharp
    class Animal { }
    class Dog : Animal { }
    ```

    #### **2. Hierarchical Inheritance:**

    Multiple classes inherit from a single base class.

    ```csharp
    class Animal { }
    class Dog : Animal { }
    class Cat : Animal { }
    ```

    #### **3. Multilevel Inheritance:**

    A derived class acts as a base class for another derived class.

    ```csharp
    class Animal { }
    class Mammal : Animal { }
    class Dog : Mammal { }
    ```

    #### **Note:**

    C# does not support **multiple inheritance** with classes to avoid ambiguity but supports it through **interfaces**.

    #### Example with Interface:

    ```csharp
    interface IWalk { void Walk(); }
    interface IBark { void Bark(); }

    class Dog : IWalk, IBark
    {
        public void Walk() => Console.WriteLine("Dog walking");
        public void Bark() => Console.WriteLine("Dog barking");
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

  ## ✅ Types of Polymorphism in C\#

    Polymorphism allows objects to take on many forms. In C#, it enables a single interface to represent different underlying forms (data types).

    ### **1. Compile-time Polymorphism (Static Polymorphism):**

    Achieved through **method overloading** and **operator overloading**.

    #### Method Overloading Example:

    ```csharp
    public class Calculator
    {
        public int Add(int a, int b) => a + b;
        public double Add(double a, double b) => a + b;
    }
    ```

    ---

    ### **2. Runtime Polymorphism (Dynamic Polymorphism):**

    Achieved through **method overriding** with **inheritance** and **virtual methods**.

    #### Method Overriding Example:

    ```csharp
    public class Animal
    {
        public virtual void Speak() => Console.WriteLine("Animal speaks");
    }

    public class Dog : Animal
    {
        public override void Speak() => Console.WriteLine("Dog barks");
    }

    Animal animal = new Dog();
    animal.Speak(); // Output: Dog barks
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

## ✅ Access Modifiers in C#

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

## ✅ Garbage Collection Basics

* Automatic memory management in .NET.
* Frees memory for unused objects.
* No need for manual memory allocation/deallocation.

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

*Continue to Part 3: Data Types & Language Features*