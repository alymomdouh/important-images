
ازاي تشغل ال LINQ Queries بطريقه Dynamic ؟

هقولك الاول علي المشكله ال قابلتني وازاي اتحلت 
شغال ع table مطبق عليه DataTables ولو متعرفش اي هي ال DataTable دي عباره عن library بتقدم features جميله ل ال table بتاعك زي ال Searching وال Sort وحاجات تانيه كتير المهم شغال ع DataTables بطريقه ال Server side 
علشان ال rows عندي كتير ففي علي كل كولم anchor tag بتعمل sort عن طريق الداتا ال في ال column ده ف انا علشان اعمل sort محتاج اعرف اي ال column ال انا دوست علي ال anchor tag بتاعه ومحتاج اعرف ال direction علشان اعرف هعمل sort ازاي ف جبت الحاجات دي من ال request وحفظتهم ف variables بس لما جيت اعمل OrderBy لقيت ان OrderBy ف ال LINQ لازم تعمل OrderBy ب Property عندك وانا عايز اعمل OrderBy عن طريق ال variables ال عملتها 
ف لقيت الحل ف اني انزل package اسمها System.Linq.Dynamic.Core ال package دي بتخليك تستخدم ال LINQ Dynamic وساعتها قدرت اديها ال variablels بتاعتي وقدرت اعمل ال Sort بتاعي 
طبعا ممكن تستخدم methods تانيه غير ال orderBy ودي ال tutorial بتاعتهم

https://dynamic-linq.net/
