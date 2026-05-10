# 🔬 مختبر الهيثم — المنظومة المالية السحابية (Lab2k Cloud)

## هيكل الملفات

```
lab2k/
├── index.html              ← الواجهة الرئيسية (HTML)
├── css/
│   └── style.css           ← كل التنسيقات
├── js/
│   ├── app.js              ← منطق التطبيق + Firebase
│   └── firebase-config.js  ← (مرجع فقط — Config داخل app.js)
├── firestore.rules         ← قواعد أمان Firestore
├── firestore.indexes.json  ← فهارس Firestore
├── firebase.json           ← إعداد Firebase Hosting
├── vercel.json             ← إعداد Vercel
└── README.md               ← هذا الملف
```

---

## 🚀 خطوات الإعداد

### الخطوة 1 — إنشاء مشروع Firebase

1. افتح [https://console.firebase.google.com](https://console.firebase.google.com)
2. اضغط **Add project** وأعطِه اسماً (مثل `lab2k`)
3. فعّل **Google Analytics** أو تخطَّها

### الخطوة 2 — تفعيل Firebase Auth

1. في القائمة الجانبية: **Build → Authentication**
2. اضغط **Get started**
3. اختر **Email/Password** وفعّله ✅
4. اذهب إلى **Users** → **Add user** وأضف بريد وكلمة مرور للمستخدم

### الخطوة 3 — تفعيل Firestore

1. في القائمة: **Build → Firestore Database**
2. اضغط **Create database**
3. اختر **Start in production mode**
4. اختر المنطقة (مثلاً `europe-west1` للسرعة من ليبيا)

### الخطوة 4 — نسخ Firebase Config

1. **Project Settings** (⚙️) → **General** → **Your apps**
2. اضغط **</>** (Web app) → أعطِه اسماً → **Register app**
3. انسخ الـ `firebaseConfig` object

### الخطوة 5 — لصق Config في الكود

افتح `js/app.js` وابحث عن:

```javascript
const firebaseConfig = {
  apiKey:            "YOUR_API_KEY",
  ...
};
```

واستبدله بالـ config الخاص بك.

### الخطوة 6 — رفع قواعد Firestore

في Firebase Console → Firestore → **Rules**، الصق محتوى `firestore.rules`:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

---

## ☁️ النشر على Vercel

### الطريقة السهلة (Drag & Drop):
1. افتح [https://vercel.com](https://vercel.com) وسجّل دخول
2. اضغط **Add New → Project**
3. اضغط **Browse** وارفع مجلد `lab2k` كاملاً
4. اضغط **Deploy** ✅

### الطريقة عبر GitHub:
```bash
# 1. أنشئ repo جديد على GitHub
# 2. ارفع الملفات
git init
git add .
git commit -m "Lab2k Cloud initial"
git remote add origin https://github.com/username/lab2k.git
git push -u origin main

# 3. في Vercel → Import from GitHub → اختر الـ repo
```

---

## 🔐 إضافة مستخدمين جدد

في Firebase Console → Authentication → Users → **Add user**

كل مستخدم يرى بياناته الخاصة فقط (معزولة تماماً).

---

## 🔄 المزايا الجديدة في النسخة السحابية

| الميزة | النسخة القديمة | النسخة السحابية |
|--------|---------------|----------------|
| التخزين | localStorage (الجهاز فقط) | Firebase Firestore (السحابة) |
| المزامنة | لا توجد | تلقائية فورية بين الأجهزة |
| تسجيل الدخول | لا يوجد | Firebase Auth (بريد + كلمة مرور) |
| الأمان | لا يوجد | كل مستخدم يرى بياناته فقط |
| النشر | ملف HTML واحد | Vercel / Firebase Hosting |
| الكود | ملف واحد | ملفات منفصلة (HTML/CSS/JS) |

---

## 📝 ملاحظات تقنية

- الـ SDK مُحمَّل مباشرة من CDN الرسمي لـ Firebase (v10 modular)
- لا توجد `node_modules` أو `package.json` — لا حاجة لـ `npm install`
- الـ `type="module"` في `<script>` يجعل الكود يعمل كـ ES Module مباشرة في المتصفح
- جاهز للنشر الفوري — رفع المجلد على Vercel يكفي
