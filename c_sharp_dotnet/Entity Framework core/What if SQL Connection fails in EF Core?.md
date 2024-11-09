## What if SQL Connection fails in EF Core?

How to implement connection resiliency? 

Why is this important? 

Use Case:

Azure SQL databases, similar to other cloud database services, are frequently moved between servers. 

This can lead to brief periods, possibly a 1s or so when they are momentarily inaccessible. 

Although this may not be a noticeable issue in apps with low usage, it can occasionally result in your database connections unexpectedly disconnecting or timing out. 

To overcome this issue, it would be best for you to try the database query again after waiting for a short duration.

1-EF Core has a built-in solution for that: 𝐄𝐧𝐚𝐛𝐥𝐞𝐑𝐞𝐭𝐫𝐲𝐎𝐧𝐅𝐚𝐢𝐥𝐮𝐫𝐞().
2-I used to use Polly until they release that feature for EF Core. Polly is still valid for SQL text

We can configure exactly how the 'EnableRetryOnFailure' option works (max retries, the delay between retries, etc), which enables resilient SQL connections.

Limitations: 

It won't help with errors that are not transient by nature, such as data validation errors or incorrect SQL queries.

Best Practices:

Always test how your application behaves with this feature enabled, particularly under scenarios that simulate transient failures. 

![image](https://github.com/user-attachments/assets/c198e9b9-d82b-40b6-8dc8-ecfdfedc5c4d)
