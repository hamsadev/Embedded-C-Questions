<div dir="rtl">

# مولفه‌های متغیرها و ورود به دنیای پوینترها

خب دوستان، توی دنیای برنامه‌نویسی C هر متغیری چند تا مولفه داره که بعضی وقت‌ها توی کدنویسی بهشون نیاز داریم (CompileTime) و بعضی وقت‌ها فقط وقتی برنامه اجرا می‌شه (RunTime) به کار میان. این مولفه‌ها شامل اسم، نوع داده، مقدار، آدرس، سایز و محدوده هستن. حالا بریم به ترتیب همه رو بررسی کنیم و به یه جای هیجان‌انگیز برسیم: **پوینترها!**

## مولفه‌های یک متغیر

### اسم (Name)
همون اسمی که بهش می‌گیم متغیر! وقتی داریم کد می‌نویسیم به این اسم نیاز داریم تا بتونیم از متغیر استفاده کنیم. مثلاً:

<div dir="ltr">

```c
int num;
```
</div>

اسم متغیر اینجا `num` هست. در زمان نوشتن کد و کامپایل شدن، اسم متغیر برای برنامه‌نویس مفهوم داره ولی وقتی برنامه اجرا بشه دیگه چیزی به اسم "اسم متغیر" برای CPU معنی نداره.

### نوع داده (Data Type)
نوع داده مشخص می‌کنه که این متغیر چه نوع داده‌ای می‌تونه توی خودش جا بده. مثلاً اعداد صحیح؟ اعداد اعشاری؟ کاراکترها؟ نوع داده به CPU کمک می‌کنه که بدونه چطور با متغیر رفتار کنه. مثل:

<div dir="ltr">

```c
float temperature;
```
</div>

وقتی نوع داده مشخص می‌شه، CPU دقیقاً می‌فهمه که این متغیر چند بایت فضا نیاز داره و چطور باید با اون برخورد کنه. مثلاً یک عدد صحیح ۴ بایت فضا نیاز داره.

### مقدار (Value)
مقدار متغیر همون چیزیه که داخل متغیر ذخیره می‌کنیم. در هنگام اجرای برنامه می‌تونیم مقدار متغیر رو تغییر بدیم. به این شکل:

<div dir="ltr">

```c
num = 10;
```
</div>

### آدرس (Address)
آدرس! اینجا همون جاییه که برنامه متغیرمون رو توی حافظه قرار می‌ده. وقتی برنامه اجرا می‌شه، متغیر توی رم یه جایی ذخیره می‌شه و اون آدرس توسط CPU استفاده می‌شه تا بتونه متغیر رو پیدا کنه. مثلاً برای اینکه آدرس متغیر `num` رو بدست بیاریم از این استفاده می‌کنیم:

<div dir="ltr">

```c
printf("آدرس متغیر num: %p\n", &num);
```
</div>

### سایز (Size)
سایز هم که مقدار فضاییه که متغیرمون توی حافظه اشغال می‌کنه. این مقدار به نوع داده بستگی داره. مثلاً:

<div dir="ltr">

```c
printf("اندازه int: %zu\n", sizeof(int));
```
</div>

این سایز بسته به نوع داده مشخص می‌شه، مثلاً یک `int` در بیشتر معماری‌ها ۴ بایت فضا اشغال می‌کنه.

### محدوده (Scope)
اینکه کجاها می‌تونیم از متغیرمون استفاده کنیم بستگی به محدوده یا همون Scope داره. اگه متغیری رو توی تابعی تعریف کنیم، فقط همون تابع بهش دسترسی داره. اما اگه متغیر گلوبال باشه، کل برنامه می‌تونه به اون دسترسی داشته باشه.

---

## بیایم فکر کنیم: چرا این کد قرار نیست کار کنه؟

خب بچه‌ها، فرض کنین یه متغیر `num` داریم و می‌خوایم با یه تابع به نام `duplicate` مقدارش رو دو برابر کنیم. به نظر شما این کد کار می‌کنه؟


<div dir="ltr">

