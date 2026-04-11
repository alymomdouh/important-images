##  Modern .NET 10 Vertical Slice Architecture Starter Kit

I’ve built and open-sourced a modern .NET 10 REST API template designed with Vertical Slice Architecture + CQRS structured the way scalable, maintainable systems should be built.

This isn’t a demo.

It’s production-ready.

 It can easily save you 100+ hours of setup, refactoring, and architectural cleanup.


𝗪𝗵𝘆 𝗧𝗵𝗶𝘀 𝗧𝗲𝗺𝗽𝗹𝗮𝘁𝗲?

Most templates stop at CRUD.

This one focuses on real-world architecture.


✔ Vertical Slice Architecture (feature-first structure)

✔ Clean CQRS separation (Commands & Queries)

✔ Minimal APIs for high performance

✔ FluentValidation pipeline (automatic validation)

✔ Logging decorators (request/response tracing)

✔ Global exception handling

✔ Result<T> pattern for consistent error responses

✔ Health checks (DB readiness & liveness)

✔ PostgreSQL + EF Core 10

✔ Scalar UI (modern OpenAPI experience)


No patching things later.

No “we’ll clean it up after MVP.”


It’s structured correctly from day one.

𝗛𝗼𝘄 𝗜𝘁’𝘀 𝗦𝘁𝗿𝘂𝗰𝘁𝘂𝗿𝗲𝗱

 • Each feature is fully isolated:
 
 • Request (record)
 
 • Response (record)
 
 • Validator
 
 • Handler
 
 • Endpoint

No fat controllers.

No service chaos.

No mixed responsibilities.


Just clean, scalable slices.

𝗖𝗹𝗲𝗮𝗻 𝗥𝗲𝗾𝘂𝗲𝘀𝘁 𝗙𝗹𝗼𝘄

HTTP Request

→ Endpoint

→ Logging Decorator

→ Validation Decorator

→ Handler

→ Result Mapping

→ HTTP Response


All unhandled exceptions are processed by a global exception handler to ensure consistent API responses.

𝗪𝗵𝘆 𝗩𝗲𝗿𝘁𝗶𝗰𝗮𝗹 𝗦𝗹𝗶𝗰𝗲?

Instead of organizing by layers (Controllers, Services, Repositories),

we organize by feature.

This improves:

• Maintainability

• Testability

• Scalability

• Team collaboration

• Long-term clarity
 
𝗪𝗮𝗻𝘁 𝘁𝗵𝗶𝘀 𝐕𝐞𝐫𝐭𝐢𝐜𝐚𝐥 𝐒𝐥𝐢𝐜𝐞 𝐀𝐫𝐜𝐡𝐢𝐭𝐞𝐜𝐭𝐮𝐫𝐞 𝐒𝐭𝐚𝐫𝐭𝐞𝐫 𝐊𝐢𝐭?

<img width="800" height="1000" alt="image" src="https://github.com/user-attachments/assets/5832e0a3-9bf6-4f39-87fd-42d9bdcf9c64" />


👉 Here’s the GitHub repository:

https://github.com/KanaiyaKatarmal/VerticalSliceArchitectureTemplate

- If you find it useful, please star ⭐ the repo
  
- Share this post with your network 🙌
