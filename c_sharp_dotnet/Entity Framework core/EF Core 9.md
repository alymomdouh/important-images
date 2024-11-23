##  𝐖𝐡𝐚𝐭 𝐢𝐬 𝐭𝐡𝐞 𝐁𝐞𝐬𝐭 𝐖𝐚𝐲 𝐭𝐨 𝐒𝐞𝐞𝐝 𝐃𝐚𝐭𝐚 𝐢𝐧 𝐄𝐅 𝐂𝐨𝐫𝐞 ?

EF 9 has brought a new way of seeding data.

See it here 👇 

Before EF 9 (EF Core 9) you needed to create separate services, resolve and call them before the application was started. Either directly in Program.cs or in a Hosted Service.

EF 9 brings a new way to seed your database. And this approach is really the best one.

You can use: 𝐔𝐬𝐞𝐒𝐞𝐞𝐝𝐢𝐧𝐠 and 𝐔𝐬𝐞𝐒𝐞𝐞𝐝𝐢𝐧𝐠𝐀𝐬𝐲𝐧𝐜 methods directly in the 𝑂𝑛𝐶𝑜𝑛𝑓𝑖𝑔𝑢𝑟𝑖𝑛𝑔 method in your DbContext on when registering DbContext in DI.

My preferable way is to call 𝐔𝐬𝐞𝐒𝐞𝐞𝐝𝐢𝐧𝐠𝐀𝐬𝐲𝐧𝐜 method when Adding DbContext.

How do you seed data in EF Core ? Will you consider using a new approach?
Let me know in the comments below.

![image](https://github.com/user-attachments/assets/2f9cdd1f-faee-4517-9734-a678b355104c)