```c
void duplicate(int a) {
    a * 2;
}

int main(void) {
    int num = 10;
    duplicate(num);
    printf("مقدار num: %d\n", num);
}
```
</div>

به نظر شما چرا این کد کار نمی‌کنه؟ اگه برنامه رو اجرا کنیم، `num` همچنان مقدار ۱۰ رو داره و تغییری نمی‌کنه. ولی چرا؟

### جواب: نه! این کد کار نمی‌کنه. ولی چرا؟

خب ببینید، توی این کد، `a` فقط یه **کپی** از مقدار `num` هست، یعنی اگه توی تابع `duplicate` مقدار `a` رو عوض کنیم، این تغییر فقط روی همون کپی اعمال می‌شه و تأثیری روی متغیر اصلی یعنی `num` نداره. این بهش می‌گن **پاس با مقدار (Pass by Value)**.

توی Pass by Value، ما یه کپی از متغیر به تابع می‌فرستیم. یعنی هر تغییری که توی تابع روی اون متغیر انجام بدیم فقط روی کپی تأثیر داره و اصلاً به متغیر اصلی کاری نداره.

حالا که این رو فهمیدیم، بیایم چند تا راه‌حل ممکن رو بررسی کنیم.

---

## حالا چطور درستش کنیم؟

### راه‌حل 1: بازگشت مقدار از تابع

یک راه‌حل ساده اینه که تابع رو طوری بنویسیم که مقدار دو برابر شده رو برگردونه و بعد توی `main` این مقدار رو دوباره به `num` اختصاص بدیم:

<div dir="ltr">

```c
int duplicate(int a) {
    return a * 2;
}

int main(void) {
    int num = 10;
    num = duplicate(num);  // مقدار دو برابر شده به num اختصاص داده می‌شه
    printf("مقدار num: %d\n", num);
}
```
</div>

این کد درست کار می‌کنه، ولی مشکل اینجاست که اگه توابع دیگه‌ای هم داشته باشیم که بخوایم مستقیماً روی متغیرهای اصلی تأثیر بذاریم، ممکنه هر بار لازم باشه این مقدار رو دستی دوباره به متغیر اختصاص بدیم.

### راه‌حل 2: استفاده از متغیر گلوبال

یه راه‌حل دیگه اینه که متغیر رو گلوبال تعریف کنیم تا تابع `duplicate` بتونه مستقیماً به اون دسترسی داشته باشه:

<div dir="ltr">

```c
int num = 10;  // تعریف متغیر به صورت گلوبال

void duplicate() {
    num = num * 2;  // تغییر مستقیم متغیر گلوبال
}

int main(void) {
    duplicate();  // مقدار num دو برابر می‌شه
    printf("مقدار num: %d\n", num);
}
```
</div>

این روش هم جواب می‌ده، ولی ممکنه با داشتن متغیرهای گلوبال توی پروژه‌های بزرگ به مشکل بخوریم. چون متغیرهای گلوبال می‌تونن باعث تداخل و تغییرات ناخواسته بشن.

---

## راه‌حل طلایی: پوینترها و Pass by Reference

خب حالا راه‌حل دیگه‌ای داریم که می‌تونه هم کد رو مرتب‌تر کنه و هم مشکلات قبلی رو نداشته باشه: **پوینترها**.

تو این روش، به جای اینکه مقدار `num` رو به تابع بدیم، **آدرس** اون رو می‌فرستیم. وقتی آدرس رو به تابع می‌دیم، تابع می‌تونه مستقیماً به متغیر اصلی دسترسی داشته باشه و تغییرات لازم رو اعمال کنه. به این روش می‌گن **Pass by Reference**.

### پیدا کردن آدرس متغیر

آدرس متغیر رو می‌تونیم با استفاده از عملگر `&` بدست بیاریم. مثلاً:

<div dir="ltr">

```c
int num = 10;
printf("آدرس num: %p\n", &num);
```
</div>

---

## تعریف پوینتر

