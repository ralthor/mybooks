# 🚧فصل چهارم. ‫کدگذاری و Evolution
راجع به
forward- and backward-compatibility
در داده‌ها صحبت کرده.

تغییر در برنامه منجر به تغییر در داده هم می‌شود.

کد جدید با داده‌ی قدیمی 
(backward-compatibility)
و کد قدیمی با داده‌های جدید
(forward-compatibility)
بتوانند کار کنند

حتی در برنامه‌های سمت سرور هم تغییرات مرحله‌ای اعمال می‌شوند که منجر به کار با داده‌های قدیمی‌تر یا جدیدتر می‌شود.

کلمه‌ی
Evolution
در این فصل به معنی تغییر ساختار داده در طول زمان به کار رفته.

> ‫**ℹ️** در این فصل چند فرمت ذخیره‌سازی ذکر کرده و برای هر کدام روش برخورد با موارد بالا، اسکیما و روش استفاده در انتقال اطلاعات را بررسی کرده.

## Formats for Encoding Data
دو نوع نمایش لازم است:
- در حافظه
- در فایل

به نوعی ترجمه بین دو حالت بالا نیاز است.
- coding & encoding

### Language-Specific Formats
روش‌های مختص زبان‌های برنامه‌نویسی مثل
pickle
در
Python،
یا
Serializable
در
Java
و
Marshal
در
Ruby.

مشکلات:
- در زبان‌های دیگر سخت خوانده می‌شوند
- مشکلات امنیتی به خاطر احتمال وجود کد اجرایی یا دسترسی به حافظه
- ورژنینگ
- کارایی

در کل ایده‌ی بدی است.

### JSON, XML and Binary Variants
فرمت‌های ذخیره‌سازی
JSON, XML و CSV
هم مشکلات خودشان را دارند:
- ابهام در ذخیره‌ی اعداد
- عدم پشتیبانی باینری - نیاز به راهکارهایی مثل
base64
هست که حجم داده‌ها را تا ۳۳ درصد بیشتر می‌کند
- پشتیبانی اسکیما اختیاری، قوی اما پیچیده برای یادگیری و پیاده‌سازی است
- CSV
اسکیما ندارد و فرمت سطر و ستون هم در آن اختیاری است و بسیاری از پارسرها هم قوانین
scaping
را به درستی برای آن پیاده نکرده‌اند.

با وجود این مشکلات به اندازه‌ی کافی خوب هستند که تا مدت زیادی قابل استفاده باقی بمانند.

فرمت‌های زیادی برای ذخیره‌ی باینری
XML و JSON
پیاده‌سازی شده‌اند.

- JSON: MessagePack, BSON, BJSON, UBJSON, BISON, Smile
- XML: WBXML, Fast Infoset

### Thrift and Protocol Buffers
هر دو فرمت‌های باینری ذخیره‌ی داده هستند و بنا بر اصل مشترکی بنا شده‌اند.

هر دو به فیلدها شماره می‌دهند که بعدا با تغییر نام آن‌ها را گم نکنند و همچنین برای ذخیره‌ی اعداد از یک سیستم کدینگ باینری استفاده می‌کنند که بتوانند اندازه‌های مختلف را در یک ساختار مشترک پشتیبانی کنند.

برای مثال در
Thrift
ست کردن بالاترین بیت یک بایت یعنی بایت بعدی نیز وجود دارد.

برای هر دو فرمت ابزارهای تولید خودکار کد وجود دارد که کدینگ و دیکدینگ را در زبان‌های برنامه‌نویسی مختلف ساده می‌کند.

تغییر اسکیما نیاز به تنظیم دستی شماره فیلدها دارد.

### Avro
راجع به پشتیبانی از اسکیماهای مختلف برای خواندن و نوشتن صحبت کرده و تغییر آن‌ها در
Evolution
و همچنین
backward- and forward-compatibility.

در
Avro
فیلدها شماره ندارند که پشتیبانی از
dynamically generated schema
را امکان‌پذیر می‌کند.


### The Merits of Schemas
پیاده‌سازی ساده‌ی سه فرمت اشاره شده در بالا باعث استفاده‌ی گسترده‌تر آن‌ها شده است و حتی آن‌ها را گزینه‌ی مناسب‌تری برای کاربردهای جدید ساخته است.

- کدینگ باینری بر پایه‌ی اسکیما می‌تواند از
JSON
فشرده‌تر باشد
- اسکیما به ما اطمینان می‌دهد که مستندات به‌روز هستند
- 
- نگهداری از دیتابیسی از اسکیماها
forward- and backward-compatibility
را مقدور می‌سازند
- در زبان‌های برنامه‌نویسیِ
statistically typed
توانایی تولید کد برای اسکیما سودمند است و توانایی تایپ‌چکنیگ در زمان کامپایل را فراهم می‌کند
- استفاده از
schema evolution
در کدینگ باینری همان انعطاف دیتابیس‌های بدون اسکیما یا
schema-on-read
برای
JSON
را فراهم می‌کند، اما علاوه بر آن گارانتی‌های بهتری را برای داده و ابزار کار با آن در اختیار قرار می‌دهد.

## Models of Dataflow
این مدل‌ها شامل پردازه‌هایی که داده‌ها را کد کرده و پردازه‌های (دیگری) که آن‌ها را دیکد کرده و می‌خوانند می‌شوند.

