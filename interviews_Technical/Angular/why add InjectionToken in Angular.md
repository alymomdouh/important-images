
سؤال بيتكرر في كل ال Angular Interviews
--------------------------
إهبد ... هما ليه اخترعوا ال InjectionToken فى Angular ؟!!

خلينا نقول ف الاول ان ال InjectionToken ده كذبه كبيره ... وانه زى حجات كتير ف Angular مش بنستعملها كتير او مش بتيجى على بالنا لما بنواجهه مشكله هو حلها ..... وده مش عيب فينا لان دى special cases ... بس خلينا برضوا نقول انه اختراع رايق جدا جدا.
 
زى ما انته عارف ان ال Angular عباره عن Class-Based Components وبرضوا انها قايمة على ال Dependency Injection (DI) واكيد اانته هارى ال Project بتاعك Services وعاملهم Injection ف كل Component بيجى قدامك.

 وزى ما انته شايف ال Services دى كلها Classes .... وطبعا لو مذاكر شوية SOLID هتاخد ف اول 5 دقايق ان ال Class لازم يبقى Responsible عن حاجه واحده بس .... وده ممكن يوصلك انك لو عندك 10 features كل واحده منهم ليها logic مختلف هيخليك تعمل 10 Services ... وبالتالى 10 refernce دا لو عامله طبعا ف ال root.

طب فكر كده هتعمل ايه لو عايز تعمل configurations وهتبقى عباره عن Object او حتى String او حتى Function بتجمع رقمين ... طب لو عايز تجيب حاجه من 3rd Party ..... كنت هتروح تعملهم Services .. طبعا مش هتحطهم ف ال component direct عشان تحقق ال Singleton 

صداع صح؟! 

عشان مطولش عليك .... ال InjectionToken هو مجرد Identifier (اسم مستعار) بتستعمله عشان تقول ل Angular لما حد يطلب الحاجة دي، روح هاتها من هنا .... 

من الاخر بيقولك كفايه Classes خلى الدنيا بسيطه وبلاش تدخل ال OOP ف الموضوع .... اشهر استعمالته لما يبقى عندك configurations او Multi Providers او لو هتجيب حاجه من 3rd Party .. بص على ال Attachments.

فى حاجه زياده اسمها ال factory .... ودى اقوى حاجه ف ال InjectionToken بعد ال Multi Providers .... ال factory دى بتخليك تعمل generate ل dynamic value ف ال runtime ودى خطيره جدا وبتنفع ف مواقع ال Trading جدا جدا او لو انته فاتح Socket connection وفى داتا كتير بتتغير وعايز تغير ال behaviour ف مكان ما.

اخر حاجه ... لو احتجت الـ Token في مكان معين، تقدر تستخدم الـ @Inject.

![image](https://github.com/user-attachments/assets/01b3d3c0-0ed8-4c7f-879a-71c0302943a7)

![image](https://github.com/user-attachments/assets/3ff2fd43-e28e-43c1-b9e9-9f576b11c6e7)

![image](https://github.com/user-attachments/assets/86c2e457-ff3c-4302-87e6-ef4f506955be)

![image](https://github.com/user-attachments/assets/5b8e58f3-2ab8-4160-a30e-c90687b6ecfc)


---------------------
الموضوع ابسط من دة كله
ال token ببساطة عبارة عن placeholder لل value الفعلية اللي هتتحقن في ال runtime اللي هي ممكن تكون class او اي value تانية من اي نوع

------------------
تكمله لكلامك ي هندسه لاستخدام ل inject function لازم يكون جوه ل injection context area بمعني لو انا مثلا ف ل Oninitمش هعرف اعمل inject
-------------
و لو هنقول السبب بطريقة تيكنيكالى شويه فال service creation cost على ال compilation process مرهقه عن ال injection Token لكن فعلا لو بصيت هتلاقي الاتنين غرضهم واحد و الاتنين بيتعاملوا ك injectable data ولا يتم استدعائهم الا في ال injection context.
