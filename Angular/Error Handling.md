
![image](https://github.com/user-attachments/assets/0e95a25a-1d52-45e6-bab9-9f33200ac3a0)

سؤال انترفيو مهم جداااا وديما هتدخل تتسأل فيه ⚠️ ⛔ 

اذاي بـ Error Handling ... أذاي بنتعامل مع ال Errors في الأنجولار ؟؟!

 تعالي كدا بقي ندردش شوية ونجيب الموضوع من تحت خالص سريعا كدا... 
 

 => يعني إيه Error Handling في Angular؟
 
ببساطة كدا إنك تكون مستعد لأي غلطة أو مشكلة تحصل في الكود بتاعك، زي مثلا لما تعمل Call Api للسيرفر وميرجعش حاجه او السيرفر يرد بـ Error , او حتي غلطة حصلت من المستخدم وهو بيدخل بيانات مثلا! الفكرة بقي انك تمسك الخطأ دا وتتصرف معاه عشان تطبيقك يفضل شغال بشكل طبيعي من غير مايعطل او يهنج.


 => جميل جدا عرفنا ال Concept نيجي بقي نشوف أهم الادوات اللي نقدر نهندل بيها الأحطاء واللي الانجولار بتوفرها لينا :-
 

* أولا : Global Error Handler :
* 
هنا بقي يصديقي بنعمل كلاس ونروح نـ "implements "ErrorHandler ومن خلاله نقدر نستخدم الميثود المشهورة handleError(error:Error) وبكدا الكلاس دا بيسمحلنا اننا نمسك اي error يحصل في التطبيق كله لاننا بنروح نحطه في ال providers عشان يبقي جلوبال علي التطبيق كله .


* ثانيآ : HTTP Interceptor :
* 
هنا بقي نقدر نتعامل مع الأخطاء اللي بترجع من السيرفر , يعني لو السيرفر رد عليك بـ 404 او 500 او 401، الانترسبتور ده هيمسكها لانه بمثابة الحارس بتاع الفيلا ونقدر نتعامل معاه ونقدر كمان نظهر رسالة لطيفة للمستخدم انه Unauthorized مثلا وكمان نقدر نبعته ع شاشة الـ Login.


* ثالثآ : Error Handling مع الـ Observables :
* 
هنا بقي بنتعامل مع الأخطأ في البيانات اللي ببتعامل مع asynchronous عن طريق

 catchError Operator اللي بيوفرهالنا RxJS , طيب اي بيحصل بدون catchError ؟؟
 
 
لما تطلب بيانات من API، لو حصل خطأ (زي إن السيرفر مش شغال أو الإنترنت فصل)، التطبيق ممكن "يكراش" أو يطلع أخطاء في الـ Console بدون أي تحكم منك.

طيب اي بقي بيحصل مع الــcatchError : نقدر نمسك الخطأ وكمان تعرض رسالة مفهومة للمستخدم بدل الكود المعقد.

تقدر ترجع حاجة بديلة (Fallback Data) لو مفيش بيانات. كمان بيمنع التطبيق من الـ Crash, كمان عندنا retry و retryWhen .


* رابعآ : تسجيل الأخطاء (Error Logging) :
* 
 هنا بقي علشان تقدر تتبع الأخطاء اللي بتحصل عند المستخدمين ونقدر نرجع لها ونحل المشاكل بتاعتها .


* خامسآ : Routing Errors :
* 
 هنا بقي لو المستخدم دخل على صفحة مش موجودة، يتم توجيهه لصفحة NotFoundComponent. او اي شاشة تهندل ان الصفحة غير متوفرة في التطبيق.

 
* سادسآ : Component Error :
* 
هنا بقي للمشاكل اللي في ال Component نفسه زي لو بتعامل مع Form ف أقدر اعمل validation او لو خايف يحصل اي مفاجأت ممكن نستخدم try-catch واقدر اعرض للمستخدم رسائل واضحة .


 🔻وأخيرا ياصديقي معالجة الأخطاء في Angular مش مجرد إضافة أكواد، لكنها خطوة أساسية لتحسين استقرار التطبيق وتجربة المستخدم.
 
🔻تقدر تشوف مثال لكل حاجه اتكلمت عنها في اللينك دا وأفتكر ان دا مش كل حاجه انا مجرد حاولت اعمل Highlight بشكل سطحي وانت بدورك تتعمق أكتر وتكتشف أكتر 🥰

https://lnkd.in/dFmmcdd8