### Dataflow Through Databases
- The process of writing data to a database encodes it, and the process of reading it decodes it
- Backward compatibility is necessary and Forward compatibility is often required for databases as well, to handle the case where an older version updates a value written by a newer version
- Data outlives code, and rewriting data into a new schema can be expensive, so most databases avoid it if possible
- Schema evolution allows the database to appear as if it was encoded with a single schema, even though different versions of the schema may be used in storage
- Archival storage, such as taking snapshots of the database, allows for data to be encoded consistently using the latest schema, and formats like Avro and Parquet are a good fit for this purpose.

### Dataflow Through Services: Rest and RPC
- There are different ways of arranging communication over a network, with the most common being clients and servers.
- Servers expose an API over the network and clients connect to the servers to make requests to that API.
- The web works by clients (web browsers) making requests to web servers using standardized protocols and data formats (HTTP, URLs, SSL/TLS, HTML, etc.).
- Web browsers are not the only type of client, other examples include native apps and client-side JavaScript applications.
- A server can also act as a client to another service, this is known as a service-oriented architecture (SOA) or microservices architecture.
- Services are similar to databases but expose an application-specific API and provide a degree of encapsulation.
- A key design goal of a service-oriented/microservices architecture is to make the application easier to change and maintain by making services independently deployable and evolvable.




#### Web services
- Web services use HTTP as the underlying protocol for communication.
- Web services can be used in various contexts:
  - A client application (e.g. mobile app or web app) making requests to a service over HTTP via the internet.
  - One service making requests to another service within the same organization, often located within the same datacenter, as part of a service-oriented/microservices architecture.
  - One service making requests to a service owned by a different organization, usually via the internet.
- Two popular approaches to web services are REST and SOAP.
- REST is a design philosophy that emphasizes simple data formats, using URLs for identifying resources and using HTTP features for cache control, authentication, and content type negotiation.
- SOAP is an XML-based protocol for making network API requests, but it aims to be independent from HTTP and avoids using most HTTP features. It comes with a complex set of related standards (the web service framework, known as WS-*).
- The API of a SOAP web service is described using an XML-based language called the Web Services Description Language (WSDL).
- Users of SOAP rely heavily on tool support, code generation, and IDEs.
- SOAP has fallen out of favor in most smaller companies and interoperability between different vendors’ implementations often causes problems.
- RESTful APIs tend to have simpler approaches, typically involving less code generation and automated tooling. A definition format such as OpenAPI can be used to describe RESTful APIs and produce documentation.

#### The problems with remote procedure calls (RPCs)
- Remote procedure calls (RPCs) have been around since the 1970s
- The RPC model tries to make a request to a remote network service look the same as calling a function or method in your programming language, within the same process
- The approach is fundamentally flawed
- A local function call is predictable and either succeeds or fails, depending only on parameters that are under your control. A network request is unpredictable
- Network request may lost due to network problem, or the remote machine may be slow or unavailable
- A local function call either returns a result, or throws an exception, or never returns. A network request may return without a result, due to a timeout
- If you retry a failed network request, it could happen that the requests are actually getting through, and only the responses are getting lost
- Local function calls don’t have this problem
- Every time you call a local function, it normally takes about the same time to execute. A network request is much slower than a function call, and its latency is also wildly variable
- When you call a local function, you can efficiently pass it references (pointers) to objects in local memory. When you make a network request, all those parameters need to be encoded into a sequence of bytes that can be sent over the network
- The client and the service may be implemented in different programming languages, so the RPC framework must translate datatypes from one language into another
- All of these factors mean that there’s no point trying to make a remote service look too much like a local object in your programming language, because it’s a fundamentally different thing
- Part of the appeal of REST is that it doesn’t try to hide the fact that it’s a network protocol



#### Current directions for RPC
- RPC isn't going away, various frameworks have been built on top of different encodings
- New generation of RPC frameworks are more explicit about the fact that a remote request is different from a local function call
- Examples of frameworks: Thrift, Avro, gRPC, Finagle, Rest.li
- gRPC and Finagle/Rest.li use futures to encapsulate asynchronous actions that may fail
- gRPC supports streams, where a call consists of not just one request and one response, but a series of requests and responses over time
- Some frameworks also provide service discovery, allowing a client to find out at which IP address and port number it can find a particular service
- Custom RPC protocols with a binary encoding format can achieve better performance than something generic like JSON over REST
- RESTful API has other significant advantages, such as being good for experimentation and debugging, supported by all mainstream programming languages and platforms, and having a vast ecosystem of tools available
- REST seems to be the predominant style for public APIs, while RPC frameworks focus on requests between services owned by the same organization, typically within the same datacenter
- For evolvability, it's important that RPC clients and servers can be changed and deployed independently
- Backward and forward compatibility properties of an RPC scheme are inherited from whatever encoding it uses
- Thrift, gRPC, and Avro can be evolved according to the compatibility rules of the respective encoding format
- SOAP uses XML schemas, which can be evolved but with subtle pitfalls
- RESTful APIs most commonly use JSON for responses, and JSON or URI-encoded/form-encoded request parameters for requests
- Service compatibility is made harder by the fact that RPC is often used for communication across organizational boundaries, so the provider of a service often has no control over its clients
- No agreement on how API versioning should work for RESTful APIs, common approaches are to use a version number in the URL or in the HTTP Accept header

### Message-Passing Dataflow

[back](README.md)