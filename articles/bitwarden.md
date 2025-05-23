# رمزنگاری در بیت‌واردن (Bitwarden)
## بِسْمِ اللَّـهِ الرَّحْمَـٰنِ الرَّحِيم
یکی از مشهورترین و شاید محبوب‌ترین خدمات ذخیره‌سازی گذرواژه‌ها در دنیا، Bitwarden است. این نرم‌افزار آزاد، همانطور که عقل حکم میکند رمز‌های شما را رمزنگاری میکند تا امنیت آنها را حفظ کند. در این نوشته هدفم شرحی اجمالی از نحوهٔ انجام این کار توسط بیت‌واردن است.

بیت‌واردن از رمزنگاری AES-CBC 256-bit برای اطلاعات گاوصندوق و از PBKDF2 SHA-256 یا Argon2 برای نگهداری کلید رمزنگاری شما استفاده میکند. بیت‌واردن همیشه اطلاعات شما را قبل از ارسال به کارساز (سرور) به صورت محلی در دستگاه خودتان رمز/هش میکند و بعد از آن اطلاعات را به کارساز ارسال میکند. یعنی در واقع کارسازهای بیت‌واردن فقط محلی برای ذخیرهٔ اطلاعات رمز/هش‌شده هستند. علاوه بر آن، بیت‌واردن از رمزگذاری داده‌های شفاف آژور هم در کارسازهایش استفاده میکند. همچنین باید به این نکته‌ هم اشاره کنم که بیت‌واردن از کارسازهای آژور مایکروسافت که در آمریکا یا اروپا میزبانی میشوند استفاده میکند و از خدمات آن برای نگهداری کارسازها استفاده میکند. برای اطلاعات بیشتر [این صفحه](https://bitwarden.com/help/data-storage/) را ببینید.

![Bitwarden](https://upload.wikimedia.org/wikipedia/commons/d/d0/Bitwarden_Desktop_MacOS.png)

کل اطلاعات گاوصندوق قبل از ارسال به جایی رمز میشوند. بیت‌واردن از روش رمزنگاری بدون-دانش استفاده میکند؛ یعنی شما تنها کسی هستید که به کلیدی که برای رمزگشایی اطلاعات لازم است دسترسی دارید. برای اطلاعات بیشتر [این صفحه](https://bitwarden.com/help/bitwarden-security-white-paper/#how-vault-items-are-secured) را ببینید.

همانطور که اشاره شد، روشی که بیت‌واردن برای رمزنگاری اطلاعات گاو صندوق استفاده میکند AES-CBC است. این روش، روش معیار دولت آمریکا و دیگر آژانس‌های دولتی دنیا برای محافظت از اطلاعات بسیار امنیتی است. با اجرای مطلوب و یک کلید رمزنگاری قوی (گذرواژهٔ اصلی در بیت‌واردن)، AES غیرقابل شکستن است.

برای تولید کلید رمزنگاری از روی گذرواژهٔ اصلی، از PBKDF2 SHA-256 استفاده میشود. البته شما میتوانید از Argon2 به عنوان جایگزین استفاده کنید. بیت‌واردن گذرواژهٔ اصلی را قبل از ارسال به کارساز با رایانه‌تان بصورت محلی salt و هش میکند. زمانیکه کارساز بیت‌واردن گذرواژهٔ هش‌شده را دریافت میکند، کارساز دوباره با یک مقدار تصادفی کریپتوگرافی دوباره salt و هش میکند؛ سپس آن را ذخیره میکند.

تکرار استفاده‌شده برای PBKDF2 در سمت کارخواه (کلاینت) ۶۰۰۰۰۱ بار است؛ یعنی در کارخواه ۶۰۰۰۰۱ بار هش اتفاق میافتد. این مقدار در فوریهٔ ۲۰۲۳ افزایش یافت و به این مقدار رسید. البته میتوانید در تنظیمات این مقدار را تغییر دهید. بعد از آن ۱۰۰۰۰۰ بار هم هش در سمت کارساز اتفاق میافتد. در نهایت کلید سازمانی (organization؟) با RSA-2048 همرسانی میشود. همانطور که عقل سلیم میگوید (😄) این الگوریتم‌های هش یک‌طرفه هستند. یعنی با داشتن مقدار هش‌شده مقدار اصلی محاسبه نمیشود؛ در نتیجه اگر بیت‌واردن هک هم شود گذرواژهٔ اصلی لو نمیرود.

Argon2، برندهٔ مسابقات هش گذرواژهٔ ۲۰۱۵ حایگزینی برای PBKDF2 است. این الگوریتم ۳ نگارش دارد؛ بیت‌واردن نگارش Argon2id را استفاده میکند همانطور که OWASP توصیه کرده. این نگارش الگوریتم ترکیبی از دیگر نگارش‌هاست.از ترکیبی از دسترسی‌های وابسته به اطلاعات و غیروابسته به اطلاعات استفاده میکند که به آن بخشی از مقاومت Argon2i را در مقابل حملات side-channel cache timing و بسیاری از مقاومت Argon2d  در مقابل حملات GPU cracking را میدهد.

بطور پیشفرض بیت‌واردن تنظیم شده که ۶۴ مگابایت حافظه و ۳ تکرار با ۴ رشته برای این الگوریتم اختصاص دهد که بالاتر از توصیه‌های OWASP است؛ ولی باید دو نکتهٔ زیر را بدانید:
1. افزایش تکرار KDF بصورت خطی زمان محاسبه را افزایش میدهد.
2. موازی‌کاری KDF بستگی به پردازندهٔ دستگاه شما دارد. حداکثر ۲ برابر تعداد هستهٔ پردازنده.

نکتهٔ آخر اینکه بیت‌واردن هیچ کتابخانهٔ رمزنگاری‌ای را خودش ننوشته و از کتابخانه‌های محبوب و مطمئن که توسط متخصصان رمزنگاری توسعه میابند استفاده کرده است. مثلا:
- جاوااسکریپت (وب، مرورگر، میزکار و خط فرمان): Web crypto - Node.js crypto - Forge
- سی‌شارت (موبایل): CommonCrypto - Javax.Crypto -BouncyCastle

---
این نوشته برگرفته از [این صفحه](https://bitwarden.com/help/what-encryption-is-used/) است.