

##  [Understanding the State Design Pattern P1](https://dev.to/moh_moh701/understanding-the-state-design-pattern-p1-52m5)

## Meta Description: 
Learn how to implement the State Design Pattern in C# with a step-by-step guide and detailed example. 

This article explains how to manage an object's behavior based on its state, encapsulating state-specific logic for maintainable and clean code.

## Introduction
The State Design Pattern is a behavioral design pattern that allows an object to alter its behavior when its internal state changes. It’s a way of implementing state machines and helps keep code organized by encapsulating state-specific behaviors in separate classes.

This pattern is particularly useful in scenarios where an object’s behavior depends on its current state, such as a vending machine, a traffic light, or a light switch.

## Why Use the State Design Pattern?
Typically, an object’s behavior depends on its fields or properties. However, with the State Pattern, we control behavior through explicit states and transitions:

## State Machines: 

State design is often used to implement finite state machines, where an object transitions from one state to another in response to events.
Encapsulation of State-Specific Behaviors: Each state has its own class, allowing us to encapsulate different behaviors in each state. This approach keeps each state’s logic organized and avoids a single, complex if-else or switch block.
## Example Scenario: Light Switch
Let’s use a Light Switch with two states: On and Off.

 We’ll define two states (OnState and OffState) and manage the switch’s behavior based on its state.

## Implementation
Step 1: Define an Abstract State Class
The LightState class is an abstract class with a method Toggle. This method will be implemented differently in each state, either switching the light on or off.
```
public abstract class LightState
{
    public abstract void Toggle(LightSwitch lightSwitch);
}
```
Step 2: Define Concrete States

We’ll create two classes, OnState and OffState, each representing a specific state of the light switch.


1. OnState: When the light is on, calling Toggle will turn it off.
2. OffState: When the light is off, calling Toggle will turn it on.

```
public class OnState : LightState
{
    public override void Toggle(LightSwitch lightSwitch)
    {
        Console.WriteLine("Turning light off...");
        lightSwitch.SetState(new OffState());
    }
}

public class OffState : LightState
{
    public override void Toggle(LightSwitch lightSwitch)
    {
        Console.WriteLine("Turning light on...");
        lightSwitch.SetState(new OnState());
    }
}

```
Each Toggle method changes the state of the LightSwitch by setting a new state. The OnState sets the state to OffState and vice versa.

##  Step 3: Create the Context Class

The LightSwitch class represents the context and holds a reference to the current state (_state).

It can set a new state and toggles the light by calling the Toggle method on the current state.

```
public class LightSwitch
{
    private LightState _state;

    public LightSwitch()
    {
        // Initial state is Off
        _state = new OffState();
    }

    public void SetState(LightState state)
    {
        _state = state;
    }

    public void Toggle()
    {
        _state.Toggle(this);
    }
}
```
## Step 4: Using the State Pattern

The following code demonstrates the Light Switch in action. We create a LightSwitch instance and toggle it multiple times to observe how it transitions between the On and Off states.
```
class Program
{
    static void Main(string[] args)
    {
        LightSwitch lightSwitch = new LightSwitch();

        lightSwitch.Toggle(); // Should turn on
        lightSwitch.Toggle(); // Should turn off
        lightSwitch.Toggle(); // Should turn on again
    }
}
```
## Explanation of the Code

1. Context and State: The LightSwitch class serves as the context. It manages the current state and delegates the Toggle functionality to the current state.

2. State Transition: Each state manages the transition to the next state. OnState transitions to OffState, and OffState transitions to OnState.

3. Behavior Control: By controlling behavior through states, we avoid complex conditional logic in the LightSwitch class and isolate the state-specific behaviors in individual classes.

## Benefits of Using the State Design Pattern
#### 1.Encapsulation of States: State-specific behaviors are isolated, leading to better organization and readability.
#### 2.Code Reusability: By defining behavior in states, we make it easy to reuse code, especially in complex systems with multiple states.
#### 3.Ease of Maintenance: Adding a new state or modifying behavior in a state becomes straightforward. You simply add or edit a state class without affecting the entire context.

