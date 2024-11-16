
## [C# Advanced: Multicast Delegates](https://dev.to/moh_moh701/c-advanced-multicast-delegates-131j?bb=180937)

-----------------------------------

### Delegates and Events in C#: Behind the Scenes!
###  What is a Delegate?
A delegate in C# is like a “contract” that defines which methods can be called based on a specific signature. 

It allows you to pass methods as parameters to other functions, making the code more flexible.

## Practical Example: Restaurant Notification
Imagine you’re at a restaurant, and after placing your order, you want to be notified as soon as your food is ready. 

The restaurant (the program) doesn’t know what you’ll do with this information—maybe you’ll go pick up the food, or maybe you’ll send someone to get it. 

To make this possible, you can leave a “phone number” for them to notify you (the delegate).

This “phone number” is a delegate, allowing the restaurant to notify you in various ways, depending on the method you assign to it. Here’s how this would look in code:

```
public delegate void NotificationDelegate(string message);

public class Restaurant
{
    public void NotifyCustomer(NotificationDelegate notificationMethod)
    {
        // Some logic to prepare the order...
        notificationMethod("Your order is ready!"); // Calls the method assigned to the delegate
    }
}

public class Program
{
    public static void NotifyByPhone(string message)
    {
        Console.WriteLine("Phone call: " + message);
    }

    public static void Main()
    {
        Restaurant restaurant = new Restaurant();
        restaurant.NotifyCustomer(NotifyByPhone); // Using delegate for notification
    }
}
```
### Explanation: 
In this example, the delegate NotificationDelegate defines the “signature” for the notification method. By passing the NotifyByPhone method to NotifyCustomer, we’re telling the program how we want to be notified.

If we want another method, we just pass it without changing the restaurant’s logic.

## And What is an Event?
An event is a way for the program to notify other parts of the code when something happens, without knowing who’s “listening” in advance.

## Practical Example: Home Doorbell
Think of a doorbell: when someone presses it, the bell rings, and everyone in the house hears it. The person pressing the doorbell (the event) doesn’t know who will answer; they’re just sending the alert. In programming, the event is like the doorbell, and any method “subscribing” to that event is like a person hearing the bell.

Here’s how it would look in C#:
```
using System;

public class House
{
    public delegate void DoorbellEventHandler();
    public event DoorbellEventHandler DoorbellRang;

    public void RingDoorbell()
    {
        Console.WriteLine("Ding dong! Someone rang the doorbell.");
        DoorbellRang?.Invoke(); // Triggers the event to notify listeners
    }
}

public class Resident
{
    public void OpenDoor()
    {
        Console.WriteLine("The resident opened the door.");
    }
}

public class Program
{
    static void Main()
    {
        House house = new House();
        Resident resident = new Resident();

        // Subscribing to the DoorbellRang event with the OpenDoor method
        house.DoorbellRang += resident.OpenDoor;

        house.RingDoorbell(); // Triggers the event and calls OpenDoor
    }
}
```
Explanation: Here, we have the House class, which defines a DoorbellRang event. The Resident subscribes to this event and responds by opening the door when the doorbell rings. 

When house.RingDoorbell() is called, the DoorbellRang event triggers, and the OpenDoor method is executed.


