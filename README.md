# نظام تقارير الزيارات المدرسية — الإشراف التربوي

موقع صفحة واحدة للمشرف التربوي: تسجيل تقارير الزيارات، إدارة قائمة المدارس،
لوحة تحليل بيانات، وتصدير التقارير بصيغة وورد.

الملفات:

| الملف | الوصف |
|---|---|
| `index.html` | الموقع كاملاً (التصميم + الأكواد + الشعار مضمّن بداخله) |
| `firestore.rules` | قواعد الحماية التي تُلصق في Firebase |

يعمل الموقع فور فتحه دون إعداد، لكنه يحفظ البيانات في المتصفح فقط.
لمشاركة البيانات بين الأجهزة والحفظ الدائم، اتبع خطوات Firebase أدناه.

---

## 1. الرفع على GitHub

1. أنشئ مستودعاً جديداً باسم `school-visits` (Public).
2. ارفع فيه `index.html`.
3. من `Settings ← Pages`: اختر `Deploy from a branch`، الفرع `main`، المجلد `/ (root)`، ثم `Save`.
4. بعد دقيقة يصبح الرابط: `https://<اسم-حسابك>.github.io/school-visits/`

---

## 2. إنشاء مشروع Firebase

1. من [console.firebase.google.com](https://console.firebase.google.com) أنشئ مشروعاً جديداً.
2. `Build ← Authentication ← Get started ← Sign-in method ← Google ← Enable` ثم `Save`.
3. في `Authentication ← Settings ← Authorized domains` أضف نطاق الموقع:
   `<اسم-حسابك>.github.io` (وأضف نطاقك الخاص إن ربطت واحداً لاحقاً).
4. `Build ← Firestore Database ← Create database ← Production mode`، واختر المنطقة `eur3` أو `nam5`.
5. `Project settings ← Your apps ← Web (</>)` وسجّل تطبيق ويب، ثم انسخ كائن `firebaseConfig`.

---

## 3. ربط الموقع بالمشروع

افتح `index.html` وابحث عن `const CONFIG` قرب بداية الأكواد، ثم عدّل:

```js
const CONFIG = {
  supervisorName: "الأستاذ أمين عبيدي",

  // البريد المسموح له بالدخول — أزل علامة التعليق وضع بريد جوجل الخاص بالمشرف
  allowedEmails: [
    "amin@gmail.com",
  ],

  // الصق هنا ما نسخته من Firebase
  firebase: {
    apiKey: "AIza........",
    authDomain: "school-visits-xxxx.firebaseapp.com",
    projectId: "school-visits-xxxx",
    storageBucket: "school-visits-xxxx.appspot.com",
    messagingSenderId: "0000000000",
    appId: "1:0000:web:abcd"
  }
};
```

ارفع الملف بعد التعديل. عند فتح الموقع ستظهر شاشة الدخول بحساب جوجل،
وسيرفض النظام أي حساب غير المذكور في `allowedEmails`.

> `apiKey` ليس كلمة سر، ووجوده في ملف عام أمر طبيعي في Firebase.
> الحماية الحقيقية تأتي من قواعد Firestore في الخطوة التالية.

---

## 4. قواعد الحماية

من `Firestore Database ← Rules` الصق محتوى `firestore.rules` بعد استبدال
البريد بالبريد الحقيقي، ثم اضغط `Publish`. بدون هذه الخطوة تكون قاعدة
البيانات مفتوحة للجميع.

---

## 5. ربط نطاق خاص (اختياري)

إن أردت ربط نطاق من Hostinger كما في مشروعك السابق:
`Settings ← Pages ← Custom domain` في GitHub، ثم أضف سجلات DNS المطلوبة،
ولا تنسَ إضافة النطاق الجديد في `Authorized domains` داخل Firebase.

---

## ملاحظات على الاستخدام

- **المدارس**: أضف الأسماء أولاً من صفحة المدارس، ويمكن لصق قائمة كاملة دفعة واحدة عبر زر «إضافة قائمة».
- **تصدير وورد**: يخرج الملف بامتداد `.doc` ويفتح مباشرة في Word مع اتجاه من اليمين لليسار. يمكن تصدير تقرير مفرد، أو كل التقارير المعروضة بعد التصفية.
- **لوحة المعلومات**: كل الرسوم تتبع عوامل التصفية أعلى الصفحة. شريط الجاهزية يحسب **آخر** وصف عام مسجّل لكل مدرسة، لا كل الزيارات.
- **تعديل القوائم المنسدلة**: كل الخيارات معرّفة في أعلى الأكواد في المتغيرات `REASONS` و`STATUSES` و`NOTES` و`LEVELS` و`VISIT_TYPES`.