## Conclusion
The State Design Pattern offers a structured approach to handle an object’s state-specific behavior, promoting clean, modular, and maintainable code. In our example, the light switch toggles smoothly between On and Off states, each encapsulating its behavior, which simplifies the code and makes it easier to extend. This pattern is especially beneficial when managing complex systems that require a clear distinction between different states.
Here’s a full code example with detailed comments to describe each part of the State Design Pattern implementation for the light switch scenario.
```
using System;

namespace StatePatternExample
{
    // Abstract class defining a State for the LightSwitch
    public abstract class LightState
    {
        // Toggle method that each state will implement differently
        public abstract void Toggle(LightSwitch lightSwitch);
    }

    // Concrete State representing the light being "On"
    public class OnState : LightState
    {
        // Method to turn the light off and change to OffState
        public override void Toggle(LightSwitch lightSwitch)
        {
            Console.WriteLine("Turning light off...");
            // Transition to OffState
            lightSwitch.SetState(new OffState());
        }
    }

    // Concrete State representing the light being "Off"
    public class OffState : LightState
    {
        // Method to turn the light on and change to OnState
        public override void Toggle(LightSwitch lightSwitch)
        {
            Console.WriteLine("Turning light on...");
            // Transition to OnState
            lightSwitch.SetState(new OnState());
        }
    }

    // Context class representing the LightSwitch
    public class LightSwitch
    {
        // Field to store the current state
        private LightState _state;

        // Constructor initializes the LightSwitch in the OffState
        public LightSwitch()
        {
            // Initial state is Off
            _state = new OffState();
        }

        // Method to change the state of the LightSwitch
        public void SetState(LightState state)
        {
            _state = state;
        }

        // Method to toggle the light by delegating to the current state's Toggle method
        public void Toggle()
        {
            _state.Toggle(this);
        }
    }

    // Main Program to test the LightSwitch and state transitions
    class Program
    {
        static void Main(string[] args)
        {
            // Create a new LightSwitch instance, starting in OffState
            LightSwitch lightSwitch = new LightSwitch();

            // Toggle the light to turn it on
            lightSwitch.Toggle(); // Output: Turning light on...

            // Toggle the light again to turn it off
            lightSwitch.Toggle(); // Output: Turning light off...

            // Toggle the light once more to turn it back on
            lightSwitch.Toggle(); // Output: Turning light on...
        }
    }
}
```
## Explanation of Each Part
##   1.LightState (Abstract State Class):

This abstract class serves as a blueprint for different states. It defines the Toggle method, which each concrete state (OnState and OffState) will implement differently.
##   2.OnState and OffState (Concrete State Classes):

Each of these classes represents a specific state of the light switch.
OnState:
When in the OnState, calling Toggle turns the light off by transitioning to the OffState.
The Toggle method in OnState changes the state of the LightSwitch to OffState and outputs "Turning light off..."
OffState:
When in the OffState, calling Toggle turns the light on by transitioning to the OnState.
The Toggle method in OffState changes the state of the LightSwitch to OnState and outputs "Turning light on..."
##  3.LightSwitch (Context Class):

The LightSwitch class holds a reference to the current state (_state) and manages state transitions.
## Constructor:
The initial state is set to OffState.
## SetState Method:
Allows changing the current state by setting _state to a new LightState.
## Toggle Method:
This method calls the Toggle method on the current state, which in turn transitions the state and outputs the corresponding message.
## Main Program:

The Main method tests the functionality of the LightSwitch.
## Toggle Calls:
Each Toggle call will change the state of the light switch between On and Off, demonstrating how the State Design Pattern allows the behavior of LightSwitch to change based on its current state.
## Output Example
Running this code will produce the following output:
```
Turning light on...
Turning light off...
Turning light on...
```
Each call to Toggle changes the state, demonstrating how the State Pattern keeps the code modular and avoids complex conditional logic, 

