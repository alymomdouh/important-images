
## How can you inject a scoped service into .NET middleware?

Most developers don't know about this.

What's this secret feature?

## The Invoke(Async) method supports method injection.

The injected services will use the current request's service scope.

This means services have the same lifetime as any other scoped service.

What about constructor injection with middleware?

Middleware is constructed once per application lifetime.

If you try injecting a scoped service, you'll get an exception:

But with method injection, you won't run into this problem
![image](https://github.com/user-attachments/assets/c02bb645-f2c2-466f-af96-e84e8a6da68f)
