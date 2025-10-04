Great idea! Here’s a **structured syllabus for learning C# from scratch**, designed for someone with programming experience (like you). This syllabus is modular, so you can track your progress and go step by step.

---

# **C# Learning Syllabus**

## **Module 1: Introduction to C#**
- What is C#? History and Ecosystem
- Setting up the Development Environment (Visual Studio/VS Code)
- Your First C# Program: “Hello, World!”

---

## **Module 2: C# Basics**
- Syntax and Structure
- Variables and Data Types
- Constants and Enumerations
- Comments and Code Organization

---

## **Module 3: Operators and Expressions**
- Arithmetic, Assignment, Comparison, Logical Operators
- Type Casting and Conversion

---

## **Module 4: Control Flow**
- Conditional Statements (`if`, `else`, `switch`)
- Loops (`for`, `while`, `do-while`, `foreach`)
- Jump Statements (`break`, `continue`, `return`)

---

## **Module 5: Methods and Functions**
- Defining and Calling Methods
- Method Parameters and Return Types
- Method Overloading
- Scope and Lifetime of Variables

---

## **Module 6: Object-Oriented Programming**
- Classes and Objects
- Fields, Properties, and Methods
- Constructors and Destructors
- Access Modifiers (`public`, `private`, etc.)
- Static Members
- Inheritance
- Polymorphism (Method Overriding, Virtual/Override)
- Abstract Classes and Interfaces

---

## **Module 7: Collections and Data Structures**
- Arrays
- Lists, Dictionaries, and Other Collections (`List<T>`, `Dictionary<K,V>`)
- Iterating Collections

---

## **Module 8: Exception Handling**
- Try-Catch-Finally Blocks
- Throwing Exceptions
- Custom Exceptions

---

## **Module 9: Advanced Topics**
- Delegates and Events
- Lambda Expressions
- LINQ (Language Integrated Query)
- Generics

---

## **Module 10: File I/O and Serialization**
- Reading and Writing Files
- Working with JSON/XML

---

## **Module 11: Introduction to .NET Libraries**
- Using Built-in Libraries (DateTime, Math, etc.)
- NuGet Packages

---

## **Module 12: Practical Projects**
- Console Applications
- Mini Projects (Calculator, To-Do List, etc.)

---

## **Module 13: Next Steps**
- Introduction to GUI (Windows Forms/WPF) or Web (ASP.NET Core)
- Best Practices and Coding Standards
- Resources for Further Learning

---

### **How to Use This Syllabus**
- **Go module by module.**
- Practice with small exercises after each topic.
- Build mini-projects as you progress.

---

Excellent! Let’s begin with **Module 2: C# Basics**.  
This module covers the essential building blocks you need to write and understand C# programs.

---

# **Module 2: C# Basics**

## **2.1 Syntax and Structure**

### **Basic Structure of a C# Program**
```csharp
using System; // Importing namespaces

class Program // Class definition
{
    static void Main() // Main method: entry point
    {
        // Your code goes here
        Console.WriteLine("Hello, World!");
    }
}
```
- Every C# program must have a `Main` method.
- Statements end with a semicolon `;`.
- Code blocks are enclosed in `{}`.

---

## **2.2 Variables and Data Types**

### **Declaring Variables**
```csharp
int age = 25;
double salary = 50000.50;
string name = "Sammitra";
bool isEmployed = true;
char grade = 'A';
```
- **Type** comes first, then the variable name, then the value.
- C# is **case-sensitive**.

### **Common Data Types**
| Type     | Example         | Description                |
|----------|-----------------|----------------------------|
| int      | `int x = 10;`   | Integer numbers            |
| double   | `double d = 3.5;` | Double-precision floating point |
| float    | `float f = 2.5f;` | Single-precision floating point (add `f` at the end) |
| string   | `string s = "Hi";` | Text (sequence of characters) |
| bool     | `bool b = true;` | Boolean (`true` or `false`) |
| char     | `char c = 'A';` | Single character           |

### **Constants**
```csharp
const double PI = 3.14159;
```
- Use `const` for values that never change.

---

## **2.3 Enumerations**
```csharp
using System;

public class HelloWorld
{
    enum Day { Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday };
    public static void Main(string[] args)
    {
        Day d1 = (Day)Enum.Parse(typeof(Day), "Friday");
        Console.WriteLine (d1);
        
        foreach (Day d in Enum.GetValues(typeof(Day)))
        {
            Console.WriteLine(d);
        }
        
    }
}

```
- `enum` defines a set of named constants.

