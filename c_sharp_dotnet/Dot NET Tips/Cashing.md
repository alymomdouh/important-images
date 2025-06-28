
![image](https://github.com/user-attachments/assets/91970931-ddd1-4d1c-9d76-b41d8a80da25)



إي هو الـ Cashing وليه مهم تكون عارفه؟ وإستخداماته.  


لما بنيجي نتكلم عن البيانات Data لأي تطبيق شغالين عليه، أول حاجة بتيجي في دماغنا دايمًا هي قاعدة البيانات Database


السؤال الطبيعي بيكون هخزن البيانات فين؟ 

والإجابة المعتادة في الـDatabase زي Sql Server أو حتى MongoDB.


ده طبيعي لإن إحنا متعودين إن البيانات بتتخزن

 على Hard Disk عن طريق Database Engine
 
والبيانات دي بنستدعيها بالكود اللي بيعمل Query عشان يرجع البيانات اللي محتاجينها.


 الكلام ده شغال كويس لما الـ Queries تكون بسيطة وعدد المستخدمين قليل.
 
 ‏
 ‏لكن لما الضغط يزيد أو الطلبات تبقى معقدة زي عمليات الـJoin في SQL السرعة بتقل جدًا، وهنا بييجي دور 
الـ Caching

يعني إيه Cashing ؟

الـ Cashing هو ببساطة مكان مؤقت بنخزن فيه البيانات اللي بيتم استخدامها كتير، وبدل ما نروح للـDatabase كل مرة، بنجيب البيانات من الكاش اللي بيبقى موجود في الذاكرة Memory وده بيوفر وقت ومجهود كبير جدًا.

ليه الـ Cashing مهم؟

1️⃣ علشان سرعة الاستجابة Response Time

لما البيانات تبقى في الكاش بدل الـDatabase، الاستجابة بتكون أسرع بكتير لأن الميموري أسرع من الهارد ديسك.

لو عملية استعلام من الـDatabase بتاخد 1-3 ثواني، نفس العملية من الكاش ممكن تاخد أقل من 50 ملي ثانية.

2️⃣ تقليل الحمل على الـDatabase

الكاش بيقلل عدد الطلبات اللي بتروح للـDatabase، وده بيخلي السيستم يقدر يشيل عدد مستخدمين أكتر.

3️⃣ كمان التعامل مع الضغط العالي High Traffic

وقت الضغط، زي مثلاً إعلان نتيجة الثانوية العامة أو نتيجة الكلية، الكاش بيمنع السيستم إنه ينهار وبيسرّع استجابة الموقع.

وعندنا أمثلة لاستخدام الكاش. 

1. تسريع الـ Queries


لو عندك بيانات مش بتتغير كتير زي نتائج الطلاب ممكن تخزنها في الكاش.

 بدل ما تعمل استعلام من الـDatabase كل مرة، بتجيبها من الكاش بشكل أسرع وأخف على السيستم.
 ‏
كمان تقدر تستخدمها في ال Rate Limiter عشان تحدد عدد الطلبات اللي المستخدم يقدر يعملها في وقت معين.

إزاي؟

بتسجل المستخدم في الكاش مع عدد الطلبات اللي عملها آخر دقيقة.

لو المستخدم عدى الحد المسموح زي 5 طلبات في الدقيقة بتمنعه وترجعله رسالة خطأ 429 Too Many Requests

بكده أنت بتحمي السيستم من هجمات

 زي Denial of Service 
 

أنواع الـ Cashing ؟

1️⃣ In-Memory Caching

وفي النوع ده بيتخزن جوه ذاكرة السيرفر اللي الابلكيشن شغال عليه. 

كل سيرفر ليه كاش خاص بيه، ولو عندك أكتر من سيرفر، كل سيرفر هيبقى معزول عن التاني.

مناسب للأنظمة اللي شغالة على سيرفر واحد بس، ومش محتاج تشارك بيانات الكاش.


2️⃣ Distributed Caching

هنا الكاش بيتخزن في مكان مركزي زي Redis أو SQL Server ، وكل السيرفرات بتقدر تشارك نفس الكاش، فلو سيرفر عمل كاش لبيانات، السيرفرات التانية تقدر تستخدمها.

بيشتغل كويس لما عندك أكتر من سيرفر وفي load balancer وبيضمن إن كل السيرفرات تشوف نفس البيانات.

وده محتاج بوست منفصل نتكلم عنه إن شاء الله.

لو ما عندكش فكرة عن الكاش أو مش بتستخدمه، فده معناه إنك بتضيع فرصة كبيرة لتحسين أداء التطبيق بتاعك. 

لو تعاملت مع الـ Cashing قبل كده فشاركنا في الكومنتات إي الـ Tools إللي استخدمتها؟ 

ولو البوست مفيد متنساش تشيره لعل غيرك يستفيد.

والسلام عليكم ورحمة الله وبركاته.👋


[How to Implement Redis Cache in .NET Core Web API](https://www.voidgeeks.com/tutorial/How-to-Implement-Redis-Cache-in-NET-Core-Web-API/23)

-----------------------------------------------------------------------------------------------------
## 💡 .𝗡𝗘𝗧 𝗧𝗶𝗽 - 𝗖𝗮𝗰𝗵𝗶𝗻𝗴 𝗦𝘁𝗿𝗮𝘁𝗲𝗴𝗶𝗲𝘀 𝗘𝘃𝗲𝗿𝘆 𝗕𝗮𝗰𝗸𝗲𝗻𝗱 𝗗𝗲𝘃𝗲𝗹𝗼𝗽𝗲𝗿 𝗦𝗵𝗼𝘂𝗹𝗱 𝗞𝗻𝗼𝘄

🔥 Not all cache strategies are built the same. Some are designed for speed. Others make sure your data stays fresh and consistent.

The important part? Choosing the right one based on how your system reads and writes data.

Here are 5 caching strategies every backend developer should be familiar with: 

## 1. Cache-Aside 

This one’s simple: your app checks the cache first. If it’s not there, it grabs the data from the database and then adds it to the cache.

It works well for read-heavy systems but comes with a trade-off, the first request hits the database and might be slower. Also, data can get stale unless you manage expiration carefully.

✅ 𝗜𝗱𝗲𝗮𝗹 𝘄𝗵𝗲𝗻: systems that read the same data frequently.

## 2. Read-Through

Your app never talks to the database directly. Instead, the cache handles it. If the cache misses, it fetches from the DB behind the scenes and returns the result.

This adds a nice layer of abstraction, but you’ll need more logic in your cache layer. And like Cache-Aside, there’s still a risk of stale data.

✅ 𝗜𝗱𝗲𝗮𝗹 𝘄𝗵𝗲𝗻: apps with heavy read traffic where simplicity on the app side is a win.

## 3. Write-Through

Every write goes through the cache, and the cache pushes it to the database synchronously. It keeps things consistent, but at a cost.

Your writes will be a bit slower, and your cache might grow quickly if you’re not careful.

✅ 𝗜𝗱𝗲𝗮𝗹 𝘄𝗵𝗲𝗻: low-write systems where data freshness is critical.


## 4. Write-Back (or Write-Behind)

Here, your app writes data to the cache, and the cache pushes it to the database later, asynchronously.

It’s fast and efficient, but risky. If the cache crashes before syncing, you could lose data.

✅ 𝗜𝗱𝗲𝗮𝗹 𝘄𝗵𝗲𝗻: high-throughput, write-heavy workloads where performance matters more than immediate consistency.

## 5. Write-Around

Skip the cache on writes. Just go straight to the database. The cache only gets updated when that data is actually read.

You avoid unnecessary cache bloat, but there’s a downside, the first read after a write will be slower.

✅ 𝗜𝗱𝗲𝗮𝗹 𝘄𝗵𝗲𝗻: large datasets that change infrequently.

✨ Don’t just add caching, choose the strategy that fits your workload. It can make or break your app’s performance, reliability, and cost.

💬 What 𝗰𝗮𝗰𝗵𝗶𝗻𝗴 𝘀𝘁𝗿𝗮𝘁𝗲𝗴𝘆 are you using in your projects? Drop in the comments! 👇 


![image](https://github.com/user-attachments/assets/ddd1a095-0324-4e18-914f-2244e76605d7)
