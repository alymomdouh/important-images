
### [How to Measure Performance in C# and .NET with BenchmarkDotNet Library ?](https://medium.com/@serkutyildirim/how-to-measure-performance-in-c-and-net-with-benchmarkdotnet-library-751530c17620)

![image](https://github.com/user-attachments/assets/f16459ca-2006-48ea-abdb-02f6f01b814e)


We can calculate the complexity analysis for the algorithms we create in big O notation. So, 

how fast and in what time does the O(n) complexity code we wrote really respond in terms of performance? Keeping such as 

this statistics and calculating them by the programmer creates additional costs in the project.

We can create test code for our own unit tests in traditional ways, but in a scenario where we have many Methods, benchmarking can start to become quite complicated.

Especially, other costly tasks such as keeping the results in a file or write the results to Excel file begin to emerge.

So why don’t we let a code library do this kind of repetitive work?

## BenchmarkDotNet, 
published by the Dotnet Foundation, is a .NET library and is used in performance measurement processes. 

It performs all the operations we mentioned in the previous paragraph and prevents common mistakes made when writing test code. 

It creates a project for each C# method to be measured, starts the project multiple times and runs the same method in loops.

There is usually no need to choose how many times to run methods, because BenchmarkDotNet automatically selects the desired precision level.

So how do we use the BenchmarkDotNet library in our project?

Now, let’s add BenchmarkDotNet to our project via the NuGet package manager.

![image](https://github.com/user-attachments/assets/d1b2c5fc-9240-4171-bdd8-a13c5f87a609)


Figure 1–0–2
We right-click on the Solution and click on “Manage Nuget Packages for Solution”, as shown in Figure 1–0–2.

![image](https://github.com/user-attachments/assets/70edd136-c972-4303-9026-28c7d4bfee9d)


Figure 1–0–3
Then we click on the “Browse” tab and type “Benchmark” as shown in Figure 1–0–3. We select the “BenchmarkDotNet” library at the bottom and click install as like Figure 1–0–3.

![image](https://github.com/user-attachments/assets/aca25539-1e2a-485a-874a-fcae5a7deb14)


Figure 1–0–4
Next, we create a class containing our methods that we will benchmark test, as shown in Figure 1–0–4.

![image](https://github.com/user-attachments/assets/25eb1613-9106-4158-a526-27d8f3da2e62)


Figure 1–0–5
As you can see in Figure 1–0–5, we wrote the methods we want to perform Benchmark tests in our BenchmarkTest class we created.

We add the [Benchmark] attribute to the head of the methods that we will do benchmark testing. In this way, we determine which methods will be Benchmark test on in our test class.

The [MemoryDiagnoser(false)] attribute we put at the head of the Test Class also ensures that the “Allocated” column, which is related to memory usage, appears in the benchmark results.

![image](https://github.com/user-attachments/assets/d57bbd05-9983-4aea-a12e-3a5b1d0f3160)


Figure 1–0–6
As seen in Figure 1–0–6, firstly, we give the class we wrote for testing as a parameter to the BenchmarkRunner class. This Method, will catch Methods with the [Benchmark] attribute in head , perform performance tests and show us the results. Then we put the project in “Release” mode. And Then right click the “Solution” and click “Clean” “Build” respectively. Now our project is ready for Benchmark tests. Finally, hit the Run button to see the results.

![image](https://github.com/user-attachments/assets/c7c84c5e-2f1a-4a66-886d-9189a21d2578)


Figure 1–0–7
As you can see in Figure 1–0–7, we performed our performance tests with the BenchmarkDotNet library. It showed us the results of the methods we used to perform performance tests on the console as summary. As you can see, it seems that MaxBy is better in performance than the old Way.

![image](https://github.com/user-attachments/assets/7afc3c79-95a2-474d-b7fc-1ebb8a4380d4)


Figure 1–0–8
As seen in Figure 1–0–8, the BenchmarkDotNet library produced the reports we mentioned at the beginning of the article for us.

## Source codes are here: [BenchmarkExample](https://github.com/serkutYILDIRIM/BenchmarkExample)

##  from fadddo github  [here](https://github.com/mfaddo/AggregationsBenchMarks)

