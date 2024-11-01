

##        [Background Tasks in .NET 8](https://thecodeman.net/posts/background-tasks-in-dotnet8)
 

Many thanks to the sponsors who make it possible for this newsletter to be free for readers.
 
• If you have ever used Postman to debug and write tests for your REST APIs, guess what, 

those are the same concepts you need to know for writing tests for your gRPC requests in Postman

For more info about gRPC, they created a great beginner article here .
 
 

###                  The Background
 
 

##    What is Background task? What is Hosted Service?
 
A Background Task in .NET typically refers to any operation that runs in the background , separate from the main flow of your application.

This can include tasks like processing data, performing I/O operations, or any other long-running processes that you don't want to run on the main thread because they might block the user interface or the main operation of your application.
 
A Hosted Service in .NET, specifically in the context of ASP.NET Core, is a service that is hosted within the application's process . 

It's a way to run long-running background tasks that are tied to the lifecycle of the application.
 
In today's article, I'll explore a recently introduced aspect of the Microsoft.Extensions.Hosting library, which has been part of .NET 8 from its fourth preview, specifically focusing on how it impacts hosted services.
 
Let's dive in...
 
## What was wrong before .NET 8?
 
In earlier versions leading up to .NET 8, the initiation and termination of hosted services within the framework were managed in a sequential manner.
 
Specifically, each service registered as an IHostedService in the Dependency Injection (DI) container was started one after the other.
 
This process involved invoking the StartAsync method for each service and waiting for its completion before moving on to the next one . 

While this approach generally did not pose significant issues for most applications, there were scenarios where it could lead to complications.
 
For example, if a hosted service executed a lengthy process in its StartAsync method, it could inadvertently delay the startup of subsequent services in the application .
 
It is advisable to minimize the workload in the StartAsync method, but the potential for a slow service to hinder the overall application startup remained a concern prior to the introduction of .NET 8.
 
## Let's see the example of how the background service was implemented before .NET 8:


```
public class SomeService : IHostedService
{
    public async Task StartAsync(CancellationToken cancellationToken)
    {
        Console.WriteLine("Start Service");
        await Task.Delay(60000); //1 minute
    }
    public async Task StopAsync(CancellationToken cancellationToken)
    {
        Console.WriteLine("Stop Service");
    }
}
```


DI registration:
```
builder.Services.AddHostedService&lt;SomeService&gt;();
```
##  So what is the problem here?
 

As I mentioned above, StartAsync will be started and will have to wait for it to finish before moving on to the next service. 

At this point the application won't actually start yet because DI hasn't registered all the services.

![image](https://github.com/user-attachments/assets/85466cb9-36e8-4622-b7ff-1e28b8d22c59)

I simulated a job that takes 6 seconds and as you can see, 6 seconds passed before the application started.
 
This can be a problem. But there is a solution.
 
.NET 8 Fix
 
In .NET 8, you now have the option to start or stop your services at the same time, instead of one after the other. This is done by changing the settings in HostOptions .

If you want all your services to start together, turn on the ServicesStartConcurrently option. And if you want them to stop at the same time, 

there's a setting for that too, called ServicesStopConcurrently . 
 
## Let' take a look on the example:
```
builder.Services.Configure&lt;HostOptions&gt;(options =>
{
    options.ServicesStartConcurrently = true;
    options.ServicesStopConcurrently = true;
});
 
builder.Services.AddHostedService
```
The result of this - other services started without any delay.

 ![image](https://github.com/user-attachments/assets/6b1df8d1-16f6-433f-bbd9-d2be029b690b)

More control over hosted services
 

.NET 8 introduced a new interface for hosted services - IHostedLifeCycleService . The reason for this is that we don't have much control over the services within the StartAsync() and StopAsync() methods.
 

This interface abstracts 4 more new methods:
 

• StartingAsync
• StartedAsync
• StoppingAsync
• StoppedAsync

```
public class SomeService : IHostedLifecycleService
{
    public async Task StartAsync(CancellationToken cancellationToken)
    {
        Console.WriteLine("Start Service");
    }

    public async Task StartedAsync(CancellationToken cancellationToken)
    {
        Console.WriteLine("Started Service");
    }

    public async Task StartingAsync(CancellationToken cancellationToken)
    {
        Console.WriteLine("Starting Service");
    }

    public async Task StopAsync(CancellationToken cancellationToken)
    {
        Console.WriteLine("Stop Service");
    }

    public async Task StoppedAsync(CancellationToken cancellationToken)
    {
        Console.WriteLine("Stopped Service");
    }

    public async Task StoppingAsync(CancellationToken cancellationToken)
    {
        Console.WriteLine("Stopping Service");
    }
}
```

This allows us to have full control over hosted services, while they starting (Starting), during their startup (Start), and when they are started (Started) - same for Stop, also.
 
 

##             Wrapping up
 
 

Before .NET 8, when an app started, its hosted services would start one after the other. Each service had to finish starting before the next one could begin. 
This usually didn't cause problems, but sometimes a slow service could delay the whole app from starting.
In .NET 8, two new features let us start or stop services simultaneously, instead of one after another. You can turn this on by changing the settings in HostOptions for any services you're using.
 

In addition, now you can have a full control over Hosted Services by implementing a new interface IHostedLifecycleService which comes with 4 new methods for controlling the service (StartedAsync, StartingAsync, StoppedAsync, StoppingAsync).
 

Install .NET 8 SDK and test, I'm sure you will see the benefits of this.
 

That's all from me today.

------------------------------------

##                     Avoid IHostedService before .NET 8 if you can,

because it has a problem... 👇 

In .NET versions before 8, the 𝐒𝐭𝐚𝐫𝐭𝐀𝐬𝐲𝐧𝐜 methods of 𝐈𝐇𝐨𝐬𝐭𝐞𝐝𝐒𝐞𝐫𝐯𝐢𝐜𝐞 implementations were called one after the other during application startup. 

Each StartAsync method had to be finished before the next one could begin, which could delay the startup if any service took a long time to initialize.

Imagine you have a task that executes for a couple of minutes.

This means the application would not start until that task is finished.

.NET 8 introduces a solution with the 𝐒𝐞𝐫𝐯𝐢𝐜𝐞𝐬𝐒𝐭𝐚𝐫𝐭𝐂𝐨𝐧𝐜𝐮𝐫𝐫𝐞𝐧𝐭𝐥𝐲 property in the HostOptions class. 

With services now starting concurrently, the host’s startup time is minimized to the duration of the slowest service's StartAsync method, rather than being the combined time of all services.

Note: Each service begins only after the previous one releases control by reaching its first await statement in the StartAsync method. 

To allow the next service to start as quickly as possible, ensure that StartAsync is truly asynchronous and avoid any costly operations before the first await.

![image](https://github.com/user-attachments/assets/4d082122-fbe9-46d2-abe4-267cad2302b2)