encapsulating behavior within state-specific classes.



<div align="right", dir="rtl", lang="ar">
أول مقالة في سلسلة "The State Design Pattern" 🎉

في السلسلة دي، هنتعلم مع بعض إزاي نبني state machine بشكل عملي ومنظم. المقالة الأولى متاحة دلوقتي على [Dev.to]
https://lnkd.in/dxjWvJYb
، وبتشرح الأساسيات وأهمية State Design Pattern.

 ليه نستخدم الـ State Design Pattern؟

الـ State Design Pattern بيساعد في التحكم في سلوك (objects) بناءً على حالتها الحالية، وده بيخلي الكود أكتر تنظيم ومرونة. شوية من أهم الفوائد:

1. تنظيم الكود وتقليل التعقيد: بيعتمد على تقسيم الحالات في كلاس خاص بكل حالة، وده بيقلل استخدام الشروط المعقدة (if-else statements).

2. سهولة الإضافة والتعديل: لو عايز تضيف حالة جديدة أو تعدل حالة موجودة، الموضوع بيكون سهل ومباشر من غير ما تأثر على باقي الكود.

3. تحسين قابلية قراءة الكود وصيانته: كل حالة ليها كلاس مستقل، فالكود بيكون أوضح وأسهل للفهم والصيانة.

بالتالي، State Design Pattern بيكون مثالي للأنظمة اللي بتتعامل مع حالات متعددة، وبيسهل عملية التطوير والصيانة.
</div>

-------------------------------------------------------------------------

###   [Understanding the State Design Pattern P2](https://dev.to/moh_moh701/understanding-the-state-design-pattern-p2-gp4)

Here’s a detailed article on building a state machine in C# from scratch. 

This example demonstrates managing a Phone Call State Machine with states like off-hook, connecting, connected, and on-hold.

We’ll implement this without any external libraries to show how to create a state machine using enums, dictionaries, and simple transitions.

### Introduction
State machines are often constructed using libraries that manage transitions and state configurations. However, implementing a state machine without any external libraries gives you a solid understanding of its internal workings. This article covers building a basic state machine from scratch to model a phone call scenario.

### Why Use a State Machine?
State machines are a way to model systems where an object can be in multiple states and transition between them based on specific events or triggers. This approach is useful in:

Finite state management.
Controlling complex workflows.
Ensuring organized and readable code.
In our example, we’ll model the states and transitions of a phone call:

## States: Define different statuses of the call (e.g., off-hook, connecting).
## Triggers: Define events that cause the call to transition from one state to another (e.g., dial a number).
Implementation of a Phone Call State Machine
We’ll break down the implementation into these main steps:

1.Define States and Triggers.

2. Set Up Transitions in a Dictionary.

3.Orchestrate the State Machine. 

## Step 1: Define States and Triggers
We’ll start by defining the states and triggers for our phone call state machine.

## States: Represents the different stages of a call.
## Triggers: Represents events that change the call’s state.

