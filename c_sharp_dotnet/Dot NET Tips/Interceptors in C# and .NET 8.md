
##  [Interceptors in C# and .NET](https://medium.com/@serkutyildirim/interceptors-in-c-and-net-458bae5f6748)

![image](https://github.com/user-attachments/assets/ec6017d7-ef52-426d-8958-85574101565a)

Interceptors, which are expected to come with C# 12 and .NET 8, allow specific method calls to be rerouted to different code.

Attributes specify the actual source code location so interceptors are generally appropriate only for source generators.

An interceptor is a method which can declaratively substitute a call to an interceptable method with a call to itself at compile time. 

This substitution occurs by having the interceptor declare the source locations of the calls that it intercepts. 

This provides a limited facility to change the semantics of existing code by adding new code to a compilation (e.g. in a source generator).

This interesting feature, performs override based on the specified lines and characters for methods functionality wise. I liken this new feature to the “go to” keyword.

Let’s take a closer look at how the Interceptors feature works in .NET

![image](https://github.com/user-attachments/assets/9b50f1d8-3d89-47ff-bdf5-2220611233a1)


Figure 1–0–1
The Figure 1–0–1 is you’d expect result before running the interceptor feature.

![image](https://github.com/user-attachments/assets/b4fbc44a-7362-4292-8333-620df565afd7)


Figure 1–0–2
Before running the app, you have to do some changes in your csproj file. Because interceptors are an experimental feature, 

you’ll need to explicitly enable them in your project file like Figure 1–0–2.

![image](https://github.com/user-attachments/assets/b7908931-0708-47d5-b21a-48050e72903b)


Figure 1–0–3
Next step, we need to define the [InterceptsLocation] attribute in our project. This is what drives the interceptor behaviour, but it’s not included in the base class library (BCL) anywhere yet, so you need to define it yourself. That’s true when you’re using interceptors from source generators too.

Define the attribute somewhere in your project like Figure 1–0–3.

Next step, we can create our interceptor class to the solution and then the magic is going to be started.

To create an interceptor you need to

Create a static class.

Define a method that has the exact same parameters and return type as your target method.

Decorate your interceptor with the [InterceptsLocation] attribute, specifying the location in the source of the method you wish to intercept.

![image](https://github.com/user-attachments/assets/8846b6a1-19ff-49f3-a367-653c79f10799)


Figure 1–0–4
For my example, I want to intercept the PersonInfo method with different features, so I need to create methods that has the same signature as Figure 1–0–4.

![image](https://github.com/user-attachments/assets/d8f71e15-34b6-4cc9-8578-ac7478221399)


Figure 1–0–5
And that’s it. When we run our application as shown in Figure 1–0–5, we see that our interceptor is working!
