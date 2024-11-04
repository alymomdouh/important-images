
## [The Art of Inheritance: Virtual, Override, and New in Action](https://dotnet-fullstack-dev.blogspot.com/2024/10/The%20Art%20of%20Inheritance%20Virtual%20Override%20and%20New%20in%20Action.html)


![image](https://github.com/user-attachments/assets/8d7248e2-574a-4bb5-b379-6cbfbfabd0fa)

Inheritance is a core concept in C# that allows for creating more flexible and reusable code. 

When working with inheritance, three important keywords often come into play: 

virtual, override, and new. Each serves a different purpose in defining or altering the behavior of methods between base and derived classes. 

In this blog, we'll explore these keywords through a practical example, making it easy to understand their usage and impact when working with inheritance and polymorphism.

## Understanding the Core Concepts:
Before diving into the code, let's break down what these keywords do:

## 1.Virtual: 
Declares a method in the base class that can be overridden by derived classes. This allows derived classes to provide their own implementation of the method.

## 2.Override: 
Used in derived classes to override a virtual method from the base class, providing a specialized implementation for that method.

## 3.New: 
Hides a base class method in the derived class. This means that the method in the derived class replaces the base class method when accessed via the derived class, 

but it doesn't override the base class method when accessed through a base class reference.

Now that we know the basics, let's move on to a practical example that demonstrates how these keywords behave when using different object instances.

## Practical Example: Vehicles, Cars, and Trucks
Imagine we are designing a simple program to simulate vehicle behaviors. We have a base class Vehicle, and two derived classes Car and Truck. 

Each vehicle can start its engine, stop it, and fuel up. However, we want Car and Truck to have specialized behaviors for certain actions.

Let's see how we can use virtual, override, and new to achieve this.
```
using System;

public class Vehicle
{
    // Virtual method: Can be overridden in derived classes
    public virtual void StartEngine()
    {
        Console.WriteLine("Vehicle engine started.");
    }

    // Non-virtual method: Cannot be overridden
    public void StopEngine()
    {
        Console.WriteLine("Vehicle engine stopped.");
    }

    // Method hidden by the 'new' keyword
    public void FuelUp()
    {
        Console.WriteLine("Vehicle is fueling up.");
    }
}

public class Car : Vehicle
{
    // Overriding the virtual method
    public override void StartEngine()
    {
        Console.WriteLine("Car engine started.");
    }

    // Hiding the base class method using 'new'
    public new void FuelUp()
    {
        Console.WriteLine("Car is fueling up with premium fuel.");
    }
}

public class Truck : Vehicle
{
    // Overriding the virtual method
    public override void StartEngine()
    {
        Console.WriteLine("Truck engine started.");
    }

    // Hiding the FuelUp method using 'new'
    public new void FuelUp()
    {
        Console.WriteLine("Truck is fueling up with diesel.");
    }
}

public class Program
{
    public static void Main()
    {
        // Vehicle reference, Vehicle object
        Vehicle vehicle = new Vehicle();
        vehicle.StartEngine(); // Output: Vehicle engine started.
        vehicle.FuelUp();      // Output: Vehicle is fueling up.

        Console.WriteLine();

        // Vehicle reference, Car object
        Vehicle vehicleAsCar = new Car();
        vehicleAsCar.StartEngine(); // Output: Car engine started.
        vehicleAsCar.FuelUp();      // Output: Vehicle is fueling up.

        Console.WriteLine();

        // Car reference, Car object
        Car car = new Car();
        car.StartEngine();  // Output: Car engine started.
        car.FuelUp();       // Output: Car is fueling up with premium fuel.

        Console.WriteLine();

        // Vehicle reference, Truck object
        Vehicle vehicleAsTruck = new Truck();
        vehicleAsTruck.StartEngine(); // Output: Truck engine started.
        vehicleAsTruck.FuelUp();      // Output: Vehicle is fueling up.

        Console.WriteLine();

        // Truck reference, Truck object
        Truck truck = new Truck();
        truck.StartEngine();  // Output: Truck engine started.
        truck.FuelUp();       // Output: Truck is fueling up with diesel.
    }
}
```
### Breaking Down the Example:
### 1.Base Class Methods:

1.1. StartEngine is marked as virtual in Vehicle. This means derived classes (Car and Truck) can provide their own versions of this method.
1.2. FuelUp is a regular method in Vehicle, but both Car and Truck use the new keyword to hide this method and provide a more specific implementation.

## 2.Car and Truck Classes:

Both Car and Truck override the StartEngine method, meaning their specific implementations will be called when this method is invoked on their instances, even when referenced as a Vehicle.
The new keyword is used in both classes to hide the base FuelUp method. However, calling FuelUp through a Vehicle reference will still use the Vehicle version, unless the object is referenced directly as Car or Truck.
What Happens When We Run This Code?
The key part of this example is understanding how different instances behave when calling these methods.

Output:
```
Vehicle engine started.
Vehicle is fueling up.

Car engine started.
Vehicle is fueling up.

Car engine started.
Car is fueling up with premium fuel.

Truck engine started.
Vehicle is fueling up.

Truck engine started.
Truck is fueling up with diesel.
```
Let’s break it down:

1. When the object is of type Vehicle: The base class methods are called as expected.
2. When the object is of type Car but referenced as Vehicle: The overridden StartEngine method from Car is called, but FuelUp falls back to the Vehicle version because we’re using a base class reference.
3. When the object is referenced as Car: Both StartEngine and FuelUp methods from Car are used.
4. Same behavior for Truck: The overridden and hidden methods behave similarly.
## Why Does This Matter?
This example showcases how you can control method behavior in inheritance using virtual, override, and new. 

These keywords are powerful tools for managing method inheritance, giving you control over how base class methods are inherited, overridden, or hidden in derived classes.

Virtual and override enable polymorphism, where a base class reference can invoke methods of the derived class.

The new keyword, on the other hand, hides base class methods and only reveals the derived class version when accessed through a derived class reference.

## Key Takeaways:
1. Use virtual in base classes to allow derived classes to customize behavior.
2. Use override in derived classes to provide new functionality for virtual methods.
3. Use new when you want to hide the base class method in the derived class.
4. Understand how object references (base vs. derived) affect method invocation, particularly with polymorphism.
By mastering the use of these keywords, you’ll have full control over how methods behave in your class hierarchies, allowing for more flexible, maintainable, and reusable code.

## Closing Thoughts:
Understanding inheritance in C# is essential for writing clean and scalable code. With the right use of virtual, override, and new, you can define sophisticated behaviors that fit your application’s needs.

Experiment with different references and method combinations to get a feel for how these keywords work together and enhance your designs.
