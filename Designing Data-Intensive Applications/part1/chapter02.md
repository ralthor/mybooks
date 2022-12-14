# ✒️فصل دوم. مدل‌های داده و زبان‌های پرس و جو

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

از عبارت‌های 
map, collect
و عبارت‌های 
reduce, fold, inject
برای دو بخش اصلی عملیات استفاده می‌شود.

برای
map-reduce
از پس و جوهای
MongoDB
هم مثال زده و 
aggregate
را گفته.

## مدل‌های داده‌ای گراف‌مانند
اگر ارتباطات چند به چند بین داده‌ها بسیار زیاد باشد استفاده از مدل‌های گرافی توصیه می‌شود.
(برای ارتباطات خیلی کم بین داده‌ها مدل داکیومنت و برای ارتباطات یک به چند زیاد مدل رابطه‌ای مناسب هستند)

در یک گراف دو نوع
object
داریم:
- راس‌ها: نودها، موجودیت‌ها
- یال‌ها: ارتباطات یا کمان‌ها

مثال‌هایی از انواع داده با ارتباطات زیاد:
- گراف‌های اجتماعی (مثل فیسبوک)
- گراف وب
- شبکه‌های ریلی یا حمل و نقل (جاده‌ای)

راس‌ها و یال‌های گراف می‌توانند از انواع متفاوتی باشند.
مثلا فیسبوک راس‌هایی از افراد، مکان‌ها، کامنت‌ها، رویدادها و مانند آن دارد و یال‌هایی که می‌توانند دوستی بین افراد، نویسنده‌ی کامنت بودن و از این دست باشند.

### Property graphs
در این روش، هر یال و راس علاوه بر نگه‌داری مشخصه‌های لازم گراف مثل یال های مرتبط، راس‌های متصل، شناسه‌ی منحصر به فرد و از این دست،
یک لیست از 
property
هم دارد که به صورت
key-value pairs
نگه‌داری می‌شوند.

می‌توان یک
graph store
را به صورت دو جدول رابطه‌ای در نظر گرفت: راس‌ها و یال‌ها.

جنبه‌های مهم این مدل:
- هر راسی می‌تواند یالی مرتبط به هر راس دیگر داشته باشد. محدودیتی برای انواعی که می‌توانند به هم متصل شوند نداریم
- برای هر راس دلخواه می‌توان هر دو مجموعه‌ی یال‌های ورودی و خروجی را به سادگی یافت: می‌توان گراف را پیمایش کرد
- با استفاده از برچسب‌های مختلف برای روابط مختلف می‌توان انواع اطلاعات را در یک گراف ذخیره کرد

### Cypher query language
یک گراف و یک پرس و جو روی آن مثال زده.
در مثال، نوع راس درخواستی و نوع رابطه با راس دیگری مشخص شده:
اشخاصی که در مکان مشخصی به دنیا آمده «و» در مکان مشخصی زندگی می‌کنند.
زندگی کردن و به دنیا آمدن از انواع روابطی هستند که میان راس‌های اشخاص و مکان‌ها وجود دارند.

### Graph queries in SQL
با در نظر گرفتن امکان ذخیره‌ی گراف در دو جدول در مدل رابطه‌ای، پرس و جوهای «بازگشتی» برای جستجو در گراف مثال زده.

### Triple-stores and SPARQL
همان مدل
property graph
است اما با بیان دیگر و ابزارهای دیگر.

تمام اطلاعات به صورت سه‌تاییِ
(subject, predicate, object)
ذخیره می‌شوند. مثلا
(Jim, likes, bananas).

در این سه‌تایی،
object
و
subject
می‌توانند «مقدار» یا «یک راس دیگر» باشد.

از این مدل در وب معنایی
semantic web
استفاده می‌شود و معمولا فرمت
RDF
به کار گرفته می‌شود.

روی
RDF
زبان پرس و جوی 
SPARQL
استفاده می‌شود که به
Cypher
نزدیک است.

### Datalog
در مورد 
Datalog
هم صحبت می‌کند که قدیمی‌تر از
SPARQL و Cypher
است.

### نکات پایانی
پایگاه داده‌ی بدون اسکیما داریم، اما اپلیکیشن قاعدتا تصوراتی از ساختار داده دارد.
این اسکیما می‌تواند
explicit
(اجباری در زمان نوشتن) یا
implicit
(مفروض در زمان خواندن) باشد.

پایگاه‌های داده‌ای داریم که اینجا ذکر نشدند:
- GenBank: برای جستجو و یافتن دنباله‌های ژنی. در پایگاه‌های داده‌ی ذکر شده پشتیبانی نمی‌شود
- در فیزیک ذرات داده‌ها چنان زیادند که نیاز به راه‌حل‌های سفارشی برای کاهش هزینه‌ی سخت‌افزار هست. حجم در حد چند صد پتابایت!
- Full-text search - بعدا در بخش اندیس‌گذاری بیشتر به این موضوع خواهد پرداخت

[back](README.md)