حالا که آدرس `num` رو داریم، می‌تونیم اون رو توی یه **پوینتر** ذخیره کنیم. پوینتر یه متغیر خاصه که به جای نگه داشتن مقدار، آدرس یه متغیر رو نگه می‌داره.

برای اینکه یه پوینتر `int` تعریف کنیم که آدرس `num` رو نگه‌داره، به این شکل عمل می‌کنیم:

<div dir="ltr">

```c
int *ptr = &num;
```
</div>

حالا `ptr` یه پوینتره که آدرس متغیر `num` رو نگه‌می‌داره. یعنی به جای اینکه مقدار `num` رو نگه داره، به خونه‌ای از حافظه اشاره می‌کنه که `num` اونجا قرار داره.

---

### استفاده از پوینتر برای تغییر مقدار

برای اینکه مقدار متغیر رو از طریق پوینتر تغییر بدیم، از عملگر `*` استفاده می‌کنیم. این عملگر به **محتویات** آدرس اشاره می‌کنه:

<div dir="ltr">

```c
*ptr = 20;  // مقدار num از طریق پوینتر تغییر می‌کنه
printf("مقدار جدید num: %d\n", num);  // خروجی: 20
```
</div>

---

## استفاده از پوینترها با داده‌های `stdint.h`

بیاید یک مثال ساده‌تر بزنیم. فرض کنید یه متغیر `uint8_t` داریم. آدرس اون رو چطور می‌گیریم و از طریق پوینتر مقدارش رو تغییر می‌دیم؟ اینطوری:

<div dir="ltr">

```c
#include <stdint.h>

uint8_t val = 255;
uint8_t *ptr = &val;

*ptr = 100;
printf("مقدار جدید val: %u\n", val);  // خروجی: 100
```
</div>

---


## تمرین: تابعی برای دو برابر کردن مقدار از طریق آدرس

حالا که درک نسبتا خوبی از پوینتر و نحوه عملکردش داریم، ازتون می‌خوام تابعی بنویسید که آدرس یه متغیر `int` رو بگیره و مقدار اون رو دو برابر کنه. برای این کار باید از **پوینترها** استفاده کنید.

<div dir="ltr">

```c
void duplicate(int *ptr) {
    *ptr = *ptr * 2;
}

int main(void) {
    int num = 10;
    duplicate(&num);
    printf("مقدار جدید num: %d\n", num);  // خروجی: 20
    return 0;
}
```
</div>

## پوینترها و سیستم باس

حالا که فهمیدیم چطوری می‌تونیم با پوینتر آدرس یه متغیر رو بدست بیاریم و تغییرش بدیم، بیایم یه نگاه به ساختار حافظه و سیستم باس بندازیم.

### سیستم باس: راه رسیدن به دنیای پوینترها

فرض کنید داریم یه سیستم میکروکنترلی رو طراحی می‌کنیم و فقط یه خونه حافظه تک‌بایتی داریم. اگه سیستم ما 8 بیتی باشه، این یعنی CPU می‌تونه با یه کلاک، همه‌ی بیت‌های این بایت رو تغییر بده. پس طبیعی هست که 8 سیم جداگانه برای هر بیت داشته باشیم که همزمان امکان مقداردهی از سمت CPU رو داشته باشن.
به این سیم ها اصطلاحا میگیم Data Bus.

<div dir="ltr">

```text
        ----+-------+-------+-------+-------+------+-------+-------+------+-------> DataBus (8 wire)
        |       |       |       |       |       |       |       |
|   +-------+-------+-------+-------+-------+-------+-------+-------+     |
|   | Bit 0 | Bit 1 | Bit 2 | Bit 3 | Bit 4 | Bit 5 | Bit 6 | Bit 7 | B1  |
|   +-------+-------+-------+-------+-------+-------+-------+-------+     |
|_________________________________________________________________________|
```
</div>

### اضافه کردن Control Bus