```
using System;
using System.Collections.Generic;

namespace StateMachineExample
{
    // Define the states of the phone call
    public enum State
    {
        OffHook,
        Connecting,
        Connected,
        OnHold
    }

    // Define the triggers that cause state transitions
    public enum Trigger
    {
        CallDialed,
        HungUp,
        CallConnected,
        PlacedOnHold,
        TakenOffHold,
        LeftMessage
    }
}
```
## Step 2: Define the Transition Rules
We use a dictionary to manage possible transitions. For each state, this dictionary will store the valid triggers and their resulting states.
```
public class PhoneCallStateMachine
{
    private static Dictionary<State, List<(Trigger, State)>> _rules = new()
    {
        // OffHook state transitions
        { State.OffHook, new List<(Trigger, State)>
            {
                (Trigger.CallDialed, State.Connecting)
            }
        },

        // Connecting state transitions
        { State.Connecting, new List<(Trigger, State)>
            {
                (Trigger.HungUp, State.OffHook),
                (Trigger.CallConnected, State.Connected)
            }
        },

        // Connected state transitions
        { State.Connected, new List<(Trigger, State)>
            {
                (Trigger.LeftMessage, State.OffHook),
                (Trigger.HungUp, State.OffHook),
                (Trigger.PlacedOnHold, State.OnHold)
            }
        },

        // OnHold state transitions
        { State.OnHold, new List<(Trigger, State)>
            {
                (Trigger.TakenOffHold, State.Connected),
                (Trigger.HungUp, State.OffHook)
            }
        }
    };

    public State CurrentState { get; private set; } = State.OffHook;

    public void MoveToNextState(Trigger trigger)
    {
        foreach (var transition in _rules[CurrentState])
        {
            if (transition.Item1 == trigger)
            {
                CurrentState = transition.Item2;
                return;
            }
        }

        Console.WriteLine($"Trigger '{trigger}' is invalid from '{CurrentState}' state.");
    }
}
```
This dictionary setup allows:

Defining valid state transitions for each state and trigger pair.

Avoiding invalid transitions by only allowing those defined in _rules.

## Step 3: Orchestrating the State Machine

Now, let’s set up the main program to test our state machine. We will prompt the user to select a trigger for transitioning between states.
```
class Program
{
    static void Main(string[] args)
    {
        var phone = new PhoneCallStateMachine();

        while (true)
        {
            Console.WriteLine($"The phone is currently: {phone.CurrentState}");
            Console.WriteLine("Select a trigger:");

            // Display possible triggers for the current state
            var availableTransitions = PhoneCallStateMachine._rules[phone.CurrentState];
            for (int i = 0; i < availableTransitions.Count; i++)
            {
                Console.WriteLine($"{i}. {availableTransitions[i].Item1}");
            }

            // Read user input for the trigger
            if (int.TryParse(Console.ReadLine(), out int choice) &&
                choice >= 0 && choice < availableTransitions.Count)
            {
                phone.MoveToNextState(availableTransitions[choice].Item1);
            }
            else
            {
                Console.WriteLine("Invalid choice, please try again.");
            }

            Console.WriteLine();
        }
    }
}
```
## Explanation of the Code
-. User Interaction: The Main method displays the current state and available triggers based on the current state.
-. Transition Execution: MoveToNextState uses the selected trigger to update the state. If the transition is invalid, it displays a message.
-. Loop Continuation: This example keeps running, allowing you to navigate between states until manually stopped.
Example Run
When you run the code, you’ll see prompts allowing you to navigate through states as if handling a real phone call.

## 1.Initial State:
```
   The phone is currently: OffHook
   Select a trigger:
   0. CallDialed
```
## 2.After Calling:
```
   The phone is currently: Connecting
   Select a trigger:
   0. HungUp
   1. CallConnected
```
## 3.When Connected:
```
   The phone is currently: Connected
   Select a trigger:
   0. LeftMessage
   1. HungUp
   2. PlacedOnHold
```
By navigating through each option, the phone call state machine emulates different real-life scenarios like being connected, put on hold, or hanging up.

### Benefits of Building a State Machine Manually
Fine-grained Control: Directly manage how states and triggers interact, allowing customization beyond what typical libraries offer.
Learn Fundamental Concepts: Understand how transitions work without relying on abstractions.
Enhanced Flexibility: Easily add or modify states, transitions, or behaviors.
## Conclusion
Creating a state machine without external libraries in C# is simple yet powerful. This approach provides clear state and transition management, crucial in systems like phone calls, vending machines, and workflow management systems. While libraries can simplify state machine implementation, building one yourself enhances understanding and control over your code’s behavior.

This example provides a solid foundation for building more complex state machines in C#.


