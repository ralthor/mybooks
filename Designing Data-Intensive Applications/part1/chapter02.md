# 🚧فصل دوم. مدل‌های داده و زبان‌های پرس و جو

## مدل رابطه‌ای و مدل داکیومنت
‫در مورد SQL و NoSQL صحبت کرده. دلایل مطرح شدن NoSQL مشکلاتی که قرار بوده حل کنه:
- ‫نیاز برای مقیاس‌پذیری بیشتر از پایگاه‌های داده‌ای رابطه‌ای مثلا برای پایگاه‌های داده‌ی خیلی بزرگ یا write throughput خیلی بالا
- ترجیح استفاده از نرم‌افزارهای متن‌باز
- عملیات خاص پرس و جو که در مدل‌های رابطه‌ای به خوبی پشتیبانی نمی‌شوند
- ناامیدی از محدودیت‌های اسکیمای مدل رابطه‌ای و اشتیاق به داشتن مدل داده‌ای پویاتر و مبیّن‌تر

بیان این مساله که جدول‌های مدل رابطه‌ای برای پرکردن مدل شیئ‌گرا نیاز به عملیات اضافه دارند.
‫فریم‌ورک‌هایی مثل Hibernate و ActiveRecord برای پر کردن این فاصله وجود دارند اما باز هم قادر به مخفی‌سازی کامل نیستند.

در مورد رابطه‌ی یک به چند و این که چطور در پایگاه داده رابطه‌ای حل می‌کنیم:
- جدول مجزا و استفاده از کلید خارجی
- ‫برخی از ورژن‌های بعدی SQL از XML و JSON در یک فیلد پشتیبانی می‌کنند و امکان ایندکس‌گذاری و جستجو در آن‌ها را فراهم می‌کنند
- ‫گزینه‌ی دیگر ذخیره‌ی این اطلاعات در قالب JSON یا XML در یک فایل است که مشخصا امکانات جستجوی پایگاه داده را ندارد.

‫رابطه‌ی یک به چند عملا یک رابطه‌ی درختی است که ذخیره‌ی آن در JSON این نکته را عیان می‌کند

### روابط چند به یک و چند به چند
مزایای روابط یک به چند را ذکر کرده:
- یکنواختی
- ابهام‌زدایی
- بروزرسانی آسان
- پشتیبانی از لوکالیزیشن و ترجمه
- جستجوی بهتر

مزیت استفاده از آیدی این است که چون برای انسان معنی ندارد نیاز به تغییر پیدا نمی‌کند

 
## زبان‌های پرس و جو برای داده
‫راجع به زبان‌های پرس و جوی
declarative
و همچنین
map-reduce
صحبت کرده.

توضیح از جایی غیر از خود کتاب: 
Declarative programming is a paradigm describing WHAT the program does, without explicitly specifying its control flow. Imperative programming is a paradigm describing HOW the program should do something by explicitly specifying each instruction (or statement) step by step, which mutate the program's state. [More here](https://stackoverflow.com/questions/1784664/what-is-the-difference-between-declarative-and-imperative-paradigm-in-programmin)

### map-reduce
‫یک نمونه فراخوانی کدهای نوشته شده در پایتون در hadoop

```shell
hadoop jar /path/to/hadoop-streaming.jar \
  -input /path/to/input/data \
  -output /path/to/output/directory \
  -mapper /path/to/mapper.py \
  -reducer NONE
```


## مدل‌های داده‌ای گراف‌مانند

[back](README.md)