---

## **2.4 Comments and Code Organization**
- **Single-line comment:** `// This is a comment`
- **Multi-line comment:**
  ```csharp
  /* This is a
     multi-line comment */
  ```

---

## **Practice Exercise**
Try writing a simple C# program that:
- Declares variables of different types
- Assigns values to them
- Prints them to the console

Awesome! Let’s move on to **Module 3: Operators and Expressions**.

---

# **Module 3: Operators and Expressions**

This module will cover:

1. **Arithmetic Operators**
2. **Assignment Operators**
3. **Comparison (Relational) Operators**
4. **Logical Operators**
5. **Other Useful Operators**
6. **Type Casting and Conversion**
7. **Operator Precedence**

---

## **3.1 Arithmetic Operators**

| Operator | Description        | Example (`a = 10, b = 3`) | Result   |
|----------|--------------------|---------------------------|----------|
| `+`      | Addition           | `a + b`                   | `13`     |
| `-`      | Subtraction        | `a - b`                   | `7`      |
| `*`      | Multiplication     | `a * b`                   | `30`     |
| `/`      | Division           | `a / b`                   | `3`      |
| `%`      | Modulus (remainder)| `a % b`                   | `1`      |
| `++`     | Increment          | `a++`                     | `11`     |
| `--`     | Decrement          | `a--`                     | `9`      |

---

## **3.2 Assignment Operators**

| Operator | Example      | Equivalent To      |
|----------|-------------|--------------------|
| `=`      | `a = 5`     | Assign 5 to a      |
| `+=`     | `a += 2`    | `a = a + 2`        |
| `-=`     | `a -= 2`    | `a = a - 2`        |
| `*=`     | `a *= 2`    | `a = a * 2`        |
| `/=`     | `a /= 2`    | `a = a / 2`        |
| `%=`     | `a %= 2`    | `a = a % 2`        |

---

## **3.3 Comparison (Relational) Operators**

| Operator | Description         | Example (`a = 10, b = 3`) | Result   |
|----------|---------------------|---------------------------|----------|
| `==`     | Equal to            | `a == b`                  | `false`  |
| `!=`     | Not equal to        | `a != b`                  | `true`   |
| `>`      | Greater than        | `a > b`                   | `true`   |
| `<`      | Less than           | `a < b`                   | `false`  |
| `>=`     | Greater or equal    | `a >= b`                  | `true`   |
| `<=`     | Less or equal       | `a <= b`                  | `false`  |

---

## **3.4 Logical Operators**

| Operator | Description         | Example (`x = true, y = false`) | Result   |
|----------|---------------------|----------------------------------|----------|
| `&&`     | Logical AND         | `x && y`                        | `false`  |
| `||`     | Logical OR          | `x || y`                        | `true`   |
| `!`      | Logical NOT         | `!x`                            | `false`  |

---

## **3.5 Other Useful Operators**

- **Ternary Operator:**  
  `condition ? value_if_true : value_if_false`
  ```csharp
  int max = (a > b) ? a : b;
  ```
- **Null-Coalescing Operator (`??`):**  
  Returns the left operand if not null, otherwise right operand.
  ```csharp
  string name = inputName ?? "Default";
  ```
- **Member Access (`.`):**  
  Access members of a class or struct.
  ```csharp
  Console.WriteLine("Hello");
  ```

---

## **3.6 Type Casting and Conversion**

- **Implicit Casting:**  
  Automatically done when no data will be lost.
  ```csharp
  int a = 10;
  double b = a; // OK
  ```
- **Explicit Casting:**  
  Required when data might be lost.
  ```csharp
  double x = 9.8;
  int y = (int)x; // y = 9
  ```

- **Convert Class:**  
  For safe conversions.
  ```csharp
  string s = "123";
  int n = Convert.ToInt32(s);
  ```

---

## **3.7 Operator Precedence**

- Multiplication/division/modulus have higher precedence than addition/subtraction.
- Use parentheses `()` to control order.

---

## **Practice Exercise**

Would you like to:
- See code examples for each operator?
- Try a set of practice problems?
- Or do you want to move to control flow after this?

