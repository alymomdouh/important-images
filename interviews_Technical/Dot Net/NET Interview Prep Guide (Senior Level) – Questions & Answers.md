
##  125 questions senior .NET devs get asked in interviews.

All 125 with full answers.

###  𝗖# & .𝗡𝗘𝗧 𝗙𝘂𝗻𝗱𝗮𝗺𝗲𝗻𝘁𝗮𝗹𝘀
Value types vs reference types. Garbage collection and GC pressure. Span<T>. Records vs classes. IEnumerable vs IQueryable. Boxing/unboxing. Abstract class vs interface. Async/await internals. Thread safety. Dependency injection.

###  𝗔𝗦𝗣.𝗡𝗘𝗧 𝗖𝗼𝗿𝗲 & 𝗪𝗲𝗯 𝗔𝗣𝗜𝘀
The full request lifecycle. DI lifetime mistakes. Idempotent endpoints. Middleware ordering bugs. Rate limiting. Error handling strategies. API versioning. Pagination. Content negotiation. File upload security.

###  𝗗𝗮𝘁𝗮𝗯𝗮𝘀𝗲 & 𝗗𝗮𝘁𝗮 𝗔𝗰𝗰𝗲𝘀𝘀
EF Core vs Dapper. The N+1 trap. Optimistic vs pessimistic concurrency. Production migrations. Rollback plans. Index strategy. Transaction boundaries. Denormalization tradeoffs.

###  𝗖𝗹𝗼𝘂𝗱 & 𝗜𝗻𝗳𝗿𝗮𝘀𝘁𝗿𝘂𝗰𝘁𝘂𝗿𝗲
App Service vs Container Apps vs AKS. Secrets management. Managed identities. Background jobs in the cloud. Multi-region availability. SLOs and error budgets. Cost optimization.

###  𝗗𝗲𝘃𝗢𝗽𝘀 & 𝗗𝗲𝗽𝗹𝗼𝘆𝗺𝗲𝗻𝘁
CI/CD pipeline design. Zero-downtime deployments. Containerizing .NET apps. Feature flag rollout. Security scanning in CI/CD. Canary releases. Environment config management.

###  𝗦𝗲𝗰𝘂𝗿𝗶𝘁𝘆
Auth flows. 401 vs 403. Role-based and policy-based authorization. Mass assignment prevention. Threat modeling. Least privilege in microservices. Resource ownership authorization. Secret rotation incident response.

###  𝗠𝗲𝘀𝘀𝗮𝗴𝗶𝗻𝗴 & 𝗔𝘀𝘆𝗻𝗰 𝗣𝗿𝗼𝗰𝗲𝘀𝘀𝗶𝗻𝗴
Message queues vs pub/sub. Idempotent consumers. Poison messages. The Outbox pattern. Retry policies without retry storms. Background processing with IHostedService.

###  𝗢𝗯𝘀𝗲𝗿𝘃𝗮𝗯𝗶𝗹𝗶𝘁𝘆 & 𝗗𝗲𝗯𝘂𝗴𝗴𝗶𝗻𝗴
Structured logging. Distributed tracing. Diagnosing slow APIs. Correlation IDs. Alert fatigue reduction. Memory leak debugging. Post-incident reviews.

###  𝗧𝗲𝘀𝘁𝗶𝗻𝗴
Testing strategy. Integration tests with real databases. What makes a test valuable. Contract testing. Testing background services. Testing retries and circuit breakers. Eventual consistency without flaky sleeps.

###  𝗔𝗿𝗰𝗵𝗶𝘁𝗲𝗰𝘁𝘂𝗿𝗲 & 𝗗𝗲𝘀𝗶𝗴𝗻
Project structure decisions. Monolith to microservices. Sync vs async communication. Bounded contexts. Graceful degradation. API gateway vs service mesh. Domain modeling.

###  𝗗𝗲𝘀𝗶𝗴𝗻 𝗣𝗮𝘁𝘁𝗲𝗿𝗻𝘀 & 𝗦𝗢𝗟𝗜𝗗
SOLID with real .NET examples. Strategy vs Factory. Observer vs direct calls. Decorator for cross-cutting concerns. Mediator vs direct DI. Anti-patterns to watch for.

These aren't trivia questions. No "what does SOLID stand for?" No memorized definitions.

Real questions. Real answers. The kind that separate senior devs from everyone else.

###  Grab the full guide for free here:
https://lnkd.in/g9bWNzkR

<img width="800" height="1004" alt="image" src="https://github.com/user-attachments/assets/546790a2-3c65-4bdd-8cf9-80fa32dffc6a" />

-------------------------------------------------------------------------------
# .NET Interview Prep Guide (Senior Level) – Questions & Answers

---

## 🟥 C#, ASP.NET Core & Data

### 1. Value Types vs Reference Types
- Value:store in stack, copied by value
- Reference: store in heap, copied by reference