حالا که می‌خوایم CPU بتونه مقادیری که روی حافظه نوشته رو بخونه یا بنویسه، به یه **Control Bus** نیاز داریم که CPU رو از قصد عملیات (خواندن یا نوشتن) مطلع کنه. Control Bus به ما اجازه می‌ده که مشخص کنیم چه زمانی باید حافظه داده رو بخونه یا بنویسه. علاوه بر این، پایه `EN` یا کلاک هم لازم داریم تا همگام‌سازی بین CPU و حافظه رو انجام بدیم.

<div dir="ltr">

```text
        ----+-------+-------+-------+-------+------+-------+-------+------+-------> DataBus (8 wire)
        |       |       |       |       |       |       |       |
|   +-------+-------+-------+-------+-------+-------+-------+-------+     |
|   | Bit 0 | Bit 1 | Bit 2 | Bit 3 | Bit 4 | Bit 5 | Bit 6 | Bit 7 | B1  |-------+-------+ ControlBus
|   +-------+-------+-------+-------+-------+-------+-------+-------+     |
|_________________________________________________________________________|
```
</div>

---

### Address Bus

حالا اگه بخوایم به بیش از یه خونه حافظه دسترسی داشته باشیم، مثلاً دو تا خونه حافظه تک‌بایتی داشته باشیم، نیاز به یه سیم دیگه به اسم **Address Bus** داریم تا مشخص کنیم که CPU به کدوم خونه حافظه باید اشاره کنه.

<div dir="ltr">

```text
        ----+-------+-------+-------+-------+------+-------+-------+------+-------> DataBus (8 wire)
        |       |       |       |       |       |       |       |
|   +-------+-------+-------+-------+-------+-------+-------+-------+       |
|   | Bit 0 | Bit 1 | Bit 2 | Bit 3 | Bit 4 | Bit 5 | Bit 6 | Bit 7 |   B1  |-------+-------+ ControlBus (2 wire)
|   +-------+-------+-------+-------+-------+-------+-------+-------+       |
|   +-------+-------+-------+-------+-------+-------+-------+-------+       |-------+-------+ AddressBus (log2(n) wire)
|   | Bit 0 | Bit 1 | Bit 2 | Bit 3 | Bit 4 | Bit 5 | Bit 6 | Bit 7 |   B2  |
|   +-------+-------+-------+-------+-------+-------+-------+-------+       |
|___________________________________________________________________________|
```
</div>

### سیستم‌های 32 بیتی

حالا اگه یه سیستم 32 بیتی داشته باشیم، این یعنی Address Bus سیستم ما می‌تونه تا 2^32 خونه حافظه رو آدرس‌دهی کنه. یعنی سیستم می‌تونه ۴ گیگابایت حافظه رو مدیریت کنه.

وقتی می‌گیم یه سیستم 32 بیتی هست، منظورمون اینه که Address Bus اون سیستم 32 بیتی هست و این یعنی می‌تونه ۴ گیگابایت حافظه رو آدرس‌دهی کنه.

---

### Address Bus: کنترلی روی همه‌چیز!

حالا بیاید به این سوال فکر کنیم: اگه ما بتونیم مقدار **Address Bus** رو به دلخواه تنظیم کنیم، چه اتفاقی میفته؟ یعنی اگه CPU بتونه به‌طور مستقیم مقدار Address Bus رو کنترل کنه و بگه "من به این خونه خاص از حافظه می‌خوام دسترسی پیدا کنم"، در این صورت CPU می‌تونه به هر نقطه‌ای از حافظه دسترسی داشته باشه، چه برای **خواندن** و چه برای **نوشتن**. این دسترسی مستقیم به تمام نقاط حافظه، به این معناست که CPU می‌تونه داده‌ها رو از هر جایی که بخواد برداره یا در هر کجای حافظه بنویسه.

برای اینکه بهتر متوجه بشید، فرض کنید سیستم ما می‌تونه با Address Bus به **۲^n** خونه حافظه دسترسی پیدا کنه. حالا هر خونه حافظه، یک آدرس منحصربه‌فرد داره. این آدرس می‌تونه به یکی از موارد زیر اشاره کنه:
- یک خونه حافظه در **RAM**.
- یک خونه حافظه در **Flash**.
- یک رجیستر سخت‌افزاری که به یک **پریفرال** (مثل تایمر، UART یا GPIO) وصل شده.

