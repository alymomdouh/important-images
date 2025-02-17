اكتر سؤال بيتكرر في أي Angular Interviews
-------------------------------------------
إيه هي Dependency Injection ؟!!!

بص السؤال ده مع انه سهل الاجابه عليه وعارف ان ليه اجابه محفوظه بس السؤال ده مهم تبقى فاهمه وفاهم اجابته ... لان السؤال ده بالتحديد الى قايم على اجابته حاليا معظم ال Frameworks سواء كانوا لل Frontend او لل Backend.

خالينا نجيب الموضوع من اول ال SOLID وبالتحديد حرف ال D الى هو ال Dependency Inversion Principle (DIP) .... ال DIP بيقولك ببساطه لو عندك High Leve Class عايز يكلم Low Level Class متخليهوش يعمل الكلام ده Direct وخليه يعمله من خلال Abstraction Layers .

 كلام كبير صح ... خلينا نبسطها ونقول مين كل واحد منهم 
1. ال High Level Class ده ممكن نقول انه ف Angular ال Component بتاعك الى مسئول عن انه يقوم ال UI والى بيتكون من ال Ts, SCSS, HTML بتوعك
2. ال Low Level Class وليكن مثلا ال Translation Service الى ف ال Angular
3. ال Abstraction Layer هتبقى ال injector 

خلينا نقول الاول قبل ال Angular لو انته هتعمل ده Native هتلقيك كنت بتروح تعمل instance من ال TranslateService عندك من خلال انك بتستعمل ال new وتعمل composition Relation زى الكود الى ف الصوره ... عارف ايه المشكله هنا
ده معناه إن الـ Component بتاعك مسؤول إنه يعرف إيه هى الـ Service ويعمله Instantiation بنفسه.

المشكلة؟
1. ال Hard Coupling: لو حصل تغيير في الـ Service، لازم تعدل في كل مكان مستخدم فيه.
2. ال Testability ميتة: لأنك مش هتعرف تعمل Mock بسهولة.

لكن Angular قالت STOP! كفاية كده يا باشا!
بدل الكلام ده كله، Angular بتقدمك الـ DI، اللي بيخلي الـ Framework نفسه هو المسؤول عن إدارة الـ Dependencies من خلال الـ Injector

طب ده بيحصل إزاي؟
1. ال Injector: هو اللي بيتحكم في كل حاجة ... وبيروح يعمل ال instance وبعمل ال referance من ال Service دى
2.ال Provider: بتقول للـ Injector إزاي يعمل Create للـ Dependency (Singleton, Factory, Value).

فوايد الـ DI؟
1. ال Decoupling: الكود خفيف ونضيف.
2. ال Reusability: نفس الـ Service ممكن تستخدمه في أكتر من مكان.
3. ال Scalability: لما التطبيق يكبر، هتفضل الدنيا واضحة ومنظمة.

حاجه اخيره ممكن تنفعك ... انا لما ب اشتغل مش بعمل Inject بشكل مباشر ل Service من بتوع angular حاجه مثلا برضوا زى ال translation الى فوق ... بروح احطها ف Service تانيه وبسميها مثلا language Service وبعد كده بستعمل ال service الى انا عملتها ... عارف ده ميزته ايه ... الميزه ان انا ضفت layer فوق الى موجوده ممكن احط فيها كل ال customizations الى انا عايزها قبل ما اروح اكلم ال last instance الى هيقوم بالمهمه ... دى حاجه انا بفضلها تحسبا لاى future change ممكن يحصل زى مثلا انك وانته ف نص المشروع لقيت انك محتاج تضيف Prefix قبل كل ال Keys ف لو انته مستعمل ال instance direct هتلقيك بتروح تلف على ال App كله تضيف ال Prefix لكن مع وجود ال Layer الزياده هتروح تعد فيها هى بس ... فاهم قصدى
