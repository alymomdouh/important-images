
سؤال بـ 100 لون وبيتكرر في كل ال Angular Interviews
---------------------------------
ازاى تعمل Custom Form Component ؟!!

خلينا ف البدايه نتفق على حاجه صغيره .... طول ما انته بتشتغل SPA متكررش HTML Template مرتين ف نفس ال File إلا لو كان leaf node ... فاهم قصدى؟!

انته اكيد عارف ان عندنا نوعين لل Forms ف Angular 
1. ال Template-Driven Form .... ودى بتعمل instance من ال ngForm وبتبنى كل الى تقدر عليه ف ال HTML وطبعا صديقك فيها ال ngModel

2. ال Reactive Forms ... ودى قايمه على ال formGroup وال formControlName

طبعا الى فوق ده اختصار مخل بالموضوع بس عشان انا عايز اركز على الى جاى .... اصحى

خلينا نقول انك عايز تعمل File Uploader ... بس هتعمله بشكل نضيف شويه من الى بنشوفهم ف الافلام .... لو هتستعمل ال TDF ف انته هيبقى عندك كده file Input ده الى هتحط عليه ال ngModel بتاعتك وتعمله bind ... بس ده لو هيبقى واحد بس ... طيب لو اكتر من واحد ف انته هتروح هنا تعمل component وبستخدام ال Input و ال Output و ال EventEmitter مع ال ngModelChange وهتعدل الداتا ف الاتجاهيين

 وطبعا لو هتعمل ب ال Reactive Form ف برضوا ممكن يجى ف بالك نفس السيناريوا وتعمل نفس اللفة الطويله.

السؤال المهم هنا .... هو مفيش حاجه احسن واسهل من اللفه دى خصوصا لو الموضوع بقى complex اكتر .... الاجابه كانت اختراع عظيم اسمه ال ControlValueAccessor 

مين بقى ال ControlValueAccessor ؟!!!
دى عباره عن Interface وظيفتها انها تعملك custom form component من غير كل اللف والعجن الى فوق ده .... وظيفتها انها تقدر تخليك تستعمل ال formControlName على ال Component بتاعك زى ما بتستعملها على ال native form components وده من خلال 4 حجات بتعملهم 

1. بتعمل writeValue وده المسئول عن انه يبقى شايل ال value الحاليه سواء كانت جايه من ال parrent او طالعه من ال component 
2. بتعمل registerOnChange ودى عشان لو حصل change ف ال value ... بتشغل ال Two-Way-Biniding 
3. بتعمل registerOnTouched ... ودى هتحتاجها ف ال errors وال validations كتير
4. setDisabledState ودى عشان لو هتعمل disable لل component


تعرف الميزه الجباره لل ControlValueAccessor بتظهر لما تيجى تتعامل مع Form Array  ... جربها كده لو مكنتش عملتها قبل كده وانته هتدعيلى

طبعا فى كلام تانى فيها هتلقيه ف ال Attachments كا بدايه


![image](https://github.com/user-attachments/assets/c2b36b44-854c-42a0-8192-021144c2e0ec)

![image](https://github.com/user-attachments/assets/a7bd9633-7f08-41d3-aee9-d0583801fced)
