<div dir="rtl" style="text-align: right">

# بانک سوال مصاحبهٔ معماری نرم‌افزار — امیررضا قادری

نوار: **Level 3 Senior Engineer** در Java Chapter (G1–G3)، مطابق `internal/Java-Chapter-Career-Path.xlsx`. سوال‌ها معماری‌سخت‌اند، نه آزمون Java / Spring / JVM / sidecar.

این سند **سوال مصاحبه** است، نه راهنمای مطالعه. داور مشخصه و trade-off می‌خواهد، نه نام ابزار. پیکربندی Kubernetes و trivia (SOLID، تعریف CAP به‌صورت حفظیات، custom starter) خارج از محدوده است.

**تفاوت نوار با Level 2:** Engineer باید timeout / retry / circuit breaker روی *downstream* و saga را به‌اسم بلد باشد. Senior باید *upstream* را طراحی کند (load shedding، rate limiting، bulkhead)، مالکیت دامنه را نگه دارد، حادثه را جلو ببرد، و هزینه / ظرفیت / build-vs-buy را در مقیاس سازمان ببیند.

**شرکت فرضی مشترک:** پلتفرم اعتبار (credit) — از **allocation** تا جریان **قسط (installment)**، با وابستگی به scoring، هستهٔ بانکی، بازپرداخت، و کانال‌های محصول. امروز حدود ۳۰ مهندس روی دامنه، هدف چند تیم هم‌جوار در دو سال. یک monolith ده‌ساله. پیک campaign حدود ۲۰×. صورتحساب زیرساخت در حال رشد. PCI روی مسیر پول.

**نوار قبولی Level 3:** تصمیم را با نیرو / مشخصه / پیامد می‌گوید؛ قربانی آگاهانه نام می‌برد؛ failure mode را قبل از happy path می‌نویسد؛ یکپارچگی دامنهٔ پلتفرم را مالک است نه فقط سرویس خودش. فهرست تکنولوژی = رد نرم. پاسخ Level 2 (Feign + CB + «eventual») سقف نیست — کف است.

---

## جریان مصاحبهٔ زنده (۴۵–۶۰ دقیقه)

چهار سوال الزامی؛ پنجمی فقط اگر هنوز عمق نمانده و وقت هست. بقیهٔ بانک برای round دوم، take-home، یا فشار روی یک محور ضعیف است.

| دقیقه | سوال زنده | نوع | چرا همین |
| ---: | --- | --- | --- |
| ۰–۵ | کالیبراسیون: یک سیستم واقعی که ساخته؛ یک تصمیم معماری که *عمداً* بد بوده و چرا | گرم‌کردن | جدا کردن داستان‌گو از کسی که trade-off دیده |
| ۵–۲۲ | **۱. مسیر اعتبار: از allocation تا قسط** | طراحی سیستم | ردیف Domain Knowledge شیت: دید سرتاسری allocation → installment |
| ۲۲–۳۴ | **۲. وابستگی بالادستی می‌ترکد؛ پلتفرم اعتبار overload می‌شود** | تشخیص شکست | Level 3 = upstream resiliency، نه فقط circuit breaker روی Feign |
| ۳۴–۴۴ | **۳. تخصیص، پرداخت تسهیلات، قسط: saga یا مالکیت غلط؟** | داده / trade-off | consistency/availability + event-driven با جدول شکست |
| ۴۴–۵۵ | **۴. یکپارچگی دامنهٔ پلتفرم اعتبار میان هشت تیم** | سازمان / معماری | G3: integrity of a platform domain؛ G2: roadmap و نفوذ بین‌تیمی |
| ۵۵–۶۰ | **۵. مهاجرت موتور اقساط: strangler، dual-write، build vs buy** *(اگر وقت ماند)* | تکامل / هزینه | data migration، ظرفیت، governance، vendor |

**قانون داور در جلسه:** اگر کاندیدا Kafka / Redis / «microservices» / Resilience4j را قبل از مشخصه و مرز شکست آورد، قطع کن: «ابزار را بگذار کنار؛ چه چیزی را بهینه می‌کنی و چه چیزی را می‌شکنی؟» Load shedding و invalidation را خودت لو نده.

**جمع‌بندی نمره (ذهنی، آخر جلسه):** Hire Level 3 اگر در حداقل سه سوال الزامی به نوار «قوی» نزدیک شد و دامنهٔ اعتبار را end-to-end دید. G3 اگر در سازمان/هزینه/build-vs-buy عمق آورد. Lean-hire اگر طراحی درست بود ولی upstream یا سازمان را در حد Level 2 گفت. No-hire اگر ابزار ردیف کرد یا «best practice» را به‌جای نیرو گذاشت.

### نگاشت سوال‌های زنده به ردیف‌های Level 3

منبع: شیت `Level 3 (Senior Engineer)`، ستون‌های G1–G3. JVM profiling، custom starter، و معماری Kubernetes امتیاز مصاحبهٔ معماری نیستند.

| سوال زنده | ردیف‌های شیت که باید دیده شوند |
| --- | --- |
| **۱. مسیر اعتبار: از allocation تا قسط** | **Domain Knowledge (G1):** holistic view از allocation تا installment و impact روی دامنه‌های مجاور. **Distributed Systems (G1):** تصمیم consistency/availability، scalability fundamentals، distributed caching و *cache invalidation*. **Collaboration (G1):** architectural trade-offs؛ پیشبرد design meeting. **Architecture (G3):** هم‌راستایی استراتژی مهندسی با هدف کسب‌وکار (چه مشخصه‌ای قربانی می‌شود). **Database (G1/G2):** caching؛ اگر به شارد/مسیر خواندن–نوشتن رسید، scaling. |
| **۲. وابستگی بالادستی می‌ترکد** | **Distributed Systems (G1):** upstream resiliency — load shedding، rate limiting، bulkhead. **Distributed Systems (G2/G3):** scale-out بی‌حالت، استراتژی resiliency/scalability مستند در مقیاس سازمان — نه «پاد بیشتر». **Debugging (G1–G3):** ارتباط در حادثه، رهبری تحقیق، postmortem و پیشگیری، کاهش MTTR. **Observability (G3):** چه سیگنالی باید *از قبل* می‌بود (symptom نه CPU). |
| **۳. تخصیص / تسهیلات / قسط** | **Distributed Systems (G1):** event-driven architecture؛ طراحی با تصمیم consistency/availability. **Collaboration (G1):** architectural trade-offs. **Domain (G1):** مرز allocation در برابر disbursement در برابر installment. **Database (G1):** اگر dual-write یا مهاجرت وضعیت را پیش کشید. الگوی saga به‌اسم = Level 2؛ جدول شکست و مالکیت داده = Level 3. |
| **۴. یکپارچگی دامنهٔ پلتفرم اعتبار** | **Collaboration (G2/G3):** رهبری پروژهٔ چندتیمی، حل تعارض فنی، نفوذ و متقاعدسازی، *maintains the integrity of the architecture for a platform domain*، تعریف استاندارد. **Architecture (G2/G3):** governance (کاتالوگ API، سیاست deprecation)، multi-year technical roadmap، align با کسب‌وکار. **Chapter (G2):** تغییر way of working نه فقط یک سرویس. |
| **۵. مهاجرت موتور اقساط** | **Database (G1/G2):** data migration؛ database scaling. **Architecture (G2/G3):** deprecation policy، roadmap چندساله، *build vs buy / vendor selection*، cost & capacity planning در مقیاس سازمان. **CI/CD (G1):** zero-downtime به‌عنوان استراتژی مهاجرت (rolling/blue-green/canary) — نه YAML. **Collaboration (G3):** یکپارچگی معماری دامنه حین دو سیستم زنده. |