### دسترسی به تمام بلوک‌های حافظه

سیستم ما طوری طراحی شده که تمام انواع حافظه و رجیسترها به یک باس مشترک وصل می‌شن. این به این معنیه که از طریق **Address Bus** می‌تونیم به هر جایی از این حافظه‌ها دسترسی داشته باشیم. حالا شاید براتون سوال پیش بیاد که "کجاهای حافظه، RAM قرار داره؟ کجاهای حافظه Flash یا رجیسترهای پریفرال قرار دارن؟"

این دقیقاً همون جاییه که وقتی می‌گیم "هر جایی از حافظه" می‌تونیم بریم، منظورمون مشخص می‌شه:

1. **RAM**: وقتی CPU به یه محدوده خاص از حافظه اشاره می‌کنه، اون محدوده ممکنه به **RAM** سیستم مربوط باشه. این حافظه برای ذخیره‌سازی موقت داده‌ها استفاده می‌شه و محتوای اون موقع خاموش شدن سیستم از بین می‌ره.

2. **Flash**: محدوده دیگه‌ای از حافظه که CPU به اون دسترسی داره، مربوط به **Flash Memory** هست. این حافظه غیرموقت هست و معمولاً برای ذخیره‌سازی برنامه‌ها و داده‌هایی که بعد از خاموش شدن سیستم نیاز داریم، استفاده می‌شه.

3. **رجیسترهای پریفرال‌ها**: بعضی از محدوده‌های خاص در حافظه مربوط به **رجیسترهای سخت‌افزاری پریفرال‌ها** مثل تایمرها، UART یا GPIO هستن. این رجیسترها به CPU این امکان رو می‌دن که با پریفرال‌ها ارتباط برقرار کنه و اون‌ها رو کنترل کنه.

### یک مثال از دسترسی همه‌جانبه

فرض کنید CPU آدرسی رو در **Address Bus** قرار می‌ده. اون آدرس می‌تونه به یکی از موارد زیر اشاره کنه:
- اگر آدرس در محدوده RAM باشه، CPU به حافظه RAM دسترسی پیدا می‌کنه و داده رو از اونجا می‌خونه یا تغییر می‌ده.
- اگه آدرس به محدوده Flash مربوط باشه، CPU می‌تونه داده یا کدی که اونجا ذخیره شده رو بخونه.
- اگر آدرس به یکی از رجیسترهای پریفرال‌ها اشاره کنه، CPU می‌تونه اون پریفرال رو کنترل کنه. مثلاً می‌تونه به یه رجیستر تایمر دستور بده که شروع به شمارش کنه یا به رجیستر UART بگه داده‌ای رو از طریق سریال ارسال کنه.

### نتیجه‌گیری: کنترل Address Bus یعنی دسترسی به همه‌جا!

پس، کنترل مستقیم Address Bus به این معنیه که CPU می‌تونه به هر نقطه‌ای از حافظه (چه RAM، چه Flash و چه پریفرال‌ها) دسترسی داشته باشه. و این همون جاییه که مفهوم **پوینتر** وارد بازی می‌شه! وقتی ما آدرس یه متغیر یا رجیستر رو توی یه پوینتر ذخیره می‌کنیم، در واقع داریم آدرس اون خونه حافظه رو کنترل می‌کنیم. و با دسترسی به اون آدرس، می‌تونیم هر تغییری که بخوایم توی اون خونه حافظه ایجاد کنیم.

## آرایه‌ها

