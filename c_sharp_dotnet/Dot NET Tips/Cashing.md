
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
