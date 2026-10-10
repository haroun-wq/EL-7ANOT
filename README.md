# 📦 تطبيق إدارة أسعار المتجر

تطبيق Flutter بسيط وسريع لإدارة أسعار متجر التجزئة مع مزامنة Firebase لحظية.

---

## 🗂️ هيكل المشروع

```
lib/
├── main.dart                  ← نقطة الدخول + Firebase init
├── theme.dart                 ← ألوان وأنماط التطبيق
├── models/
│   └── product.dart           ← نموذج المنتج
├── services/
│   └── product_service.dart   ← كل عمليات Firestore
└── screens/
    ├── home_screen.dart        ← الرئيسية + مسح الباركود
    ├── product_result_screen.dart ← عرض السعر بعد المسح
    ├── add_product_screen.dart ← إضافة / تعديل منتج
    └── product_list_screen.dart   ← قائمة كل المنتجات
```

---

## ⚙️ خطوات الإعداد

### 1. إنشاء مشروع Firebase
1. اذهب إلى https://console.firebase.google.com
2. أنشئ مشروعاً جديداً
3. فعّل **Firestore Database**
4. فعّل **Firebase Storage**
5. فعّل **Authentication** → Email/Password

### 2. ربط Flutter بـ Firebase
```bash
# تثبيت Firebase CLI
npm install -g firebase-tools
firebase login

# تثبيت FlutterFire CLI
dart pub global activate flutterfire_cli

# ربط المشروع (داخل مجلد التطبيق)
flutterfire configure
```
هذا سيُنشئ ملف `lib/firebase_options.dart` تلقائياً.

### 3. تحديث main.dart
بعد تشغيل `flutterfire configure`، عدّل `main.dart`:
```dart
import 'firebase_options.dart';

await Firebase.initializeApp(
  options: DefaultFirebaseOptions.currentPlatform,
);
```

### 4. تثبيت المكتبات
```bash
flutter pub get
```

### 5. إعداد Android
في `android/app/src/main/AndroidManifest.xml` أضف:
```xml
<uses-permission android:name="android.permission.CAMERA"/>
<uses-permission android:name="android.permission.INTERNET"/>
```

### 6. رفع Firestore Rules
```bash
firebase deploy --only firestore:rules
```

### 7. تشغيل التطبيق
```bash
flutter run
```

---

## 📱 الشاشات

| الشاشة | الوظيفة |
|--------|---------|
| الرئيسية | بحث نصي + زر مسح الباركود + آخر المنتجات |
| نتيجة المسح | عرض اسم المنتج والسعر بخط كبير |
| إضافة منتج | صورة + اسم + باركود + سعر + فئة |
| قائمة المنتجات | كل المنتجات مع بحث، تعديل، حذف |

---

## 🗄️ هيكل Firestore

```
products/
  {barcode}/          ← الباركود = Document ID
    name: String
    price: Number
    category: String
    image_url: String
    updated_at: Timestamp
```

---

## 🔒 الأمان

- كل مستخدم يحتاج حساباً للدخول
- أنشئ حسابات الموظفين يدوياً من Firebase Console
- عطّل "Allow new users to sign up" في Authentication Settings

---

## 🚀 بناء APK للتوزيع

```bash
flutter build apk --release
# الملف في: build/app/outputs/flutter-apk/app-release.apk
```

---

## 📦 المكتبات المستخدمة

| المكتبة | الغرض |
|---------|-------|
| `firebase_core` | تهيئة Firebase |
| `cloud_firestore` | قاعدة البيانات |
| `firebase_storage` | تخزين الصور |
| `flutter_barcode_scanner` | مسح الباركود |
| `image_picker` | التقاط الصور |
| `provider` | إدارة الحالة |
| `cached_network_image` | تحميل الصور بكفاءة |
