
## 🖥️ Mastering EF Core Inheritance Mapping: TPH, TPT, and TPC 🔍

Entity Framework Core offers powerful strategies to map class hierarchies to relational databases. If you’ve ever wondered how to effectively manage inheritance in EF Core, let’s dive into the three main approaches:

#### 1️⃣ Table Per Hierarchy (TPH)

TPH uses a single table to store data for all types in the hierarchy. A discriminator column determines the specific type.

✅ Pros:

Simpler and faster queries with one table.

Easy to implement.

❌ Cons:

Can lead to sparse data when many columns are not shared among derived types.


#### 2️⃣ Table Per Type (TPT)

TPT creates separate tables for the base and derived classes, linked by a primary key.

✅ Pros:

Normalized structure and avoids NULL values.

❌ Cons:

Querying requires JOINs, which may impact performance.

#### 3️⃣ Table Per Concrete Class (TPC)

TPC creates a table for each concrete class, containing all properties (including inherited ones).

✅ Pros:

No shared table, making queries straightforward.

❌ Cons:

Redundant schema for shared fields.

![image](https://github.com/user-attachments/assets/db933775-ae64-42b2-8ad8-18b324f4abef)

![image](https://github.com/user-attachments/assets/3cacec9d-f773-4f5b-b13d-08222e818901)

![image](https://github.com/user-attachments/assets/5b2f7138-8994-4871-a28a-6a525105bd0a)

![image](https://github.com/user-attachments/assets/4eafa420-e309-4483-b309-9971a2869e7e)

![image](https://github.com/user-attachments/assets/7c9a7614-8a3e-4909-a244-0c01d54e283b)

![image](https://github.com/user-attachments/assets/8d0a1996-0cef-48dc-9929-18584090e3ff)

![image](https://github.com/user-attachments/assets/2909ddf5-d1fd-40d1-b899-db81ff51914b)