### 2. Garbage Collection in .NET
- is an automatic memory management system that frees memory occupied by objects that are no longer in use, helping prevent memory leaks and improving performance
- is handled by the CLR(Common Language Runtime) and is responsible for automatically reclaiming memory of objects that are no longer referenced.
- Objects are allocated on the managed heap and organized into generations (Gen 0, 1, 2) to optimize performance, since most objects are short-lived.
- The GC process involves marking reachable objects, sweeping unused ones, and compacting memory.
- It starts from GC roots like stack variables and static references.
- For unmanaged resources, we use IDisposable because GC is non-deterministic.
 
### 2.1 What is CLR in .NET?
- It is the execution engine of .NET `responsible for running applications and managing memory, security, and code execution`
```
# 🧠 Deep Explanation

### 1. Execution Engine

* CLR هو اللي **يشغل الكود**
* بيحول **IL (Intermediate Language)** إلى **Machine Code** باستخدام:

✔ **JIT Compiler (Just-In-Time)**

---

### 2. Memory Management

* مسؤول عن:

  * **Garbage Collection (GC)**
  * تخصيص وإزالة الذاكرة

---

### 3. Language Interoperability

✔ أهم ميزة:

> تقدر تكتب C# أو VB.NET أو F# وكلهم يشتغلوا مع بعض

لأنهم بيتحولوا لـ **IL**

---

### 4. Security

* Code Access Security
* يمنع تشغيل كود غير موثوق

---

### 5. Exception Handling

* إدارة الأخطاء بشكل موحد

---

### 6. Thread Management

* إدارة الـ multithreading

---

### 7. Type Safety

* يمنع:

  * invalid casting
  * memory corruption

---

# ⚙️ Execution Flow (مهم جدًا 👇)

1. تكتب كود C#
2. يتحول لـ **IL**
3. CLR يشغل JIT
4. يتحول لـ Machine Code
5. يبدأ التنفيذ

---

# 🔥 جملة تميزك في الإنترفيو

> CLR acts like a virtual machine for .NET applications, similar to JVM in Java.

---

# 💬 Example Answer (جاهزة للحفظ)

> The CLR (Common Language Runtime) is the core runtime of the .NET platform. It is responsible for executing .NET applications by converting Intermediate Language (IL) into machine code using the JIT compiler.
> It also provides services like garbage collection, memory management, exception handling, security, and thread management.
> Essentially, it acts as a virtual machine that ensures safe and efficient execution of .NET code.

```

### 3. Span<T>
- High performance memory access without allocation

### 4. Record vs Class
- Record: immutable, value equality

### 5. IEnumerable vs IQueryable
- IEnumerable: in-memory
- IQueryable: database query

### 6. HTTP Request Lifecycle
Middleware → Routing → Controller → Response

### 7. DI Lifetimes
- Scoped: per request
- Transient: new instance
- Singleton: one instance

### 8. Idempotent APIs
- Same request = same result

### 9. Middleware Order
- Order matters (Auth before Authorization)

### 10. Rate Limiting
- Prevent abuse

### 11. EF Core vs Dapper
- EF: ORM
- Dapper: fast SQL

### 12. N+1 Problem
- Multiple queries issue
- Fix: Include / projection

### 13. Concurrency Control
- Optimistic vs Pessimistic

### 14. DB Migration
- Use CI/CD, backups

### 15. Slow Query Debugging
- Logs, indexes, execution plan

---

## 🟩 Cloud, DevOps & Security

### 1. App Service vs Containers vs AKS
- App Service: simple
- Containers: flexible
- AKS: scalable

### 2. Secrets Management
- Use Key Vault

### 3. Managed Identity
- No credentials needed

### 4. Background Jobs
- Hangfire / Functions

### 5. CI/CD
- Build → Test → Deploy

### 6. Zero Downtime
- Blue-Green / Rolling

### 7. Docker
- Containerize apps

### 8. Authentication
- JWT / OAuth

### 9. 401 vs 403
- 401: unauthenticated
- 403: unauthorized

### 10. Authorization
- Role / Policy based

### 11. Overposting
- Use DTOs

### 12. Message Queue
- Async processing

### 13. Duplicate Messages
- Idempotency

### 14. Failed Messages
- Retry / Dead-letter

---

## 🟪 Testing, Architecture & Design

### 1. Testing Strategy
- Unit, Integration, E2E

### 2. Integration Testing
- Use test DB

### 3. Good Test
- Fast, independent

### 4. Logging
- Structured logging (Serilog)

### 5. Debug Slow API
- Logs, APM, DB

### 6. Distributed Tracing
- Trace IDs

### 7. Project Structure
- Clean Architecture

### 8. Monolith vs Microservices
- Use microservices for scalability

### 9. Sync vs Async
- Async better for scaling

### 10. SOLID
- SRP, OCP, LSP, ISP, DIP

### 11. SOLID Impact
- Maintainable code

### 12. Anti-patterns
- God class, tight coupling

### 13. Exceptions vs Result
- Use both wisely

### 14. Async Issues
- Avoid unnecessary async

---

## 💡 Tips
- Use real experience in interviews
- Explain decisions and trade-offs

