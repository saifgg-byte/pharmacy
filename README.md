# ملازم المرحلة الثانية صيدلة

تطبيق أندرويد للملازم والتبليغات لطلاب المرحلة الثانية - كلية الصيدلة.

## المطوّر
**Saif Adnan**

## التقنيات المستخدمة
- Kotlin + Jetpack Compose
- Room Database
- OkHttp + Retrofit
- Android SDK / Gradle

## المزايا
- الملازم والمحاضرات حسب المادة والفصل، مع قارئ PDF (تظليل، رسم، ملاحظات)
- الجدول الأسبوعي وتنبيهات قبل المحاضرة
- الامتحانات والتبليغات
- لعبة أسئلة للمراجعة
- مساعد ذكاء اصطناعي للشرائح
- بطاقة الطالب
- لوحة تحكم للأدمن

## السيرفر
التطبيق يعتمد على سيرفر REST API خاص، **وكود السيرفر مو موجود بهذا المستودع**. رابط السيرفر انحذف من الكود، وتحط رابط سيرفرك بنفسك.

### وين تغيّر الرابط
ملف واحد بس:

`app/src/main/java/com/saifadnan/pharmacynotes/data/ServerConfig.kt`

```kotlin
object ServerConfig {
    const val BASE_URL = "https://YOUR-SERVER-URL"
}
```
الرابط لازم يكون **HTTPS** وبدون `/` بالنهاية (الـ cleartext traffic مغلق بالـ manifest).

### المسارات اللي يتوقعها التطبيق
الاستدعاءات كلها بالملف `data/SupabaseClient.kt` (رغم الاسم، هو يكلم السيرفر الخاص مو Supabase).

__supabase كان السيرفر القديم للتطبيق لكن حولته الى سيرفر محلي على كمبيوتري__


| المسار | الوظيفة |
|---|---|
| `GET /health` | فحص أن السيرفر شغال |
| `/auth/*` | `signup`, `login`, `refresh`, `update-profile`, `switch-stage` |
| `/users` | إدارة المستخدمين |
| `/subjects` `/lectures` `/exams` `/schedule` | المواد والمحاضرات والامتحانات والجدول |
| `/broadcasts` `/holidays` `/stages` | التبليغات والعطل والمراحل |
| `/weekly-schedule` | صورة الجدول الأسبوعي |
| `/game/*` | أسئلة اللعبة |
| `/files/img` `/files/pdf` | رفع الصور والملفات |
| `/ai/*` `/chem/*` `/viz/*` | المساعد الذكي والكيمياء والرسومات |
| `/admin/*` | الإحصائيات وطلبات الحظر وإدارة الأدمنية |
| `/library-api/my-card` | بطاقة الطالب |

المصادقة JWT عبر `Authorization: Bearer <token>`، ويرسل التطبيق معها الهيدرز `X-Device-Id` و `X-Stage`.

## التشغيل محلياً

**المتطلبات:** [Android Studio](https://developer.android.com/studio)

1. افتح Android Studio
2. اختر **Open** وحدد مجلد المشروع
3. غيّر `BASE_URL` بملف `ServerConfig.kt`
4. اترك Android Studio يزامن الاعتماديات (Gradle sync)
5. شغّل التطبيق على محاكي أو جهاز فعلي

## بناء نسخة Release
اولا استخرج ملف src.zip بعدها:

التوقيع يُقرأ من متغيرات البيئة:

```
KEYSTORE_PATH=/path/to/my-upload-key.jks
STORE_PASSWORD=...
KEY_PASSWORD=...
./gradlew assembleRelease
```
الـ alias هو `upload`، والمسار الافتراضي للمفتاح `my-upload-key.jks` بجذر المشروع.

## ملاحظات
- إيميل الأدمن الرئيسي مكتوب بالكود بالملف `util/AdminConfig.kt`، غيّره حسب سيرفرك.
- رابط الدعم (تيليجرام) بالملف `ui/components/TelegramSupport.kt`.
