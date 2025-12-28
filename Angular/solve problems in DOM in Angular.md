 ````md
<div dir="rtl">

# التفاعل مع الـDOM في Angular: ليه هو موضوع حساس؟

Angular مش عايزك تتعامل مع الـDOM مباشرة، وده لسبب بسيط: هدفه دايمًا إنه يعمل ده بالنيابة عنك.

هو كـFramework، مسؤول عن:

- تحديث الـUI
- إدارة الـState بتاع التطبيق
- تحديد إمتى وإزاي يحصل تحديث للواجهة

لو اتعاملت مع الـDOM مباشرة، حتلاقي نفسك في مشاكل زي: الـState والـUI مش حيكونوا متزامنين، تحديثات بتيجي في وقت غلط، مشاكل في الأداء، وصعوبة مع الـServer-Side Rendering (SSR). عشان كده، Angular موفّرلك أكتر من طريقة آمنة.

---

## إمتى نحتاج نوصل لعناصر الـDOM أصلًا؟

Angular بيدير الواجهة بالـData Binding والـChange Detection، بس في حالات واقعية بنحتاج نعرف معلومات مش موجودة في الـState أصلاً، زي: قياس عرض العنصر (بالبكسل)، هل هو اتعمله render فعلًا؟ أو محتاجين نعمل focus أو scroll. إجابة الأسئلة دي بتكون في الـDOM نفسه، مش في الـState.

**الهدف**: نوصل للمعلومة من خلال Angular وفي الوقت المناسب، مش بالرجوع لـ:

```ts
document.querySelector(...)
````

عشان الـState يفضل متزامن والـUI يتحدث صح مع دعم الـSSR.

---

## أمثلة شائعة وطرق الحل في Angular

### 1️⃣ Focus أو Scroll بعد ما الصفحة تفتح

**الحل**: نستخدم `@ViewChild` مع `ElementRef` ونفذ الإجراء في `ngAfterViewInit`.

```ts
@ViewChild('emailInput') emailInput!: ElementRef;

ngAfterViewInit() {
  this.emailInput.nativeElement.focus();
}
```

### 2️⃣ قياس عرض أو ارتفاع عنصر

**الحل**: نستخدم `@ViewChild` للوصول لـ `ElementRef` ونقرأ منه `offsetWidth` أو `offsetHeight`.

```ts
@ViewChild('card') card!: ElementRef;

ngAfterViewInit() {
  const width = this.card.nativeElement.offsetWidth;
}
```

> ⚠️ ملاحظة: القياس ده ينفع في المتصفح بس، لازم يتمنع على السيرفر (SSR).

### 3️⃣ تغيير styles أو classes حسب حالة معينة

**الحل باستخدام Renderer2**: هو آمن ومتوافق مع SSR ومش مربوط بالمتصفح مباشرة، بدل ما نستخدم:

```ts
element.style.border = '1px solid red';
```

```ts
constructor(private renderer: Renderer2) {}

highlight(el: ElementRef) {
  this.renderer.addClass(el.nativeElement, 'error');
}
```

### 4️⃣ UI ديناميكي (Dialogs / Tooltips)

**الحل باستخدام ViewContainerRef**: ده أفضل لأنه بيتعامل مع Angular Views ومش محتاج `document.body.appendChild`، ومتوافق مع SSR.

```ts
constructor(private viewContainer: ViewContainerRef) {}

open(template: TemplateRef<any>) {
  this.viewContainer.createEmbeddedView(template);
}
```

### 5️⃣ استخدام مكتبات خارج Angular (Charts / Maps)

**الحل**: نستخدم `@ViewChild` لـ ElementRef بتاع العنصر الحاوي، ونبدأ تهيئة المكتبة جواه في `ngAfterViewInit`.

```ts
@ViewChild('chart') chart!: ElementRef;

ngAfterViewInit() {
  // initialize chart here
}
```

> ⚠️ مهم: تشغيل المكتبة يكون في المتصفح فقط عشان أغلب المكتبات بتعتمد على window.

---

## 📌 نقطة مهمة جدًا عن ElementRef

* `ElementRef.nativeElement = DOM حقيقي`
* لو استخدمتها لتغيير UI أو إضافة عناصر، Angular مش شايف التغييرات لانك فتحت stream مباشر مع الـ DOM وبكده كسرت الـ Change Detection.
* يفضل تستخدمها لما تكون محتاج تقرأ قيمة فقط.

---

## الخلاصة

الوصول لعناصر الـDOM في Angular مش هو المشكلة، المشكلة إننا نخرج برا الإطار اللي Angular موفّرهولنا.

لو حابب شرح أعمق وأمثلة أكتر:
**DOM Manipulation in Angular – The Complete Guide**

</div>
```
<img width="800" height="533" alt="image" src="https://github.com/user-attachments/assets/197f2e03-8e91-4a5c-8e41-a70568026e0c" />

[source url ](https://www.linkedin.com/posts/mnagi74_%D8%A7%D9%84%D8%AA%D9%81%D8%A7%D8%B9%D9%84-%D9%85%D8%B9-%D8%A7%D9%84%D9%80dom-%D9%81%D9%8A-angular-%D9%84%D9%8A%D9%87-%D9%87%D9%88-%D9%85%D9%88%D8%B6%D9%88%D8%B9-activity-7410711063282843648-sGng?utm_source=share&utm_medium=member_desktop&rcm=ACoAACAxZAMBTyGrbYm_-D9XMKFFF64DRrKZ8d0)

 

