

## [C# : How can we access private method outside class](https://dotnet-fullstack-dev.blogspot.com/2020/08/c-access-private-method-outside-class.html)

![image](https://github.com/user-attachments/assets/15532420-e0d5-4c5f-8443-1b5d6abf26db)

## Introduction
In object-oriented programming, encapsulation is a fundamental principle that restricts direct access to the internal implementation details of a class. 

Private methods, being part of this internal implementation, are designed to be accessible only within the confines of the class they belong to.

However, there might be scenarios where you need to access a private method from outside the class. In this blog post, we'll explore several techniques to achieve this in C#.

## 1. Reflection: A Powerful Yet Delicate Approach
Reflection is a mechanism in C# that allows inspecting and interacting with metadata about types, fields, properties, and methods. 

While it provides a way to access private methods, it should be used cautiously due to its potential impact on maintainability and performance.
```
using System;
using System.Reflection;

public class MyClass
{
    private void PrivateMethod()
    {
        Console.WriteLine("This is a private method.");
    }
}

class Program
{
    static void Main()
    {
        MyClass myInstance = new MyClass();

        // Using Reflection to access the private method
        MethodInfo privateMethod = typeof(MyClass).GetMethod("PrivateMethod", BindingFlags.NonPublic | BindingFlags.Instance);

        if (privateMethod != null)
        {
            privateMethod.Invoke(myInstance, null);
        }
    }
}
```
### 2. InternalsVisibleTo Attribute: Friend Assemblies
The InternalsVisibleTo attribute allows an assembly to expose its internal members to another specified assembly. This is often referred to as creating a "friend" assembly.
```
// In AssemblyInfo.cs of the target assembly (e.g., MyLibrary)
[assembly: InternalsVisibleTo("MyTestingAssembly")]

public class MyClass
{
    private void PrivateMethod()
    {
        Console.WriteLine("This is a private method.");
    }
}

// In the testing assembly
public class TestClass
{
    static void Main()
    {
        MyClass myInstance = new MyClass();
        myInstance.PrivateMethod(); // Accessible due to InternalsVisibleTo attribute
    }
}
```
## 3. Nested Class: A Creative Workaround
If you have control over the original class, a creative workaround is to create a nested class within the original class, allowing the outer class to access the private members of the nested class.
```
public class MyClass
{
    private class PrivateContainer
    {
        public static void PrivateMethod()
        {
            Console.WriteLine("This is a private method.");
        }
    }

    public static void AccessPrivateMethod()
    {
        PrivateContainer.PrivateMethod();
    }
}

class Program
{
    static void Main()
    {
        // Accessing private method via the outer class
        MyClass.AccessPrivateMethod();
    }
}
```
### 4. Delegates: Indirect Invocation
Using delegates, you can indirectly invoke a private method by creating a delegate within the class that points to the private method.
```
using System;

public class MyClass
{
    private void PrivateMethod()
    {
        Console.WriteLine("This is a private method.");
    }

    public Action AccessPrivateMethod;

    public MyClass()
    {
        AccessPrivateMethod = new Action(PrivateMethod);
    }
}

class Program
{
    static void Main()
    {
        MyClass myInstance = new MyClass();
        myInstance.AccessPrivateMethod(); // Indirect invocation
    }
}
```
### Conclusion
While these techniques provide ways to access private methods from outside a class, it's crucial to approach such scenarios with caution. 

Accessing private methods directly might violate the encapsulation principle, leading to increased coupling and decreased maintainability.

Before opting for these methods, consider alternative designs or refactorings to ensure that your code remains clean, understandable, and maintainable.

Remember, encapsulation is a key tenet of object-oriented design, and accessing private methods from outside the class should only be done when absolutely necessary and with a clear understanding of the consequences.

Use these techniques judiciously, keeping the overall design and maintainability of your code in mind. 

Happy coding!