| شماره | سوال                                                                                                                                                                                                                                                                     | بارم‌بندی |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| 1     | تابعی بنویسید که محتویات یک آرایه از اعداد صحیح را چاپ کند                                                                                                                                                                                                               | 1        |
| 2     | تابعی بنویسید که یک آرایه از اعدا صحیح و یک عدد دریافت و تمام خانه های آن را برابر با آن مقدار قرار دهد.                                                                                                                                                                 | 1        |
| 3     | تابعی بنویسید محتویات یک آرایه از اعداد صحیح را در آرایه ی دیگر کپی نماید، سایز آرایه می تواند متغیر باشد                                                                                                                                                                | 2        |
| 4     | تابعی بنویسید که محتوای یک آرایه از اعداد صحیح را از هر مکان دلخواهی در آرایه یدوم کپی نماید                                                                                                                                                                             | 2        |
| 5     | تابعی بنویسید که محتویات یک آرایه را معکوس نماید                                                                                                                                                                                                                         | 2        |
| 6     | تابعی بنویسید که محتویات یک ارایه را به صورت معکوس درون آرایه دوم کپی نماید                                                                                                                                                                                              | 3        |
| 7     | تابعی بنویسید که یک عدد را درون یک آرایه پیدا نماید و آدرس مکان آن را بر گرداند                                                                                                                                                                                          | 2        |
| 8     | تابعی بنویسید که دو آرایه را با هم مقایسه نماید و حالات زیر را بر گرداند (مقایسه دو آرایه به صورت عضو به عضو صورت می گیرد)                                                                                                                                               | 3        |
| 9     | تابعی بنیوسید که یک پترن (دنباله ی اعداد) را درون یک آرایه ی بزرگتر پیدا نماید و مکان شروع آن را برگرداند                                                                                                                                                                | 4        |
| 10    | تابعی بنویسید که یک آیتم (عدد) را به درون یک آرایه اضافه نماید، تعاداد آیتم ها مغیر می باشد اما حداکثر تعداد آیتم درون آرایه ثابت و برابر با طول آرایه می باشد (مثال: آرایه ای با طول حداکثر 10 عضو و دارا بودن 5 عضو پس از افزودن یک عضو دارای 6 عضو معتبر می باشد)     | 3        |
| 11    | تابعی بنویسید که یک آیتم (عدد) را از یک آرایه حذف نماید                                                                                                                                                                                                                  | 3        |
| 13    | تابعی بنویسید که در یک آرایه ی مرتب شده با استفاده از الگوریتم باینری سرچ دنبال عددی بگردد و ایندکس مکان عدد را بر گرداند                                                                                                                                                | 4        |
| 14    | تابعی بنویسید که میانگین اعداد یک آرایه از اعداد صحیح را محاسبه کند.                                                                                                                                                                                                     | 2        |
| 15    | تابعی بنویسید که بزرگترین و کوچکترین عنصر یک آرایه را پیدا نماید.                                                                                                                                                                                                        | 3        |
| 16    | تابعی بنویسید که تکرارهای یک عنصر خاص در یک آرایه را محاسبه نماید.                                                                                                                                                                                                       | 3        |
| 17    | یک آرایه دو بعدی از اعداد صحیح ایجاد کنید و تابعی بنویسید که میانگین اعداد هر سطر را داخل یک ارایه دیگر ذخیره نماید.                                                                                                                                                     | 4        |
| 18    | تابعی بنویسید که یک ارایه از اعداد صحیح دریافت کرده و اعلام نماید این آرایه یک آرایه پالیندروم (تقارنی) است یا خیر.                                                                                                                                                      | 3        |
| 19    | تابعی بنویسید که یک ارایه دریافت نماید و اعداد را به ترتیب صعودی و نزولی مرتب نمیاد (صعودی یا نزولی بودن قابل انتخاب است)                                                                                                                                                | 4        |
| 20    | تابعی بنویسید که دو آرایه دریافت نماید و بزرگترین عنصر مشترک دو آرایه را باز گرداند.                                                                                                                                                                                     | 4        |
| 21    | تابعی بنویسید که یک ارایه از اعداد صحیح دریاقت نمیاد و عناصر تکراری را از آرایه حذف کند.                                                                                                                                                                                 | 3        |
| 22    | تابعی بنویسید که یک آرایه پویا (dynamic array) با ظرفیت اولیه n ایجاد کند. سپس توابعی برای اضافه کردن، حذف کردن و تغییر اندازه‌ی آرایه به شکلی پویا و بهینه بنویسید. باید بتوان از این آرایه به عنوان یک لیست استفاده کرد و عملیات اضافه و حذف به صورت دینامیک انجام شود. | 5        |
| 23    | تابعی بنویسید که یک آرایه و یک عدد دریافت کند و بزرگترین زیرآرایه‌ای که مجموع آن برابر با عدد داده شده باشد را پیدا کند و اندیس‌های شروع و پایان آن را برگرداند.                                                                                                           | 5        |



 
## رشته‌ها