**اگر وقت round دوم ماند (پوشش ردیف باقی‌مانده):** سوال بانک ۲ (event-driven خلاف مد)، ۵ (شارد + invalidation)، ۱۴ (هزینه/ظرفیت سازمانی)، ۱۶ (کاتالوگ API و deprecation). Stress-testing را داخل سوال ۱ یا ۲ بپرس، نه به‌عنوان ابزار.

---

## گروه ۱ — مشخصه، تصمیم، trade-off

داور این گروه: معماری انتخاب تکنولوژی نیست. «بهترین عمل» وجود ندارد.

### ۱. مسیر اعتبار: از allocation تا قسط

**نوع:** طراحی سیستم *(سوال زنده)*

**سوال.** مسیر اعتبار را طراحی کن: از **تخصیص سقف (allocation)** تا **جدول اقساط و بازپرداخت**. محدودیت‌ها همزمان‌اند: پیک campaign حدود ۲۰×، درستی مانده و قسط، PCI روی مسیر پول، وابستگی به scoring و هستهٔ بانکی، تیم ۳۰ نفره *امروز*. ۱۲ دقیقه برای برش اول. Kubernetes و نام برند cloud امتیاز ندارد.

**پاسخ ضعیف.** فهرست stack (Kafka، Redis، چند microservice، Gateway). Happy path. همه چیز strongly consistent و 99.99. امتیازدهی، اعلان، گزارش ریسک و disbursement همه روی یک مسیر sync با retry نامحدود. Cache سقف اعتبار بدون حرف از invalidation.

**پاسخ قوی باید پوشش دهد:**

- حداکثر ۳ مشخصهٔ غیرقابل‌مذاکره (مثلاً درستی مانده/قسط، idempotency تخصیص، زمان پاسخ مسیر درخواست اعتبار) و حداقل ۲ قربانی آگاهانه (تازگی امتیاز بازاریابی، fan-out ضدتقلب غیرحیاتی، تنوع stack).
- مرز دامنه: allocation، disbursement، installment — چه چیزی *باید* هم‌مالک بماند؛ impact روی scoring / هستهٔ بانکی / حساب مشتری.
- وابستگی hard در برابر soft؛ برای هر soft، degrade مشخص.
- مسیر خواندن در برابر نوشتن: سقف قابل‌نمایش را می‌شود cache کرد؛ تخصیص را نه بدون invalidation. رویداد بازپرداخت / تسویه / معوق چه چیزی را باطل می‌کند و با چه تأخیر قابل‌مشاهده‌ای.
- اوج campaign: throttle محصول یا جدا کردن hot path، نه «scale-out تا بی‌نهایت».
- PCI به‌عنوان محدودیت ساختاری (چه داده‌ای وارد quantum اعتبار نمی‌شود).

**پیگیری.**

1. p99 سرویس scoring از ۸۰ms به ۲s می‌رسد. کدام قابلیت *عمداً* زنده می‌ماند؟
2. Vendor خارجی برای موتور اقساط پیشنهاد شده (build vs buy). سه نیرو که تصمیم را عوض می‌کنند — نه فهرست ویژگی فروشنده.
3. کاربر قسط را پرداخته؛ اپ هنوز سقف را قفل نشان می‌دهد. این cache است، replication lag، یا مالکیت غلط رویداد؟

**نمره.** Level 3 یعنی مسیر allocation→installment را یک سیستم می‌بیند و invalidation را نام می‌برد. Wish-list مشخصه یا Redis بدون سیاست باطل‌سازی = زیر نوار.

---

### ۲. تخصیص اعتبار نباید تماماً event-driven شود — دفاع کن

**نوع:** trade-off

**سوال.** یک تیم ارشد می‌گوید کل جریان allocation تا قسط باید روی broker برود «برای decoupling و مقیاس». تو مخالفی. نیروهای مسئله را MECE بچین، گزینه‌ها را بگذار (sync در فرآیند تخصیص، async برای side-effect مثل اعلان/ایندکس، outbox، و «همین‌طور که هست»)، و *least-worst* را برای تیم ۳۰ نفره امروز دفاع کن — نه برای Netflix.

**پاسخ ضعیف.** «Kafka از بهترین عمل‌هاست» یا برعکس «event-driven همیشه بد است». هیچ نیروی قابل‌اندازه‌گیری (coupling زمانی، failure mode، پیچیدگی ذهنی، exactly-once دروغین). کپی بلاگ شرکت بزرگ بدون زمینه.

**پاسخ قوی باید پوشش دهد:**

- نیروها: درستی پرداخت در برابر decoupling زمانی؛ latency کاربر در برابر جداسازی شکست؛ هزینهٔ عملیاتی و cognitive load تیم.
- گزینه‌ها واقعاً جایگزین‌اند، نه اسلاید الگو.
- پیامد تصمیم: چه inconsistencyای برای کاربر دیده می‌شود؛ چه چیزی را بعداً سخت می‌کند.
- تلهٔ خارج از زمینه: آنچه برای شرکت با هزار مهندس درست است، اینجا least-worst نیست.

**پیگیری.**

1. یک نقطه که *باید* async باشد (مثلاً اعلان سررسید/ایندکس) نام ببر و بگو چرا همان استدلال برای تخصیص سقف غلط است.
2. شش ماه بعد تیم ۳ برابر شده. کدام بخش این ADR را باز می‌کنی — کل تصمیم را نه؟