Let’s begin **Module 4: Control Flow**—a crucial part of programming that lets you make decisions and repeat actions in your code.

---

# **Module 4: Control Flow**
everything is same as CPP
### **1. if Statement**

### **2. if-else Statement**

### **3. else if Ladder**

### **4. switch Statement**

## **4.2 Loops**

### **1. for Loop**

### **2. while Loop**

### **3. do-while Loop**

### **4. foreach Loop**
Iterate over each element in a collection (like an array or list).
```csharp
string[] names = { "Sammitra", "Pallavi", "Florian" };
foreach (string name in names)
{
    Console.WriteLine(name);
}
```

---

## **4.3 Jump Statements**

- **break:** Exit a loop or switch.
- **continue:** Skip to the next iteration of a loop.
- **return:** Exit a method and optionally return a value.

---

## **Practice Exercise**
Would you like to:
- See more detailed examples for any of these?
- Try some practice problems?
- Or move to the next module after this?

---

### 📘 **Module 5: Methods and Functions**

#### 🔹 **1. Defining and Calling Methods**
- Syntax:
  ```csharp
  public void SayHello() {
      Console.WriteLine("Hello!");
  }
  SayHello(); // Method call
  ```

#### 🔹 **2. Method Parameters and Return Types**
- With parameters:
  ```csharp
  public int Add(int a, int b) {
      return a + b;
  }
  int result = Add(5, 3);
  ```

#### 🔹 **3. Method Overloading**
- Same method name, different parameters:
  ```csharp
  public void Print(string msg) {
      Console.WriteLine(msg);
  }

  public void Print(int number) {
      Console.WriteLine(number);
  }
  ```

#### 🔹 **4. Scope and Lifetime of Variables**
- **Local variables** exist only within the method.
- **Global/static variables** persist across method calls.

---
Great question, Sammitra! Understanding the difference between **static** and **non-static methods** is key to mastering C# and object-oriented programming.

---

### 🔹 **5. Static Methods**

- Belong to the **class itself**, not to any specific object.
- Can be called **without creating an instance** of the class.
- Can **only access other static members** (fields, methods) of the class.

#### ✅ Example:
```csharp
public class MathUtils {
    public static int Square(int x) {
        return x * x;
    }
}

// Usage
int result = MathUtils.Square(5); // No object needed
```

---

### 🔹 **6. Non-Static Methods**

- Belong to an **instance of the class**.
- Require you to **create an object** of the class to call them.
- Can access both **static and non-static members**.

#### ✅ Example:
```csharp
public class Printer {
    public void PrintMessage(string msg) {
        Console.WriteLine(msg);
    }
}

// Usage
Printer p = new Printer();
p.PrintMessage("Hello!");
```

---

### 🔍 **Key Differences**

| Feature              | Static Method                  | Non-Static Method              |
|----------------------|--------------------------------|-------------------------------|
| Belongs to           | Class                          | Object (instance of class)    |
| Requires object?     | ❌ No                          | ✅ Yes                         |
| Can access static members? | ✅ Yes                  | ✅ Yes                         |
| Can access instance members? | ❌ No                | ✅ Yes                         |

---

## 🧠 **Module 6: Object-Oriented Programming (OOP)**

### 🔹 1. **Classes and Objects**
- A **class** is a blueprint for creating objects.
- An **object** is an instance of a class.

**Example:**
```csharp
public class Car
{
    public string brand;
    public int speed;

    public void Drive()
    {
        Console.WriteLine($"{brand} is driving at {speed} km/h.");
    }
}
```

**Usage:**
```csharp
Car myCar = new Car();
myCar.brand = "Toyota";
myCar.speed = 80;
myCar.Drive();
```

---

### 🔹 2. **Constructors**
- A **constructor** is a special method that runs when an object is created.
- Used to initialize fields.

**Example:**
```csharp
public class Person
{
    public string name;
    public int age;

    public Person(string name, int age)
    {
        this.name = name;
        this.age = age;
    }
}
```

---

### 🔹 3. **Private Fields and Properties**
- Fields can be made `private` to restrict direct access.
- Use **properties** to safely expose and control access.

**Example:**
```csharp
public class Person
{
    private string name;
    private int age;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public int Age
    {
        get { return age; }
        set { if (value >= 0) age = value; }
    }
}
```

