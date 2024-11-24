
## MediatR is one of the best .NET libraries I ever worked with.

You'll end up reinventing the wheel if you roll your own.

You can use it to implement the CQRS pattern.

The CQRS pattern separates the writes and reads in the application.

This separation is mainly logical. Separate components handle each operation.

MediatR decouples the in-process sending of messages from handling messages.

You can extend MediatR's IRequest interface with a custom ICommand and IQuery abstraction.

This allows you to be explicit about commands and queries in your system.

A few benefits of using MediatR:

- Organize your code around use cases (high cohesion)
- Easily implement cross-cutting concerns
- API endpoints become very thin
- Use cases are highly testable

If you want to learn more about CQRS with MediatR, I wrote a detailed guide.

You can read it here: https://lnkd.in/eUhi8KkG

The one drawback of MediatR I have to highlight is indirection.

But this is a tradeoff I accept to get the other benefits.

What's your stance on using MediaR?

---
Do you want to simplify your development process? 
Grab my free Clean Architecture template here: https://bit.ly/3UAgssW

![image](https://github.com/user-attachments/assets/bfdefb89-2707-462d-8bd6-04cd2b2a85bc)