**نمره.** امتیاز با دفاع تصمیم *خلاف مد* است. شعار مخالف مد هم به اندازهٔ شعار موافق مد صفر است.

---

### ۳. «ما modular هستیم» — دروغ را در CI بگیر

**نوع:** trade-off / حاکمیت معماری

**سوال.** VP مهندسی می‌گوید سیستم modular است. هیچ آستانه‌ای نیست. سه fitness function پیشنهاد بده که *نقض مشخصه* را نشان دهد: یکی ساختاری، یکی عملیاتی، یکی فرآیندی. برای هر کدام: متریک، آستانه، واکنش. یکی هم نام ببر که *نباید* fitness function شود.

**پاسخ ضعیف.** «کد را مرور می‌کنیم»، پوشش تست، تعداد microservice، CPU. متریک بدون آستانه. واکنش = «آلارم می‌دهیم».

**پاسخ قوی باید پوشش دهد:**

- ساختاری: مثلاً وابستگی چرخه‌ای بین ماژول‌های مالک دامنه / نقض مرز pack-like — و این‌که *شکست CI* مجاز است.
- عملیاتی: بودجهٔ latency یا error budget مسیر allocation، نه میانگین CPU.
- فرآیندی: change fail rate یا lead time برای یک bounded context — اتصال به این‌که معماری loosely-coupled باید تحویل را تند کند، نه کند.
- مورد غیرمجاز: چیز کیفی، زودگذر، یا proxy غلط (تعداد سرویس ≠ modularity).
- چه چیزی فقط دیده شود و چه چیزی deploy را متوقف کند.

**پیگیری.**

1. تیم محصول می‌گوید آستانه p99 سخت است و رلیز را می‌خواباند. error budget را چطور به این fitness وصل می‌کنی؟
2. یک fitness که بعد از شش ماه *باید حذف شود* — چرا زنده نگه‌داشتنش معماری را دروغ می‌کند؟

**نمره.** مشخصه بدون عدد، مشخصه نیست. عدد بدون واکنش، تزئین است. Level 3 واکنش سازمانی می‌گوید نه فقط نمودار.

