
## [Polly retry pattern in .Net Core](https://dotnet-fullstack-dev.blogspot.com/2024/09/Polly%20retry%20pattern%20in%20.Net%20Core.html)
![image](https://github.com/user-attachments/assets/04a2730b-56ce-4e28-8383-0ef3d135d5f7)

The Polly retry pattern is a powerful mechanism for handling transient faults in applications by retrying failed operations. 

Polly is a .NET library that provides resilience strategies like retries, circuit breakers, timeouts, and fallback mechanisms. 

The retry pattern is particularly useful when dealing with unreliable external services, such as APIs or databases, that may occasionally fail but succeed on subsequent attempts.

In this blog, we will explore how to implement the Polly retry pattern in .NET Core, particularly in the context of using HttpClient with IHttpClientFactory. 

We'll also see how to configure retry policies to make your HTTP requests more resilient.

##  Why Use the Retry Pattern?
### 1.Transient Failures: 
Network failures or intermittent connectivity issues may cause operations to fail temporarily.
### 2.Rate Limiting: 
External services may apply rate limits and reject requests, but retrying after a delay can succeed.
### 3.Load Balancing:
Sometimes, servers are temporarily overloaded, and retrying the request may succeed after a few seconds.
### 4.Failover: 
When working with multiple instances of a service, one instance might fail, but a retry could succeed on another instance.

### Installing Polly
Polly is available as a NuGet package. You can add it to your project using the following command:
```
dotnet add package Polly
```
### Integrating Polly with HttpClient and IHttpClientFactory
Polly can be easily integrated with IHttpClientFactory to apply retry policies to HTTP requests. Here’s how you can set up a retry policy for an HttpClient that makes requests to an external API.

### Step 1: Register IHttpClientFactory and Polly Policies
In your Startup.cs or Program.cs (for .NET 6+), you will register your HttpClient with Polly retry policies.
```
using Polly;
using Polly.Extensions.Http;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        // Register IHttpClientFactory with a retry policy
        services.AddHttpClient("RetryClient", client =>
        {
            client.BaseAddress = new Uri("https://jsonplaceholder.typicode.com/");
            client.DefaultRequestHeaders.Add("Accept", "application/json");
        })
        .AddPolicyHandler(GetRetryPolicy()); // Add retry policy

        services.AddControllers();
    }

    // Define the retry policy
    private static IAsyncPolicy<HttpResponseMessage> GetRetryPolicy()
    {
        return HttpPolicyExtensions
            .HandleTransientHttpError()  // Handles 5xx, 408, and network failures
            .OrResult(msg => msg.StatusCode == System.Net.HttpStatusCode.TooManyRequests) // Custom condition
            .WaitAndRetryAsync(3, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt))); // Exponential backoff
    }
}
```
### Explanation of the Retry Policy:
HandleTransientHttpError(): This handles HTTP 5xx status codes (server errors), HTTP 408 (request timeout), and other network issues.
OrResult(): This allows you to handle custom conditions like 429 Too Many Requests, which is a common status for rate limiting.
WaitAndRetryAsync(): This specifies how many retries should be attempted (3 retries in this case) and the delay between retries. Here, we use exponential backoff (2^retryAttempt), meaning the delays will be 2, 4, and 8 seconds.
### Step 2: Using the HttpClient with Retry Policy
In your controller or service, you can inject the HttpClientFactory to use the HttpClient configured with the retry policy.
```
using Microsoft.AspNetCore.Mvc;
using System.Net.Http;
using System.Threading.Tasks;

[ApiController]
[Route("api/[controller]")]
public class ItemsController : ControllerBase
{
    private readonly IHttpClientFactory _httpClientFactory;

    public ItemsController(IHttpClientFactory httpClientFactory)
    {
        _httpClientFactory = httpClientFactory;
    }

    [HttpGet("retry-example")]
    public async Task<IActionResult> GetDataWithRetry()
    {
        // Create an HttpClient with the retry policy
        var client = _httpClientFactory.CreateClient("RetryClient");

        // Make an HTTP request to an external API
        var response = await client.GetAsync("/posts");

        if (response.IsSuccessStatusCode)
        {
            var data = await response.Content.ReadAsStringAsync();
            return Ok(data);
        }

        return StatusCode((int)response.StatusCode, "Failed to retrieve data after retries");
    }
}
```
### Step 3: Customizing the Retry Policy
You can customize the retry policy by adding conditions based on the specific needs of your application. For instance, you can add more specific error conditions or change the backoff strategy.

Retry Based on Specific HTTP Status Codes:

If you want to retry only for certain status codes, like 500 Internal Server Error or 503 Service Unavailable, you can customize the policy as follows:
```
private static IAsyncPolicy<HttpResponseMessage> GetCustomRetryPolicy()
{
    return Policy
        .HandleResult<HttpResponseMessage>(r => r.StatusCode == System.Net.HttpStatusCode.InternalServerError || 
                                                r.StatusCode == System.Net.HttpStatusCode.ServiceUnavailable)
        .WaitAndRetryAsync(3, retryAttempt => TimeSpan.FromSeconds(2));
}
```
###  Jitter Strategy:

A jitter strategy adds randomness to retry delays, which helps avoid thundering herd problems, where many requests retry at the same time and overload the system.
```
private static IAsyncPolicy<HttpResponseMessage> GetJitterRetryPolicy()
{
    var jitterer = new Random();
    return HttpPolicyExtensions
        .HandleTransientHttpError()
        .WaitAndRetryAsync(3, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)) 
                                              + TimeSpan.FromMilliseconds(jitterer.Next(0, 1000)));
}
```
### Retry on HTTP Method:

You can also configure the retry policy to apply only to certain HTTP methods, such as GET, to ensure retry is not applied to non-idempotent operations like POST.
```
private static IAsyncPolicy<HttpResponseMessage> GetMethodSpecificRetryPolicy()
{
    return Policy
        .Handle<HttpRequestException>()
        .OrResult<HttpResponseMessage>(r => r.StatusCode == System.Net.HttpStatusCode.RequestTimeout)
        .WaitAndRetryAsync(3, retryAttempt => TimeSpan.FromSeconds(2), onRetry: (outcome, timespan, retryCount, context) =>
        {
            if (context["httpMethod"] as string == HttpMethod.Get.Method)
            {
                Console.WriteLine($"Retrying GET request: {retryCount}");
            }
        });
}
```
Then, use context when calling the HTTP client:
```
var client = _httpClientFactory.CreateClient("RetryClient");
var context = new Context();
context["httpMethod"] = HttpMethod.Get.Method;
var response = await Policy.ExecuteAsync(ctx => client.GetAsync("/posts"), context);
```
### Advanced Polly Features
1. Circuit Breaker: You can combine retry with the circuit breaker pattern, which stops retrying after a certain number of failures to prevent overwhelming the service.

2. Bulkhead Isolation: Bulkhead limits the number of concurrent requests to a resource, ensuring that failures in one part of the system don’t overload the rest.

3. Timeouts: Use Polly's timeout policy to prevent long-running requests from hanging indefinitely.

### Conclusion
The Polly retry pattern, combined with IHttpClientFactory, provides a robust and flexible way to handle transient faults in your .NET Core applications. By implementing retry policies, you can make your HTTP communication more resilient to intermittent network failures, rate limiting, and other external service issues.

Incorporating these resilience strategies, like retry with exponential backoff or jitter, ensures that your application can recover from failures without overloading external services. You can further customize Polly by integrating features like circuit breakers, bulkhead isolation, and timeouts to build a resilient and fault-tolerant microservice architecture.

With Polly, your .NET Core applications become much more reliable when dealing with flaky or unreliable external services.
   