---

### 🔹 4. **Constructor + Properties Together**
- You can use **auto-implemented properties** with constructors for cleaner code.

**Example:**
```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }

    public void Introduce()
    {
        Console.WriteLine($"Hi, I'm {Name} and I'm {Age} years old.");
    }
}
```
Great idea, Sammitra! Understanding the difference between **Fields**, **Properties**, and **Methods** is key to mastering object-oriented programming in C#. Here's a clear breakdown:

---

## 🔍 **Fields vs Properties vs Methods in C#**

### 🔹 1. **Fields**
- **Definition**: Variables declared inside a class to store data.
- **Access**: Can be `public`, `private`, etc., but usually kept `private` for encapsulation.
- **Purpose**: Hold the actual data/state of an object.

**Example:**
```csharp
private string name;
```

---

### 🔹 2. **Properties**
- **Definition**: Members that provide controlled access to fields.
- **Access**: Usually `public`, with `get` and `set` accessors.
- **Purpose**: Encapsulate fields and allow validation or logic during access.

**Example:**
```csharp
public string Name
{
    get { return name; }
    set { name = value; }
}
```

Or using auto-implemented properties:
```csharp
public string Name { get; set; }
```

---

### 🔹 3. **Methods**
- **Definition**: Functions defined inside a class that perform actions.
- **Access**: Can be `public`, `private`, etc.
- **Purpose**: Define behavior or operations related to the object.

**Example:**
```csharp
public void Introduce()
{
    Console.WriteLine($"Hi, I'm {Name}.");
}
```
---

### 🔹 What is Inheritance?

Inheritance allows a class (called the **derived class**) to inherit members (fields, methods, properties) from another class (called the **base class**). This promotes **code reuse** and establishes a **hierarchical relationship** between classes.

---

### 🔹 Basic Syntax

```csharp
// Base class
public class Animal
{
    public void Eat()
    {
        Console.WriteLine("This animal eats food.");
    }
}

// Derived class
public class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("The dog barks.");
    }
}
```

**Usage:**

```csharp
Dog myDog = new Dog();
myDog.Eat();  // Inherited from Animal
myDog.Bark(); // Defined in Dog
```

---

### 🔹 Key Concepts

- **Single Inheritance**: C# supports single inheritance (a class can inherit from only one base class).
- **`base` keyword**: Used to access members of the base class from the derived class.
- **Access Modifiers**: Only `public`, `protected`, and `internal` members are accessible in derived classes.
- **Method Overriding**: You can override base class methods using `virtual` and `override`.

---

### 🔹 Example with Overriding

```csharp
public class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Some generic animal sound");
    }
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Dog barks");
    }
}
```
---

### 🔷 What is Polymorphism?

**Polymorphism** means *many forms*. In C#, it allows objects to be treated as instances of their parent class rather than their actual class. This enables **method overriding** and **dynamic method invocation**.

There are two main types:

1. **Compile-time Polymorphism** (Method Overloading)
2. **Run-time Polymorphism** (Method Overriding using `virtual` and `override`)

---

### ✅ 1. Compile-time Polymorphism (Method Overloading)

This occurs when multiple methods have the same name but different parameters.

```csharp
class Calculator {
    public int Add(int a, int b) {
        return a + b;
    }

    public double Add(double a, double b) {
        return a + b;
    }
}
```

Here, `Add` is overloaded based on parameter types.

---

### ✅ 2. Run-time Polymorphism (Method Overriding)

This is more powerful and is achieved using **inheritance** and the `virtual` / `override` keywords.

#### 🔹 Example:

```csharp
class Animal {
    public virtual void Speak() {
        Console.WriteLine("Animal speaks");
    }
}

class Dog : Animal {
    public override void Speak() {
        Console.WriteLine("Dog barks");
    }
}

class Cat : Animal {
    public override void Speak() {
        Console.WriteLine("Cat meows");
    }
}
```

#### 🔹 Usage:

```csharp
Animal myAnimal = new Dog();
myAnimal.Speak();  // Output: Dog barks

myAnimal = new Cat();
myAnimal.Speak();  // Output: Cat meows
```

Even though `myAnimal` is of type `Animal`, the correct overridden method is called at runtime.

---

## 🔷 Abstract Classes — Deep Dive