**منابع داور این گروه:** Richards & Ford، *Fundamentals of Software Architecture* (2nd ed.)، فصول مشخصه‌ها و سنجش؛ Ford et al.، *Software Architecture: The Hard Parts*، فصل ۱ و ۱۵ (نبود best practice، تحلیل trade-off). Fowler، [Architecture Decision Record](https://www.martinfowler.com/bliki/ArchitectureDecisionRecord.html).

---

## گروه ۲ — داده در مقیاس

داور این گروه: «eventual» شعار نیست. کلید پارتیشن محدودیت محصول است. تراکنش توزیع‌شده آخرین انتخاب است.

### ۴. تخصیص بعد از تأیید غیب می‌شود

**نوع:** تشخیص شکست / داده

**سوال.** هفتهٔ پیش replica خواندن به سرویس سقف اعتبار اضافه شد. کاربران می‌گویند: تخصیص موفق، سقف یک لحظه به‌روز است، refresh نیست. دو روز بعد failover رهبر؛ تعدادی disbursement بانکی بدون ردیف allocation. تیم می‌گوید «consistency eventual است». تشخیص بده: چند failure mode است، کدام برای اعتبار قابل‌قبول نیست، و replication اصلاً *برای چه* اضافه شده بود.

**پاسخ ضعیف.** «replica را sync کنیم» یا «از Mongo/Cassandra استفاده کنیم». یک علت برای دو پدیدهٔ متفاوت. Durability و read-your-writes را یکی می‌گیرد.

**پاسخ قوی باید پوشش دهد:**

- پدیدهٔ ۱: replication lag و شکستن read-your-writes (خواندن از follower بعد از write روی leader).
- پدیدهٔ ۲: async replication + failover = از دست رفتن commitهایی که کلاینت موفق دیده؛ این inconsistency نیست، **از دست رفتن دوام** است — در اعتبار یعنی پول رفته / سقف دروغ.
- replication برای چیست: مقیاس خواندن، latency جغرافیایی، یا availability — برای مسیر تخصیص معمولاً این سه یکی نیستند.
- تضمین‌هایی که باید بمانند (read-your-writes بعد از allocation، یا «تسهیلات رفته ⇒ ردیف اعتبار یا جبران قطعی») و تضمین‌هایی که می‌شود رها کرد (تازگی لیست اقساط دیگران).
- اصلاح معماری: sticky به leader برای جلسهٔ تخصیص، یا quorum، یا اعلام محصول — نه «روشن کردن eventual».

**پیگیری.**

1. اگر محصول multi-region فعال بخواهد، کدام مدل (single-leader / multi-leader / leaderless) *کمتر بد* است و چه anomalyای را به کاربر نشان می‌دهی؟
2. کاربر دو تب باز دارد؛ یکی سقف را می‌بیند یکی نه. این همان باگ است یا جلسه/routing؟

**نمره.** اگر «eventual consistency» را بدون پنجره و ناسازگاری قابل‌مشاهده گفت، همان‌جا این سوال را ببند — زیر نوار داده. Level 3 باید دوام را از تازگی جدا کند.

---

### ۵. کلید پارتیشن Allocation / Ledger / Installment و کمپین Viral

**نوع:** طراحی داده / مقیاس‌دهی DB

**سوال.** برای Allocation، دفتر مانده (ledger)، و Installment کلید پارتیشن بده. یک query که با این کلیدها *دیگر نباید وجود داشته باشد* نام ببر و جایگزین معماری‌اش را بگو. بعد: یک کمپین اعتبار یک `product_id` را داغ می‌کند و یک پارتیشن می‌سوزد. سه پاسخ معماری، با هزینه. سیاست cache سقف اعتبار و *باطل‌سازی* بعد از قسط را هم بگو.

**پاسخ ضعیف.** «روی user_id شارد می‌کنیم، horizontal scale». Join بین‌پارتیشنی را «با یک سرویس aggregation» پنهان می‌کند. Hot key را با «Redis» تمام می‌کند بدون invalidation.

**پاسخ قوی باید پوشش دهد:**

- کلید از الگوی دسترسی: allocation/ledger معمولاً حول حساب اعتبار یا کاربر؛ installment حول همان حساب *و* سررسید — تنش این دو را نام ببرد.
- Query ممنوع: مثلاً «همهٔ اقساط معوق همهٔ محصولات با join پرداخت» — جایگزین: read model، API composition، یا محدودیت محصول.
- Hot key کمپین: salt/spread، جدا کردن hot path، یا throttle تخصیص؛ trade-off هر کدام.
- Read path در برابر write path: replica/read model برای لیست اقساط؛ نوشتن تخصیص روی رهبر/شارد مالک.
- Cache: TTL دروغ است اگر رویداد بازپرداخت باید سقف را فوری باز کند؛ invalidation با کلید حساب، نه flush سراسری.

**پیگیری.**

1. گزارش ریسک روزانه به کلید تو نیاز دارد. این را در مسیر آنلاین حل می‌کنی یا عمداً به مسیر تحلیلی می‌فرستی؟
2. یک partner بزرگ = ۳۰٪ تخصیص‌ها. این hot key است یا isolation چندمستأجره غلط؟
3. اگر invalidation گم شود، کاربر دوباره از سقف خالی خرید اقساطی می‌کند. جبران معماری چیست — نه «fix باگ»؟

**نمره.** مقیاس = محدودیت مدل داده + سیاست باطل‌سازی. «چند replica + Redis» بدون skew و invalidation = Level 2.

---

### ۶. تخصیص، پرداخت تسهیلات، قسط: saga یا مالکیت غلط؟

**نوع:** داده / trade-off *(سوال زنده)*

**سوال.** سه تیم سه سرویس ساخته‌اند: Allocation، Disbursement، Installment. پیشنهاد A: 2PC. پیشنهاد B: event و «امید». جریان «قفل سقف + واریز تسهیلات + صدور جدول اقساط» را دو بار طراحی کن: (الف) یک واحد مالکیت و تراکنش محلی؛ (ب) توزیع اجباری با saga. برای (ب) جدول شکست هر مرحله و compensating action. ADR یک جمله‌ای: این workflow را saga *نمی‌کنیم* چون… یا می‌کنیم چون…

**پاسخ ضعیف.** 2PC پیش‌فرض. یا choreography شاد بدون جبران. سقف کم می‌شود، واریز fail، قسط یتیم؛ «پشتیبانی دستی» جزء معماری می‌شود. دلیل توزیع: «microservices». نام بردن saga بدون مالک داده = Level 2.

**پاسخ قوی باید پوشش دهد:**

- اول بپرسد: آیا این سه *باید* سرویس جدا باشند؟ کدام داده باید هم‌مالک بماند (مثلاً سقف + جدول قسط) تا از توزیع فرار کنیم.
- 2PC: availability و coupling عملیاتی؛ lock و failure mode هماهنگ‌کننده — برای مسیر پول معمولاً least-worst نیست.
- Saga: orchestration در برابر choreography؛ حداقل یک مرحله که جبران ندارد یا خطرناک است (واریز به حساب برگشت‌پذیر نیست مثل اعلان؛ صدور قسط بعد از واریز جبرانش چیست).
- Idempotency تخصیص، exactly-once دروغین، و visible state برای پشتیبانی و حسابرسی.
- «Saga پاداش اجبار توزیع است، نه سبک ارتباط.» Event-driven بودن با «پیام می‌فرستیم» یکی نیست.

**پیگیری.**

1. پول واریز شده و صدور اقساط جبران شکست خورده. کاربر چه می‌بیند؟ منبع حقیقت مانده کدام است؟
2. اگر جدول اقساط را با سقف در یک quantum نگه داری و فقط کانال واریز را جدا کنی، کدام failure mode می‌میرد؟

**نمره.** توزیع تراکنش آخرین انتخاب است. اگر نتوانست موردی بگوید که saga *نمی‌کند*، الگو حفظ کرده — زیر نوار Level 3.

**منابع داور این گروه:** Kleppmann، *Designing Data-Intensive Applications*، فصول Replication / Partitioning / Transactions. Ford et al.، *The Hard Parts*، مالکیت داده و sagas. Meta، [TAO](https://engineering.fb.com/2013/06/25/core-infra/tao-the-power-of-the-graph/) — availability آگاهانه در برابر strong consistency.

---

## گروه ۳ — شکست و قابلیت اطمینان

داور این گروه: «همه چیز up» امید است. قابلیت اطمینان بودجهٔ ریسک است. میانگین دروغ می‌گوید.

### ۷. وابستگی بالادستی می‌ترکد؛ پلتفرم اعتبار overload می‌شود

**نوع:** تشخیص شکست *(سوال زنده)*

**سوال.** از ۱۴:۰۰ سرویس scoring (downstream) p99 از ۹۰ms به ۲s می‌رسد. تا ۱۴:۰۸ نرخ خطای **API تخصیص اعتبار** ۴۰٪ است. همزمان سه کانال بالادستی — اپ، BNPL در checkout، و کمپین شریک — هر کدام ۳ retry بدون jitter می‌زنند. Pool نخ پر است، instanceها restart می‌شوند، health check timeout می‌شود و LB آن‌ها را از مدار خارج می‌کند. لیست اقساط و پرداخت قسط هم که به scoring وصل نیستند ۵۰۲ می‌دهند. نمودارها «CPU عادی» است؛ هیچ SLO/trace طلایی برای *ورودی* دامنه نیست. تشخیص زنجیره. ترک *بالادستی* از کجا باید طراحی می‌شد؟ بعد از حادثه چه postmortem و چه تغییر مشاهده‌پذیری اجباری است؟

**پاسخ ضعیف.** «پاد اضافه کنیم»، «autoscaling»، «circuit breaker روی Feign» بدون عدد. فقط downstream را می‌بیند. یک مقصر (scoring) بدون amplification از callerها. Postmortem = «timeout نداشتیم».

**پاسخ قوی باید پوشش دهد:**

- زنجیرهٔ downstream: integration point کند → blocked threads → کاهش ظرفیت مؤثر → health check دروغین.
- زنجیرهٔ **upstream** (نوار Level 3): retry storm از چند caller → dogpile → saturate کردن کل دامنه؛ بدون rate limit / load shedding / bulkhead بین کانال‌ها، قسط‌پرداخت هم می‌میرد.
- Bulkhead: استخر جدا برای allocation در برابر repayment؛ سهمیه per-caller / per-partner.
- Load shedding و fail fast روی لبهٔ دامنهٔ اعتبار؛ retry فقط downward با jitter و بودجه؛ idempotency کلید تخصیص.
- بعد از بهبود scoring: thundering herd بازیابی.
- حادثه: چه را باید از روی symptom صفحه می‌کردی (نرخ رد تخصیص، عمق صف، saturation) نه CPU؛ رهبری تحقیق؛ postmortem با اقدام پیشگیرانه و مالک MTTR — نه فهرست «درس‌ها».

**پیگیری.**

1. کدام وابستگی‌ها softاند (امتیاز بازاریابی، اعلان سررسید) و degrade دقیق چیست؟ پرداخت قسط نباید به scoring وابسته باشد — اگر هست، چه غلط مرزی‌ای رخ داده؟
2. یک آزمایش آشوب با شعاع کوچک: steady state نرخ تخصیص موفق، فرضیه، معیار رد؛ چرا این هفته در production کامل نه.
3. چه استاندارد سازمانی (نه ابزار یک تیم) جلوی تکرار این الگو را در دامنه‌های مجاور می‌گیرد؟

**نمره.** Level 2 = timeout و CB روی scoring. Level 3 = shed load / rate limit / bulkhead روی ورودی، جدا ماندن جریان قسط، و postmortem با تغییر سیستم نه فرد. افزودن ظرفیت به‌تنهایی = تغذیهٔ آتش.

---

### ۸. محصول ۹۹.۹۹ می‌خواهد و هفته‌ای ۲۰ رلیز

**نوع:** reliability / trade-off سازمانی

**سوال.** مدیر محصول برای API تخصیص اعتبار SLO سالانه 99.99 می‌خواهد و همزمان cadence بیست رلیز در هفته. SLO/SLI/error budget بنویس از نگاه *کاربر*، نه CPU. وقتی بودجه تمام شد معماری و فرآیند چه می‌کنند. عمداً SLO را ضعیف‌تر از رؤیای محصول دفاع کن.

**پاسخ ضعیف.** 99.99 را می‌پذیرد. SLI = uptime سرور. Error budget را نمی‌تواند به دقیقه/درخواست تبدیل کند. «پس رلیز را متوقف کنیم» بدون سیاست، یا «SRE مسئول uptime است».

**پاسخ قوی باید پوشش دهد:**

- SLI کاربرمحور: مثلاً نسبت تخصیص موفق در زمان توافق‌شده، یا پرداخت قسط موفق — نه «فرآیند زنده است».
- 99.99 یعنی حدود ۵۲ دقیقه در سال — با ۲۰ رلیز/هفته تقریباً هیچ فضای تغییر نمی‌ماند مگر change fail خیلی پایین باشد.
- Error budget به‌عنوان سیاست: تمام شد → انجماد ویژگی، تمرکز روی قابلیت اطمینان، یا کاهش SLO با امضای محصول.
- Four golden signals برای صفحه: symptom نه cause.
- ارتباط با متریک تحویل (lead time، change fail rate): پایداری و سرعت علیه هم نیستند اگر معماری loosely-coupled باشد؛ 99.99 روی مرز نادرست آن‌ها را دشمن می‌کند.

**پیگیری.**

1. کدام خطاها در SLI *نباید* شمرده شوند (کلاینت بد، کلید نامعتبر) و چرا تیم‌ها این را بازی می‌کنند؟
2. اگر scoring یا هستهٔ بانکی SLO ضعیف‌تر از تو داشته باشد، سقف واقعی تو چیست؟

**نمره.** قابلیت اطمینان را بودجهٔ ریسک ببیند. پذیرفتن 99.99 بدون حساب error budget = پاسخ سیاسی، نه معماری.

---

### ۹. میانگین عالی است؛ کاربر می‌گوید کند است

**نوع:** طراحی / تشخیص latency

**سوال.** صفحهٔ «وضعیت اعتبار من» به ۱۲ backend همزمان fan-out می‌کند (سقف، اقساط، امتیاز، کمپین، ضدتقلب، …). میانگین هر backend عالی است؛ p99 هر کدام حدود ۵۰ms. کاربر p99 کل را فاجعه می‌داند. ظرفیت اضافه کرده‌اند، صورتحساب بالا رفته، دم نیامده. توضیح بده چرا، و معماری مسیر interactive را عوض کن — hedging را فقط اگر خودت رسیدی.

**پاسخ ضعیف.** «cache»، CDN برای HTML، «شبکه مشکل دارد». میانگین را دفاع می‌کند. Fan-out را مقدس می‌داند.

**پاسخ قوی باید پوشش دهد:**

- دمِ تأخیر: p99 سیستم به سمت بدترین وابستگی می‌رود؛ با استقلال نسبی، احتمال این‌که *حداقل یکی* کند باشد با N بالا می‌رود.
- جدا کردن مسیر بحرانی از تزئینی؛ budget برای fan-out همزمان؛ early termination برای widget غیرحیاتی.
- Hedged request / hedged با هزینه و amplification؛ کی حرام است (نوشتن، پرداخت).
- افزودن ظرفیت دمِ همبستگی (GC، دیسک، lock، همسایهٔ noisy) را درمان نمی‌کند.

**پیگیری.**

1. یک ADR یک‌خطی: «fan-out همزمان این مسیر از N بیشتر ممنوع مگر با بودجهٔ p99 مشخص.»
2. اگر همبستگی بین backendها بالا باشد (یک datastore مشترک)، hedging چه ضرری می‌زند؟

**نمره.** Latency را توزیع ببیند. اگر فقط cache گفت بدون حرف از fan-out و دم، زیر نوار Level 3 است.

**منابع داور این گروه:** Nygard، *Release It!* (antipatternهای پایداری و الگوها). Google SRE Book، [Embracing Risk](https://sre.google/sre-book/embracing-risk/)، [Service Level Objectives](https://sre.google/sre-book/service-level-objectives/)، [Addressing Cascading Failures](https://sre.google/sre-book/addressing-cascading-failures/). Dean & Barroso، [The Tail at Scale](https://cacm.acm.org/research/the-tail-at-scale/). Amazon Builders’ Library، [Timeouts, retries and backoff with jitter](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/).

---

## گروه ۴ — سازمان شکل سیستم است

داور این گروه: نمودار بدون نقشهٔ تیم ناقص است. Microservice پاداش استقلال است، نه پیش‌فرض. Distributed monolith را باید بو کشید.

### ۱۰. یکپارچگی دامنهٔ پلتفرم اعتبار میان هشت تیم

**نوع:** سازمان / معماری *(سوال زنده)*

**سوال.** تیم‌ها امروز: Frontend، Backend، DBA، QA، «پلتفرم» با صف تیکت، Scoring، Installment، Mobile. تو Senior دامنهٔ اعتبار هستی: *یکپارچگی معماری این platform domain* با توست. اگر هیچ‌کس دخالت نکند، معماری شش ماه بعد چه شکلی است؟ Reverse Conway بده تا جریان allocation→قسط بدون هشت round-trip ممکن شود. Team API یک تیم stream-aligned. یک برش از **roadmap دوساله** (نه فهرست اپیک این اسپرینت): چه چیزی امسال استخراج نمی‌شود. یک تعارض فنی بین‌تیمی را چگونه حل و *متقاعد* می‌کنی — نه از روی hierarchy.

**پاسخ ضعیف.** «Inverse Conway؛ هر موجودیت یک سرویس.» Cognitive load را نمی‌بیند. پلتفرم همان ticket queue. همه Collaboration. Roadmap = «microservices کامل». هیچ حرفی از deprecation یا کاتالوگ API.

**پاسخ قوی باید پوشش دهد:**

- پیش‌بینی Conway: لایه‌ای کند؛ DBA گلوگاه schema دفتر اعتبار؛ QA انتهای صف؛ Frontend منتظر Backend؛ Installment تنها برش نسبتاً هم‌تراز.
- جریان ارزش: درخواست اعتبار، واریز، پرداخت قسط — نه لایه‌های تکنولوژی.
- یکپارچگی دامنه: چه قراردادهایی *نباید* شکسته شوند (مدل مانده، شناسهٔ حساب اعتبار، سیاست idempotency)؛ governance = کاتالوگ API + سیاست deprecation، نه اسلاید استاندارد.
- Roadmap چندساله: امسال مرز ماژول و مشاهده‌پذیری؛ سال بعد استخراج انتخابی جایی که cognitive load یا مقیاس جدا می‌شود — نه ۲۰ سرویس این فصل.
- Team API؛ پلتفرم X-as-a-Service؛ قطع ارتباط همه-با-همه.
- نفوذ: تعارض «scoring باید در تراکنش تخصیص باشد» را با نیرو حل می‌کند، نه با rank.

**پیگیری.**

1. Scoring باید enabling باشد، platform، یا stream-aligned با X-as-a-Service؟ انتخاب غلط چه معماری‌ای می‌سازد؟
2. افزودن «قسط در میان‌دوره» امروز چند round-trip تیمی می‌خورد؛ بعد از maneuver باید چند تا باشد؟
3. یک تیم محصول API داخلی تخصیص را بدون نسخه در اپ شریک شکسته. پاسخ حاکمیتی تو چیست؟

**نمره.** Level 3 G3 یعنی مالک یکپارچگی دامنه است نه مالک یک microservice. C4 بدون نقشهٔ تیم و بدون roadmap = ناتمام.

---

### ۱۱. سی مهندس در برابر سیصد: modular monolith یا سرویس؟

**نوع:** trade-off

**سوال.** جدول تصمیم برای محصول ۳۰ نفره و محصول ۳۰۰ نفره: modular monolith، service-based درشت، microservice. هر سلول: چه مشخصه‌ای برنده می‌شود و چه هزینه‌ای می‌پردازی. پیش‌نیازهایی که اگر نباشند microservice تبدیل به distributed monolith می‌شود نام ببر. یک جمله: Shopify چرا مرز داخل monolith را سخت کرد؛ Uber با هزاران سرویس چه دردی را با domain-oriented بودن هدف گرفت. نتیجه باید از *درد شما* بیاید نه از کپی.

**پاسخ ضعیف.** «همیشه monolith first» یا «همیشه microservice برای مقیاس». تعداد مهندس را با تعداد سرویس یکی می‌گیرد. Distributed monolith را نمی‌تواند تعریف کند.

**پاسخ قوی باید پوشش دهد:**

- درد *الان*: modularity، مقیاس مستقل، استقلال تیم، تنوع تکنولوژی — معمولاً یکی است نه هر چهار.
- پیش‌نیاز: provision سریع، مشاهده‌پذیری، deploy مستقل، مالکیت داده؛ بدون این‌ها شبکه می‌آوری و استقلال نه.
- ۳۰ نفر: هزینهٔ هماهنگی توزیع معمولاً از سود استقلال بیشتر است؛ مرز ماژول + fitness ساختاری.
- ۳۰۰ نفر: استخراج *انتخابی* جایی که cognitive load یا مقیاس یا امنیت جدا می‌شود — نه entity-per-service.
- Distributed monolith: شبکه‌دار، بدون استقلال deploy، با تراکنش توزیع‌شدهٔ پنهان.

**پیگیری.**

1. یک سرویس موجود را که نباید استخراج می‌شد نام ببر (فرضی، از مسیر اعتبار). کدام نیروی *integrator* را نادیده گرفتند؟
2. Thoughtworks هرگز microservices را Adopt نکرد. این را چطور برای VP ترجمه می‌کنی بدون ضدیت مذهبی؟

**نمره.** Microservice را پاداش بداند. اگر نتوانست distributed monolith را در یک دیاگرام کلامی تشخیص دهد، زیر نوار Level 3 برای این سطح سازمان است.

---

### ۱۲. پلتفرم «به اندازه» یا امپراتوری تیکت؟

**نوع:** سازمان / معماری

**سوال.** بیست تیم محصول، بیست راه برای هویت، لاگ، مهاجرت schema، و صف. cognitive load منفجر است. فهرست ۱۰ قابلیت مشترک. سه تا که باید self-service پلتفرم شود، سه تا که *نباید* (هنوز). یک interaction mode برای رابطهٔ تیم محصول و پلتفرم؛ یک رفتار ممنوع. پلتفرم خودش چگونه monolith سازمانی نمی‌شود؟

**پاسخ ضعیف.** «یک platform team همه‌چیز را استاندارد می‌کند.» صف تیکت برای هر namespace. استاندارد به‌معنی مجوز مرکزی برای هر کتابخانه. هیچ UX برای پلتفرم به‌عنوان محصول.

**پاسخ قوی باید پوشش دهد:**

- هدف پلتفرم کاهش بار ذهنی است نه یکدست‌سازی ایدئولوژیک.
- Just big enough: هویت، مشاهده‌پذیری طلایی، مسیر deploy، راز — بله؛ چارچوب دامنه، ORM اجباری، «یک database برای همه» — معمولاً نه.
- X-as-a-Service با رابط روشن؛ Collaboration فقط برای ورود اولیه؛ Facilitating موقت.
- رفتار ممنوع: تیکت برای کار تکراری؛ دور زدن پلتفرم با cloud حساب سایه؛ پلتفرم به‌عنوان approver هر رلیز.
- پلتفرم هم fitness و نسخه می‌خواهد؛ در غیر این صورت خودش گلوگاه Conway می‌شود.

**پیگیری.**

1. تیم اعتبار می‌گوید به خاطر PCI نمی‌تواند پلتفرم مشترک لاگ را استفاده کند. استثنا است یا بوی مرز اعتماد غلط؟
2. اگر پلتفرم SLO نداشته باشد، تیم محصول حق دارد مسیر سایه بسازد؟

**نمره.** پلتفرم محصول داخلی است. صف تیکت با نام platform = شکست معماری سازمانی.

**منابع داور این گروه:** Skelton & Pais، *Team Topologies* (Conway، cognitive load، چهار توپولوژی، interaction modes). Fowler، [Monolith First](https://martinfowler.com/bliki/MonolithFirst.html) و [Microservice Prerequisites](https://martinfowler.com/bliki/MicroservicePrerequisites.html). Shopify Engineering، [Deconstructing the Monolith](https://shopify.engineering/deconstructing-monolith-designing-software-maximizes-developer-productivity)؛ Uber، [Domain-Oriented Microservice Architecture](https://www.uber.com/us/en/blog/microservice-architecture/).

---

## گروه ۵ — سیستم زنده: تکامل، هزینه، امنیت

داور این گروه: بازنویسی بزرگ شکست است. هزینه مشخصه است. امنیت مرز اعتماد است نه فایروال. قرارداد باید پیر شود بدون شکستن مشتری.

### ۱۳. مهاجرت موتور اقساط: strangler، dual-write، build vs buy

**نوع:** تکامل / طراحی مهاجرت *(سوال زندهٔ اختیاری)*

**سوال.** موتور اقساط داخل monolith ده‌ساله است؛ مانده، جریمهٔ دیرکرد، و گزارش ریسک به آن گره‌اند. همزمان یک vendor مجموعه (collections) پیشنهاد buy می‌دهد. برنامهٔ strangler در چهار فاز: transform / coexist / eliminate. برای هر فاز معیار خروج. façade، anti-corruption layer، آنچه *عمداً* در monolith می‌ماند. شرط خاموش کردن کهنه. ریسک data-first در برابر code-first. **Build vs buy** را با نیرو ببند — نه با دموی فروشنده. یک عدد capacity/cost که اگر رد شد مهاجرت را متوقف می‌کند.

**پاسخ ضعیف.** Rewrite از صفر. یا «vendor را می‌گیریم چون time-to-market». ۱۸ ماه dual-run بدون معیار. دو writer. «اول همهٔ داده را migrate می‌کنیم.» Zero-downtime = «rolling در Kubernetes».

**پاسخ قوی باید پوشش دهد:**

- قابلیت باریک اول (مثلاً اقساط یک محصول اعتبار) نه «کل ledger».
- Façade واحد؛ coexist با parallel run روی درصد مشخص؛ ACL تا مدل کهنه به API نو نشت نکند.
- معیار خروج: اختلاف صفر در مانده/قسط، SLO مهاجرت جدا، درصد ترافیک، *یک* writer.
- Data-first خطر قفل روی مدل کهنه است؛ code-first بدون مالکیت داده = dual-write ابدی — برای دفتر اعتبار غیرقابل‌قبول.
- Build vs buy: کنترل مدل مانده، قفل vendor، هزینهٔ خروج، انطباق PCI، تناسب با roadmap دوساله؛ buy فقط گوشهٔ غیراصلی (مثلاً reminder) مجاز است نه منبع حقیقت قسط.
- Capacity/cost در مقیاس سازمان: هزینه به ازای ۱۰۰۰ قسط یا GB دفتر گرم؛ over-shard و dual-write ابدی قفل هزینه‌اند.
- Zero-downtime به‌عنوان استراتژی cutover (canary روی یک محصول اعتبار) نه ابزار استقرار.

**پیگیری.**

1. در coexist اختلاف ۱ ریال در ۱۰۰۰ قسط. rollback، flag، یا freeze نوشتار؟
2. چه نشانه‌ای می‌گوید *هنوز* موتور اقساط را جدا نکن (premature decomposition)؟
3. اگر buy را رد کردی، چگونه به کسب‌وکار دفاع می‌کنی که تاریخ عرضه را جابه‌جا کنند؟

**نمره.** مهاجرت با SLO و یک writer. Buy بدون هزینهٔ خروج = رد G3. دو منبع حقیقت مانده = رد.

---

### ۱۴. صورتحساب cloud از محصول جلو زده

**نوع:** هزینه به‌عنوان مشخصه

**سوال.** واحد هزینه و ظرفیت را برای دامنهٔ اعتبار در *مقیاس سازمان* تعریف کن (نه یک سرویس). سه تصمیم معماری که هزینه را *قفل* می‌کند نام ببر (نه instance گران). یک cost/capacity fitness function: اگر رد شد چه بررسی معماری اجباری است. یک تصمیم را با ADR عوض کن.

**پاسخ ضعیف.** Reserved instance، «مهندسی را بگویید خاموش کنند». هزینه را بعد از alarm مالی می‌بیند. Fan-out و dual-write و retention ابدی event را نمی‌بیند.

**پاسخ قوی باید پوشش دهد:**

- واحد: هزینه به ازای ۱۰۰۰ allocation موفق / به ازای حساب فعال / GB-ماه دفتر گرم — یکی را انتخاب کند و بگوید چه چیزی را پنهان می‌کند.
- قفل‌ها: chatty sync (N+1 تخصیص→سقف→قسط)، over-sharding، dual-write ابدی مهاجرت، fan-out صفحهٔ وضعیت، دادهٔ مرده در hot store بدون TTL، multi-AZ برای سرویسی بدون SLO.
- Fitness: آستانه + مالک دامنه + واکنش (بررسی معماری اجباری، نه ticket فین‌آپس). Capacity planning: پیک campaign را با throttle محصول وصل کند نه provision ابدی ۲۰×.
- Cost کنار latency و availability؛ گاهی SLO پایین‌تر ارزان‌تر و درست‌تر است — هم‌راستایی با کسب‌وکار.

**پیگیری.**

1. Hedged request p99 را نجات داده و صورتحساب را ۲.۴× کرده. کدام مشخصه برنده است و که تصمیم می‌گیرد؟
2. یک read model که هر تیم برای احتیاط کپی می‌کند. این هزینهٔ داده است یا هزینهٔ Conway؟

**نمره.** Level 3 G3 قفل معماری و ظرفیت سازمانی را نام می‌برد نه SKU ابر. هزینه «بعداً» = مشخصه‌های متقاطع را نفهمیده.

---

### ۱۵. سرویس کمپین به دفتر اقساط وصل است — چون «داخلی است»

**نوع:** امنیت / مرز اعتماد

**سوال.** Threat model سبک برای دامنهٔ اعتبار: بازیگران، trust boundary، حساس‌ترین داده (مانده، هویت، دستور واریز). یک مسیر exploit *منطقی* (نه ابزار نفوذ): سرویس کمپین/توصیه از شبکهٔ داخلی به store دفتر اقساط می‌رسد. برای هر مرز یک کنترل. ADR: سرویس X حق store اعتبار ندارد. پیامد عملیاتی را بگو.

**پاسخ ضعیف.** «TLS و فایروال و WAF». Perimeter trust. Secret در env بیست سرویس. Compliance بعد از طراحی. فهرست OWASP بدون مالکیت داده.

**پاسخ قوی باید پوشش دهد:**

- مرز اعتماد با quantum معماری یکی است: مانده و دستور واریز فقط در واحد اعتبار؛ least privilege روی هویت سرویس نه روی VPC.
- سطح حمله با هر سرویس جدید بزرگ می‌شود؛ defense in depth یعنی *کدام لایه چه کنترل*: هویت، authorization در API، جداسازی store، audit، نه «همه جا mTLS» به‌تنهایی.
- Confused deputy / over-broad internal ACL؛ کمپین حداکثر به شناسهٔ حساب و سیگنال غیرحساس.
- پیامد ADR: اعتبار SLO و on-call جدا؛ schema دفتر از سرویس کمپین جدا؛ مهاجرت و backup جدا؛ هزینهٔ DX برای تیم کمپین.
- انسان از داده دور: دیباگ production با ماسک، نه dump جدول.

**پیگیری.**

1. یک batch job ریسک شبانه به همان store نیاز دارد. آن را داخل quantum اعتبار می‌آوری یا یک export کنترل‌شده با TTL؟
2. اگر mTLS سراسری باشد ولی همه یک role داشته باشند، چه چیزی را دروغ گفته‌ای؟

**نمره.** امنیت مشخصهٔ ساختاری است (مالکیت، سطح حمله، identity). چک‌لیست انتهای پروژه = Junior با اسلاید Security.

---

### ۱۶. قرارداد عمومی پیر می‌شود؛ lake مرکزی دامنه را گروگان گرفته

**نوع:** تکامل داده / قرارداد

**سوال.** دو بحران همزمان: (۱) breaking change در API عمومی تخصیص اعتبار؛ شرکای BNPL می‌شکنند؛ تیم می‌گوید versioning گران است. (۲) افزودن فیلد «وضعیت معوق» به قسط سه ماه معطل تیم data lake است. سیاست نسخه برای API داخلی در برابر خارجی، در قالب **کاتالوگ API و سیاست deprecation** دامنه. یک data product برای دامنهٔ اعتبار: مالک، SLA داده، مصرف‌کننده، و آنچه lake *دیگر* مالک آن نیست. چه کسی هزینهٔ تکامل را می‌پردازد — تو یا مشتری؟

**پاسخ ضعیف.** «Semver و همه باید آپگرید کنند.» یک نسخهٔ ابدی. Lake به‌عنوان منبع حقیقت عملیاتی. هیچ کاتالوگ و تاریخ deprecation.

**پاسخ قوی باید پوشش دهد:**

- حاکمیت: کاتالوگ API دامنهٔ اعتبار، نسخه، تاریخ مرگ، مالک — نه wiki پراکنده.
- خارجی: نسخهٔ تاریخ‌دار / pin شریک / encapsulation هزینهٔ نسخهٔ کهنه روی دوش ارائه‌دهنده (الگوی Stripe).
- داخلی: قرارداد سست‌تر اگر هماهنگ شوند؛ stamp coupling را نام ببرد.
- Data product: دامنهٔ اعتبار مالک SLA تازگی و معنای مانده/قسط است؛ lake مصرف‌کننده است نه صاحب تعریف Installment.
- دو عمر سیستم: عملیاتی در برابر تحلیلی؛ یکی کردن مدل هر دو را ضعیف می‌کند.

**پیگیری.**

1. مصرف‌کنندهٔ داخلی در همان شرکت API خارجی را parse کرده. سیاست تو چیست — تنبیه، adapter، یا پذیرش به‌عنوان مشتری؟
2. اگر data product اعتبار p99 تازگی ۱۵ دقیقه دارد و ضدتقلب به ۳ ثانیه نیاز دارد، محصول داده غلط است یا مصرف‌کننده باید مسیر عملیاتی جدا بگیرد؟

**نمره.** تکامل و deprecation را طراحی کند. شکستن شریک، یا lake به‌عنوان architect دامنه، هر دو زیر نوار Level 3 است.

**منابع داور این گروه:** Fowler، [Strangler Fig Application](https://www.martinfowler.com/bliki/StranglerFigApplication.html). Newman، *Building Microservices* (2nd ed.)، مهاجرت تدریجی و امنیت. Stripe، [Online migrations at scale](https://stripe.com/blog/online-migrations) و [APIs as infrastructure](https://stripe.com/blog/api-versioning). AWS Well-Architected، [Cost Optimization](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html) و [Security foundations](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/security.html). Dehghani، [How to Move Beyond a Monolithic Data Lake to a Distributed Data Mesh](https://www.martinfowler.com/articles/data-monolith-to-mesh.html).

---

## نحوهٔ استفادهٔ بقیهٔ بانک

- Round دوم (داده / مقیاس / cache): ۴ → ۵ → ۱۶.
- اگر در سوال ۱ ابزار ردیف کرد: ۲ و ۳ را جایگزین طراحی طولانی نکن؛ همان‌جا بکش پایین.
- اگر شکست را در حد Level 2 (CB روی Feign) گفت: همان سوال ۲ زنده را فشار بده تا shedding و bulkhead؛ به Kubernetes نرو.
- اگر سازمان را شعار گفت: وقت را به ۱۰ و ۱۲ بده؛ ۱۳ را برای G3 (buy/ظرفیت) نگه دار.
- take-home اختیاری: یک ADR دو صفحه‌ای برای سوال ۶ یا ۱۳؛ جلسهٔ بعدی فقط پیامد و آنچه غلط از آب درآمد.

*۱۶ سوال. چهار الزامی زنده، یکی اختیاری. نوار: Level 3 Senior (G1–G3) Java Chapter — معماری، نه Spring.*

</div>