| شماره | سوال                                                                                                                                                                                                                           | بارم‌بندی |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| 1     | تابعی بنویسید که یک رشته را درون رشته ای دیگر کپی نماید                                                                                                                                                                        | 1        |
| 2     | تابعی بنویسید که یک رشته را به انتهای رشته ای دیگر اضافه نماید                                                                                                                                                                 | 1        |
| 3     | تابعی بنویسید که دو رشته را مقایسه نماید و حالات زیر را برگرداند                                                                                                                                                               | 2        |
| 4     | تابعی بنویسد که یک حرف را درون رشته ای پیدا نماید و آدرس مکان آن را برگرداند                                                                                                                                                   | 2        |
| 5     | تابعی بنویسید که طول یک رشته را برگرداند                                                                                                                                                                                       | 1        |
| 6     | تابعی بنویسید که یک رشته را درون رشته ای بزرگتر پیدا و مکان شروع آن را برگرداند                                                                                                                                                | 3        |
| 7     | تابعی بنوییسد که دو رشته را با طول مشخص با هم مقایسه نماید                                                                                                                                                                     | 2        |
| 8     | تابعی بنویسد که یک رشته را درون یک متن (رشته ای بزرگتر) با یک رشته ای دیگر جایگزین نماید                                                                                                                                       | 4        |
| 9     | تابعی بنویسید که یک آرایه از رشته ها دریافت کند و آن را به دو صورت کوچک به بزرگ و بزرگ به کوچک مرتب نماید                                                                                                                      | 4        |
| 10    | تابعی بنویسید که یک رشته را درون آرایه ای از رشته ها پیدا نماید و ایندکس آن را بر گرداند                                                                                                                                       | 3        |
| 11    | تابعی بنویسید که یک رشته را درون آرایه از رشته های مرتب شده با استفاده از الگوریتم باینری سرچ پیدا نماید و ایندکس آن را برگرداند                                                                                               | 4        |
| 12    | تابعی بنویسد که آرایه از رشته ها را در یافت نموده و درون یک متن (رشته ای بزرگ) جستجو نماید و هرکدام از رشته ها که اول به آن بر خورد را مکان شروع آن درون رشته ی بزرگتر و ایندکس آن رشته را برگرداند                            | 4        |
| 13    | تابعی بنویسید که از الگوریتم Knuth-Morris-Pratt برای پیدا کردن تمام وقوعات یک رشته (Pattern) درون یک رشته بزرگتر (Text) استفاده کند و اندیس‌های شروع هر وقوع را برگرداند.                                                       | 5        |
| 14    | پیاده‌سازی رمزنگاری سزار (Caesar Cipher): تابعی بنویسید که یک رشته و یک عدد دریافت کند و آن رشته را با استفاده از رمزنگاری سزار رمزگذاری و رمزگشایی کند. این تابع باید قادر باشد هر دو عملیات رمزگذاری و رمزگشایی را انجام دهد. | 5        |
| 15    | پیاده‌سازی الگوریتم Longest Common Subsequence: تابعی بنویسید که دو رشته دریافت کند و طول بلندترین زیررشته مشترک (LCS) آن‌ها را پیدا کند. این الگوریتم باید بهینه و با استفاده از برنامه‌ریزی پویا پیاده‌سازی شود.                 | 5        |
| 16    | تابعی بنویسید که یک آرایه از رشته ها دریافت کند و آن را به دو صورت کوچک به بزرگ و بزرگ به کوچک مرتب نماید                                                                                                                      | 5        |


</div>