An **abstract class** is a blueprint for other classes. It allows you to define **common behavior** while leaving some methods to be implemented by derived classes.

### ✅ Key Features:
- Can have **abstract methods** (no body) and **concrete methods** (with body).
- Can contain **fields**, **properties**, and **constructors**.
- Cannot be instantiated directly.
- Supports **inheritance** (only one abstract class can be inherited).

### 🧠 Example:

```csharp
abstract class Vehicle
{
    public int Speed;

    public abstract void Start(); // Must be implemented by derived class

    public void Stop()
    {
        Console.WriteLine("Vehicle stopped.");
    }
}

class Car : Vehicle
{
    public override void Start()
    {
        Console.WriteLine("Car started.");
    }
}
```

### 🔍 Use Case:
Use abstract classes when:
- You want to provide **default behavior**.
- You want to **enforce a contract** but also allow shared code.
- You expect **closely related classes** (e.g., `Vehicle`, `Car`, `Bike`).

---

## 🔷 Interfaces — Deep Dive

An **interface** defines a **contract** that classes must follow. It’s purely abstract — no implementation allowed (until C# 8.0, which introduced default interface methods).

### ✅ Key Features:
- Only contains **method signatures**, **properties**, **events**, and **indexers**.
- No fields or constructors.
- A class can implement **multiple interfaces**.
- Promotes **loose coupling** and **testability**.

### 🧠 Example:

```csharp
interface IDriveable
{
    void Drive();
}

interface IRefuelable
{
    void Refuel();
}

class Truck : IDriveable, IRefuelable
{
    public void Drive()
    {
        Console.WriteLine("Truck is driving.");
    }

    public void Refuel()
    {
        Console.WriteLine("Truck is refueling.");
    }
}
```

### 🔍 Use Case:
Use interfaces when:
- You want to define **capabilities** (e.g., `IDrawable`, `ISavable`).
- You need **multiple inheritance**.
- You want to **decouple** your code for better testing and flexibility.

---

## 🔄 Abstract Class vs Interface — Detailed Comparison

| Feature                     | Abstract Class                      | Interface                          |
|----------------------------|-------------------------------------|------------------------------------|
| Inheritance                | Single                              | Multiple                           |
| Method Implementation      | Yes                                 | No (until C# 8 default methods)    |
| Fields                     | Yes                                 | No                                 |
| Constructors               | Yes                                 | No                                 |
| Access Modifiers           | Allowed                             | Not allowed (everything is public)|
| Use Case                   | Shared base logic                   | Capability contract                |
| Versioning Flexibility     | Less flexible                       | More flexible                      |

---

## 🧪 Real-World Analogy

- **Abstract Class**: Think of it like a **template** for a family of objects. For example, `Animal` is an abstract class — all animals sleep, but each makes a different sound.
- **Interface**: Think of it like a **role or skill**. For example, `IFlyable` — birds, planes, and drones can all fly, but they’re not related by inheritance.

---

Awesome, Sammitra! Module 7 — **Collections and Data Structures** — is a crucial part of C# programming. It helps you manage and organize data efficiently.

---
## 🔷 Collections and Data Structures in C#

Perfect! Here's a clear explanation of each **C# data structure** you mentioned, along with simple examples to help you understand how they work and when to use them:

---

### 🔹 **1. Arrays (`int[]`, `string[]`, etc.)**
Arrays are fixed-size collections of elements of the same type.

```csharp
int[] numbers = { 10, 20, 30 };
foreach (int num in numbers)
{
    Console.WriteLine(num);
}
```

✅ **Use when**: You know the number of elements in advance and need fast access by index.

---

### 🔹 **2. List (`List<T>`)**
A dynamic array that can grow or shrink.

```csharp
List<string> fruits = new List<string>();
fruits.Add("Apple");
fruits.Add("Banana");

foreach (string fruit in fruits)
{
    Console.WriteLine(fruit);
}
```

✅ **Use when**: You need a resizable collection with fast access and iteration.

---

### 🔹 **3. Dictionary (`Dictionary<K,V>`)**
Stores key-value pairs for fast lookup.

```csharp
Dictionary<string, int> ages = new Dictionary<string, int>();
ages["Sammitra"] = 28;
ages["Pallavi"] = 35;

foreach (var pair in ages)
{
    Console.WriteLine($"{pair.Key}: {pair.Value}");
}
```

✅ **Use when**: You need to associate values with unique keys.

---

### 🔹 **4. Queue (`Queue<T>`)**
First-In-First-Out (FIFO) collection.

```csharp
Queue<string> tasks = new Queue<string>();
tasks.Enqueue("Task 1");
tasks.Enqueue("Task 2");

while (tasks.Count > 0)
{
    Console.WriteLine(tasks.Dequeue());
}
```

✅ **Use when**: You need to process items in the order they were added.

---

### 🔹 **5. Stack (`Stack<T>`)**
Last-In-First-Out (LIFO) collection.

```csharp
Stack<string> history = new Stack<string>();
history.Push("Page 1");
history.Push("Page 2");

while (history.Count > 0)
{
    Console.WriteLine(history.Pop());
}
```

✅ **Use when**: You need to reverse order or backtrack (e.g., undo functionality).

---

### 🔹 **6. HashSet (`HashSet<T>`)**
Stores unique elements with fast lookup.

```csharp
HashSet<string> uniqueNames = new HashSet<string>();
uniqueNames.Add("Sammitra");
uniqueNames.Add("Sammitra"); // Duplicate, won't be added

foreach (string name in uniqueNames)
{
    Console.WriteLine(name);
}
```

✅ **Use when**: You need to store unique items and check existence quickly.

---

### 🔹 **7. LinkedList (`LinkedList<T>`)**
Doubly linked list for efficient insertions/removals.

```csharp
LinkedList<string> playlist = new LinkedList<string>();
playlist.AddLast("Song A");
playlist.AddLast("Song B");
playlist.AddFirst("Intro");

foreach (string song in playlist)
{
    Console.WriteLine(song);
}
```

✅ **Use when**: You need frequent insertions/removals from both ends or in the middle.

---
Awesome! Here's a **small C# console project** that lets you practice all the major data structures you've learned:

---

## 🧪 **Project: Task Manager Console App**

### 🔹 **Goal**:
Build a simple console app to manage tasks using different data structures:
- Store tasks in a `List<string>`
- Track completed tasks in a `HashSet<string>`
- Use a `Queue<string>` for upcoming tasks
- Use a `Stack<string>` for undo history
- Use a `Dictionary<string, string>` to store task descriptions
- Use a `LinkedList<string>` to maintain task order

---

### 🔹 **Features**:
1. **Add a task**
2. **Mark a task as completed**
3. **View all tasks**
4. **Undo last action**
5. **View upcoming tasks**
6. **View completed tasks**
7. **Exit**

---

### 🔹 **Sample Code Structure**:

```csharp
using System;
using System.Collections.Generic;

class TaskManager
{
    static List<string> tasks = new List<string>();
    static HashSet<string> completedTasks = new HashSet<string>();
    static Queue<string> upcomingTasks = new Queue<string>();
    static Stack<string> undoStack = new Stack<string>();
    static Dictionary<string, string> taskDescriptions = new Dictionary<string, string>();
    static LinkedList<string> taskOrder = new LinkedList<string>();

    static void Main()
    {
        while (true)
        {
            Console.WriteLine("\n--- Task Manager ---");
            Console.WriteLine("1. Add Task");
            Console.WriteLine("2. Complete Task");
            Console.WriteLine("3. View All Tasks");
            Console.WriteLine("4. Undo Last Action");
            Console.WriteLine("5. View Upcoming Tasks");
            Console.WriteLine("6. View Completed Tasks");
            Console.WriteLine("7. Exit");
            Console.Write("Choose: ");
            string choice = Console.ReadLine();

            switch (choice)
            {
                case "1": AddTask(); break;
                case "2": CompleteTask(); break;
                case "3": ViewTasks(); break;
                case "4": Undo(); break;
                case "5": ViewUpcoming(); break;
                case "6": ViewCompleted(); break;
                case "7": return;
                default: Console.WriteLine("Invalid choice."); break;
            }
        }
    }

    static void AddTask()
    {
        Console.Write("Enter task name: ");
        string name = Console.ReadLine();
        Console.Write("Enter description: ");
        string desc = Console.ReadLine();

        tasks.Add(name);
        upcomingTasks.Enqueue(name);
        taskDescriptions[name] = desc;
        taskOrder.AddLast(name);
        undoStack.Push($"add:{name}");

        Console.WriteLine("Task added.");
    }

    static void CompleteTask()
    {
        Console.Write("Enter task name to complete: ");
        string name = Console.ReadLine();

        if (tasks.Contains(name))
        {
            completedTasks.Add(name);
            undoStack.Push($"complete:{name}");
            Console.WriteLine("Task marked as completed.");
        }
        else
        {
            Console.WriteLine("Task not found.");
        }
    }

    static void ViewTasks()
    {
        Console.WriteLine("\nAll Tasks:");
        foreach (var task in taskOrder)
        {
            Console.WriteLine($"- {task}: {taskDescriptions[task]}");
        }
    }

    static void Undo()
    {
        if (undoStack.Count == 0)
        {
            Console.WriteLine("Nothing to undo.");
            return;
        }

        string action = undoStack.Pop();
        string[] parts = action.Split(':');
        string type = parts[0];
        string name = parts[1];

        if (type == "add")
        {
            tasks.Remove(name);
            upcomingTasks = new Queue<string>(upcomingTasks.Where(t => t != name));
            taskDescriptions.Remove(name);
            taskOrder.Remove(name);
            Console.WriteLine($"Undid adding task: {name}");
        }
        else if (type == "complete")
        {
            completedTasks.Remove(name);
            Console.WriteLine($"Undid completing task: {name}");
        }
    }

    static void ViewUpcoming()
    {
        Console.WriteLine("\nUpcoming Tasks:");
        foreach (var task in upcomingTasks)
        {
            Console.WriteLine($"- {task}");
        }
    }

    static void ViewCompleted()
    {
        Console.WriteLine("\nCompleted Tasks:");
        foreach (var task in completedTasks)
        {
            Console.WriteLine($"- {task}");
        }
    }
}
```

---

### ✅ **What You'll Practice**:
- Adding/removing items from collections
- Iterating over collections
- Using different data structures for specific purposes
- Undo functionality with `Stack`
- Task ordering with `LinkedList`

---
Perfect 👍 Let’s dive into **Module 8: Exception Handling**.

---

# 📘 Module 8: Exception Handling in C#

### 🔹 1. What are Exceptions?

* Exceptions represent **runtime errors** (like dividing by zero, invalid file path, accessing a null object).
* In C#, exceptions are **objects derived from `System.Exception`**.

Example of a crash without handling:

```csharp
int x = 10, y = 0;
int result = x / y; // ❌ This will throw DivideByZeroException
```

---

### 🔹 2. Try–Catch–Finally

You can handle exceptions using `try`, `catch`, and `finally`.

```csharp
try
{
    int x = 10, y = 0;
    int result = x / y;
    Console.WriteLine(result);
}
catch (DivideByZeroException ex)
{
    Console.WriteLine("You can't divide by zero!");
}
catch (Exception ex) // generic catch for other errors
{
    Console.WriteLine("Something went wrong: " + ex.Message);
}
finally
{
    Console.WriteLine("This block always runs (cleanup code).");
}
```

---

### 🔹 3. Throwing Exceptions

You can raise your own exception when something goes wrong.

```csharp
static void Withdraw(int amount)
{
    if (amount > 1000)
        throw new InvalidOperationException("Withdrawal limit exceeded!");
    Console.WriteLine("Withdrawal successful: " + amount);
}
```

---

### 🔹 4. Custom Exceptions

You can define your own exception classes (useful for big projects).

```csharp
class TaskNotFoundException : Exception
{
    public TaskNotFoundException(string message) : base(message) { }
}
```

Usage:

```csharp
throw new TaskNotFoundException("The task you are looking for does not exist!");
```

---

### 🔹 5. Best Practices

* ✅ Catch **specific exceptions** first (`FileNotFoundException`, `DivideByZeroException`, etc.).
* ✅ Use `finally` for cleanup (closing files, releasing resources).
* ✅ Avoid catching `Exception` unless you re-throw or log it.
* ✅ Use custom exceptions for **domain-specific errors** (like `TaskNotFoundException`).

---

### 🔹 Mini Practice (Try Now)

Write a console app that:

1. Asks the user to enter two numbers.
2. Divides them and prints the result.
3. If the user enters `0` as the second number, handle it gracefully with `DivideByZeroException`.
4. If the user enters something that’s not a number, handle it with `FormatException`.

---


