ايه نوع ال DOM ف Angular ؟!!

طبعا انته اكيد لطمت زيي وقولت نهار ابيض هو طلع فى انواع لل DOM ... وقعدت طبعا تقول ف عقل بالك احنا واخدين واحنا صغيرين ان عندنا DOM و BOM و مستوره من وقتها ... مخدناش ان ليهم types.

انواع ال DOM دى طلعت حوار ... وطلع برضوا ان الاجانب بيعيبوا ف شغل الصنايعى الى قبلهم زينا كده ... المهم ايه الانواع
1. ال Incremental DOM
2. ال Optimized Static DOM
3. ال Shadow DOM
4. ال Virtual DOM
5. ال Real DOM

ال Angular بقى بتستعمل ال Incremental DOM ودى ليها شوية مميزات هى الى ادت لل Angular ال super-power بتاعتها ... الفكره بتاعتها شغاله على حاجتين اساسين
1. ال Template Compilation 
وده بيبنى ال template بشكل instructions ... ياعنى على سبيل المثال بيكتب instruction انه عايز 10 DIVs هيتبنوا هنا ... بالشكل ده مثلا
 ([IN] <div></div> 10)

2. Change Tracking
وده بيعمل update للحاجه الى عايزه تتغير بس ف ال DOM

 طيب هنا هيطلع حبايبنا بتوع ال library الى بيسموها React .. ويقولك ما احنا عندنا ال Virtual DOM بتعمل الكلام ده ... ف نقوله ونفهمه ان الميزه الاكبر ف ال Inc DOM انها مش بتتخزن ف ال memory زى ال Virtual DOM ... ال Inc DOM بتغير Direct ف ال Real DOM

طبعا دا تبسيط للحكايه ... بس الموضوع كبير والسؤال ده لولبى. 

![image](https://github.com/user-attachments/assets/4fc1cc3d-3e57-46a9-8c55-4eafb8c6e6cc)
