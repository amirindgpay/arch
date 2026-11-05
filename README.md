<div dir="rtl">

# مسیر یادگیری معماری نرم‌افزار — امیررضا قادری

مسیر **چالش‌محور** است، نه فهرست موضوع. هر مرحله یک مسئله‌ای است که شرکت بزرگ واقعاً با آن روبه‌رو می‌شود. فرض: کدنویسی بلدی؛ معماری به‌عنوان discipline تازه است.

**مدت:** حدود ۱۶–۲۰ هفته، پنج مرحلهٔ پشت‌سرهم. هر مرحله ۲–۴ هفته. اگر تمرینی را نتوانستی دفاع کنی، به مرحلهٔ بعد نرو.

**قانون مسیر**

- معماری انتخاب تکنولوژی نیست؛ انتخاب **trade-off** است.
- «بهترین عمل» وجود ندارد. سؤال درست این است: *برای این مشخصه‌ها، در این سازمان، با این هزینه، کدام تصمیم کمتر بد است؟*
- هر تصمیم مهم را در یک **ADR** بنویس: زمینه، گزینه‌ها، تصمیم، پیامد.
- پیکربندی Kubernetes و جزئیات ابزار خارج از محدوده است. اصل و مبادله را یاد بگیر.

---

## مرحله ۱ — وقتی «بهترین عمل» وجود ندارد

**هفته ۱–۳**

**چالش شرکت بزرگ:** محصول کار می‌کند، ولی هیچ‌کس نمی‌تواند بگوید سیستم *برای چه مشخصه‌هایی* طراحی شده. تصمیم‌ها شفاهی‌اند، هر تیم stack خودش را می‌آورد، و شش ماه بعد کسی نمی‌داند چرا Kafka وسط checkout نشسته است.

### ۱.۱ معماری به‌جای لیست تکنولوژی

**حالت شکست:** سند معماری می‌شود «ما از React + Nest + Kafka + K8s استفاده می‌کنیم». وقتی ترافیک ۱۰× شود یا تیم دو برابر شود، هیچ‌کس نمی‌داند کدام مشخصه قربانی شده.

**سؤال معماری:** مشخصه‌های معماری (*architectural characteristics*) این سیستم کدام‌اند، کدام ۳ تا غیرقابل‌مذاکره‌اند، و کدام‌ها با هم در تضادند؟

**تمرین**

1. یک محصول واقعی که می‌شناسی (مثلاً فروشگاه یا پرداخت) را بردار. حداکثر **۷ مشخصه** بنویس (عملیاتی مثل availability/elasticity، ساختاری مثل modularity، و متقاطع مثل security/cost). بعد به ۳ تا کاهش بده. اگر نتوانستی حذف کنی، هنوز architect نیستی — هنوز wish-list می‌نویسی.
2. یک ADR یک‌صفحه‌ای: «چرا این سیستم *الان* modular monolith است نه microservices.» گزینه‌های ردشده را بنویس.

**خوب یعنی:** می‌توانی به یک VP مهندسی بگویی «این سیستم برای X و Y بهینه است و Z را آگاهانه ضعیف کرده‌ایم» — بدون نام بردن از ابزار.

**منبع**

- Mark Richards & Neal Ford، *Fundamentals of Software Architecture*, 2nd ed. (۲۰۲۵): فصل ۴ *Architectural Characteristics Defined*، فصل ۵ *Identifying Architectural Characteristics* (کاتای Silicon Sandwiches).
- همان کتاب، فصل ۶ *Measuring and Governing Architecture Characteristics* — fitness function یعنی سنجش عینی مشخصه، نه شعار.
- Neal Ford, Mark Richards, Pramod Sadalage, Zhamak Dehghani، *Software Architecture: The Hard Parts* (۲۰۲۱): فصل ۱ *What Happens When There Are No “Best Practices”?* — ADR و fitness function به‌عنوان ابزار تصمیم، نه تزئین.
- Martin Fowler، [Architecture Decision Record](https://www.martinfowler.com/bliki/ArchitectureDecisionRecord.html)
- InfoQ، [Has Your Architectural Decision Record Lost Its Purpose?](https://www.infoq.com/articles/architectural-decision-record-purpose/) — ADR را با هر تصمیم غیرمعماری شلوغ نکن.

### ۱.۲ سنجش معماری؛ وگرنه فراموش می‌شود

**حالت شکست:** «ما modular هستیم» تا روزی که یک تغییر کوچک سه تیم را قفل می‌کند. هیچ آستانه‌ای تعریف نشده؛ پس هیچ‌کس نقض را نمی‌بیند.

**سؤال معماری:** کدام fitness function باید *شکست بسازد* (مثلاً در CI)، و کدام فقط باید دیده شود؟

**تمرین**

1. برای همان محصول، سه fitness function بنویس: یکی ساختاری (مثلاً وابستگی چرخه‌ای بین ماژول‌ها)، یکی عملیاتی (بودجهٔ latency p99)، یکی فرآیندی (change fail rate). برای هر کدام: متریک، آستانه، واکنش وقتی رد شد.
2. یک مورد که *نباید* fitness function شود پیدا کن (چیز کیفی یا زودگذر). دلیل بیاور.

**خوب یعنی:** مشخصه بدون عدد، مشخصه نیست. عدد بدون آستانه، تزئین است.

**منبع**

- FSA 2nd ed.، فصل ۶؛ فصل ۷ *The Scope of Architectural Characteristics* (quantum و granularity).
- Thoughtworks Technology Radar، [Architectural fitness function](https://www.thoughtworks.com/en-us/radar/techniques/architectural-fitness-function)
- Neal Ford / Building Evolutionary Architectures — تعریف: *«An architectural fitness function provides an objective integrity assessment of some architectural characteristic(s).»*

### ۱.۳ تحلیل trade-off، نه جمع‌آوری رأی

**حالت شکست:** جلسهٔ معماری تبدیل می‌شود به evangelism («همه باید event-driven شوند»). تصمیم از روی مد روز گرفته می‌شود، نه از روی نیروهای مسئله.

**سؤال معماری:** نیروهای این تصمیم کدام‌اند، گزینه‌ها MECE هستند یا نه، و کدام گزینه *least-worst* است؟

**تمرین**

1. مسئله: «آیا notification باید sync باشد یا async؟» جدول trade-off بساز: coupling، latency، failure mode، هزینهٔ عملیاتی، پیچیدگی ذهنی تیم. یک تصمیم بگیر و پیامد را بنویس.
2. تلهٔ out-of-context را عمداً پیدا کن: یک blog پست شرکت بزرگ که *برای آن‌ها* درست است و *برای یک تیم ۲۰ نفره* غلط.

**خوب یعنی:** می‌توانی تصمیم مخالف مد را با دلیل دفاع کنی.

**منبع**

- *The Hard Parts*، فصل ۱۵ *Build Your Own Trade-off Analysis* (MECE، تلهٔ خارج از زمینه، پرهیز از snake oil).
- FSA 2nd ed.، فصل ۴ — بخش *Trade-Offs and Least Worst Architecture*.

**خروجی مرحله ۱:** ۳ ADR + فهرست مشخصه‌های اولویت‌بندی‌شده + ۳ fitness function. اگر این‌ها را نداری، مرحله ۲ را شروع نکن.

---

## مرحله ۲ — داده در مقیاس؛ جایی که معماری واقعاً سخت می‌شود

**هفته ۴–۷**

**چالش شرکت بزرگ:** ترافیک و حجم داده چند برابر می‌شود. یک دیتابیس، یک مدل، و «تراکنش همه‌جا» دیگر کار نمی‌کند. تیم‌ها داده را کپی می‌کنند، lag را «باگ» می‌نامند، و hot key یک shard را می‌سوزاند.

### ۲.۱ تکرار (replication) ارزان نیست

**حالت شکست:** replica برای خواندن اضافه می‌شود. کاربر سفارش را می‌بیند، بعد نمی‌بیند (*read-your-writes* می‌شکند). failover رهبر، آخرین commit را می‌بلعد. تیم می‌گوید «consistency eventual است» بدون اینکه بداند *کدام* inconsistency برای کسب‌وکار قابل قبول است.

**سؤال معماری:** replication برای چیست — availability، latency جغرافیایی، یا مقیاس خواندن؟ و وقتی leader می‌میرد، کدام داده اجازه دارد گم شود؟

**تمرین**

1. برای checkout یک فروشگاه: سه تضمین بنویس که *باید* برقرار بماند (مثلاً read-your-writes بعد از پرداخت) و دو تضمینی که می‌توانی رها کنی. به مدل single-leader / multi-leader / leaderless وصل کن.
2. سناریوی failover را روی کاغذ بکش: write روی leader، replication async، leader می‌میرد. کاربر چه می‌بیند؟ محصول چه باید بگوید؟

**خوب یعنی:** «eventual consistency» را به‌عنوان نام یک *پنجرهٔ مشخص* و *ناسازگاری قابل‌مشاهده* توضیح می‌دهی، نه به‌عنوان شعار.

**منبع**

- Martin Kleppmann، *Designing Data-Intensive Applications*: فصل ۱ *Reliable, Scalable, and Maintainable Applications*؛ فصل ۵ *Replication* (single/multi/leaderless، sync vs async، replication lag).
- Meta Engineering، [TAO: The power of the graph](https://engineering.fb.com/2013/06/25/core-infra/tao-the-power-of-the-graph/) و مقالهٔ USENIX ATC ’۱۳ *TAO: Facebook’s Distributed Data Store for the Social Graph* — انتخاب آگاهانهٔ availability بر strong consistency برای social graph، با override برای موارد خاص.

### ۲.۲ پارتیشن، hot key، و دروغ مقیاس افقی

**حالت شکست:** sharding روی `user_id` عالی به نظر می‌رسد تا یک سلبریتی یا یک tenant بزرگ یک پارتیشن را منفجر کند. یا join بین‌پارتیشنی برمی‌گردد و سیستم دوباره شبیه یک دیتابیس می‌شود، فقط با شبکه در وسط.

**سؤال معماری:** کلید پارتیشن چیست، skew را چگونه می‌بینی، و کدام query دیگر *نباید* وجود داشته باشد؟

**تمرین**

1. برای marketplace: کلید پارتیشن پیشنهاد بده برای Order، Inventory، و Payment. یک query که با این کلیدها *غیرممکن* می‌شود نام ببر و بگو جایگزین معماری‌اش چیست (API composition، read model، یا پذیرش محدودیت محصول).
2. یک hot-key واقعی طراحی کن (مثلاً محصول Viral در Black Friday) و سه پاسخ معماری بنویس: salt کردن کلید، جدا کردن hot path، یا محدود کردن محصول — با trade-off.

**خوب یعنی:** مقیاس را به‌صورت *محدودیت مدل داده* می‌فهمی، نه به‌صورت «چند replica بیشتر».

**منبع**

- DDIA، فصل ۶ *Partitioning*.
- DDIA، فصل ۳ *Storage and Retrieval* — وقتی LSM vs B-tree و secondary index، پارتیشن را گران می‌کند.
- Stripe، [Online migrations at scale](https://stripe.com/blog/online-migrations) — dual-write، cutover خواندن، سپس نوشتن؛ مهاجرت داده در production بدون downtime.

### ۲.۳ تراکنش توزیع‌شده؛ یا saga، یا دروغ

**حالت شکست:** سفارش، موجودی، پرداخت در سه سرویس‌اند و تیم 2PC می‌خواهد. یا «event می‌فرستیم و امیدواریم». موجودی کم می‌شود، پرداخت fail می‌شود، سفارش یتیم می‌ماند. پشتیبانی دستی، معماری می‌شود.

**سؤال معماری:** مرز تراکنش کجاست؟ کدام داده *باید* هم‌مالک باشد تا از توزیع فرار کنیم؟ اگر توزیع اجباری است، orchestration یا choreography — و compensating action چیست؟

**تمرین**

1. جریان «پرداخت + رزرو موجودی + صدور سفارش» را دو بار طراحی کن: (الف) یک واحد مالکیت داده، یک تراکنش محلی؛ (ب) سه سرویس و یک saga. برای (ب) جدول شکست هر مرحله و عمل جبرانی بنویس.
2. ADR: «این workflow را saga نمی‌کنیم چون…» یا «می‌کنیم چون…». اگر دلیل فقط «microservices مد است»، رد است.

**خوب یعنی:** توزیع تراکنش را *آخرین* انتخاب می‌دانی، نه اولین. Saga را با failure mode می‌شناسی، نه با دیاگرام شاد.

**منبع**

- *The Hard Parts*، فصل ۶ *Pulling Apart Operational Data*؛ فصل ۹ *Data Ownership and Distributed Transactions*؛ فصل ۱۱ *Managing Distributed Workflows*؛ فصل ۱۲ *Transactional Sagas* (هشت الگوی saga).
- Sam Newman، *Building Microservices*, 2nd ed.: فصل ۶ *Workflow* (saga در برابر distributed transaction).
- DDIA، فصل ۷ *Transactions* و فصل ۹ *Consistency and Consensus* — 2PC و اجماع چه قیمتی دارند.

### ۲.۴ مدل داده غلط، معماری غلط

**حالت شکست:** همه چیز را در Postgres می‌گذارند تا graph را با JOIN ۲۰تایی می‌خوانند؛ یا همه چیز را document می‌کنند و بعد integrity را در اپلیکیشن دوباره اختراع می‌کنند. مدل از روی عادت انتخاب شده، نه از روی الگوی دسترسی.

**سؤال معماری:** الگوی دسترسی چیست (read/write ratio، locality، graph vs record vs stream) و کدام ناسازگاری *محصول* می‌تواند تحمل کند؟

**تمرین**

1. سه workload بنویس: feed اجتماعی، ledger مالی، جستجوی کاتالوگ. برای هر کدام مدل و store مناسب + یک مشخصه که آگاهانه قربانی می‌شود.
2. مقالهٔ TAO را بخوان و در نیم‌صفحه بگو: چرا Facebook از lookaside cache روی MySQL به graph API رسید — چه failure modeای کش را کشت؟

**خوب یعنی:** store را از *query* انتخاب می‌کنی، از رزومه نه.

**منبع**

- DDIA، فصل ۲ *Data Models and Query Languages*؛ فصل ۴ *Encoding and Evolution*.
- TAO (بالا).
- *The Hard Parts*، فصل ۱۴ *Managing Analytical Data* و Zhamak Dehghani، [How to Move Beyond a Monolithic Data Lake to a Distributed Data Mesh](https://www.martinfowler.com/articles/data-monolith-to-mesh.html) — وقتی analytics متمرکز به گلوگاه سازمان تبدیل می‌شود.

**خروجی مرحله ۲:** یک مدل داده + استراتژی replication/partition برای یک دامنه، یک طراحی saga با جدول شکست، یک ADR مالکیت داده.

---

## مرحله ۳ — وقتی بخشی از سیستم می‌میرد (و بقیه را با خود می‌برد)

**هفته ۸–۱۱**

**چالش شرکت بزرگ:** یک dependency کند می‌شود. retryها طوفان می‌سازند. صف‌ها رشد می‌کنند. کل محصول، نه فقط آن سرویس، از کار می‌افتد. postmortem می‌گوید «timeout نداشتیم».

### ۳.۱ شکست آبشاری

**حالت شکست:** integration point بدون timeout. threadها بلوکه می‌شوند. یک instance می‌میرد، بار روی بقیه می‌ریزد، همه crash-loop می‌شوند. کاربر ۵۰۰ می‌بیند؛ سیستم «سالم» به‌نظر می‌رسد چون health check هم timeout شده.

**سؤال معماری:** ترک از کجا وارد می‌شود، چگونه باید متوقف شود، و کدام قابلیت باید *عمداً* در degradه‌ باقی بماند؟

**تمرین**

1. یک checkout با ۵ وابستگی (پرداخت، موجودی، مالیات، ایمیل، توصیه) بکش. برای هر کدام: hard یا soft dependency؟ اگر soft است، رفتار degrade چیست؟ اگر hard است، چرا؟
2. جدول antipattern: integration point، retry بدون jitter، unbounded result set، unbalanced capacity. برای سیستمی که می‌شناسی، حداقل دو مورد واقعی پیدا کن.

**خوب یعنی:** شکست را *طراحی* می‌کنی. «همه چیز همیشه up» معماری نیست؛ امید است.

**منبع**

- Michael Nygard، *Release It!*, 2nd ed. (۲۰۱۸): فصل ۲ *The Exception That Grounded an Airline*؛ فصل ۴ *Stability Antipatterns* (Integration Points, Cascading Failures, Blocked Threads, Unbounded Result Sets, Dogpile)؛ فصل ۵ *Stability Patterns* (Timeouts, Circuit Breaker, Bulkheads, Fail Fast, Shed Load, Back Pressure).
- Google SRE Book، فصل ۲۲ [Addressing Cascading Failures](https://sre.google/sre-book/addressing-cascading-failures/) — overload، load shedding، retry+jitter، «always go downward in the stack».
- Amazon Builders’ Library، [Timeouts, retries and backoff with jitter](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/)

### ۳.۲ قابلیت اطمینان ۱۰۰٪ یک تصمیم معماری بد است

**حالت شکست:** محصول می‌خواهد 99.99 و همزمان هفته‌ای ۲۰ رلیز. بدون SLO، هر outage سیاسی می‌شود. بدون error budget، هیچ‌کس حق ندارد تغییر را متوقف یا تسریع کند.

**سؤال معماری:** SLI از نگاه کاربر چیست (نه CPU)؟ SLO چقدر است؟ وقتی budget تمام شد، معماری و فرآیند چه باید بکنند؟

**تمرین**

1. برای API پرداخت: ۲–۳ تا SLI (مثلاً availability موفقیت‌آمیز authorize، latency p99). SLO فصلی و error budget معادل دقیقه/درخواست. یک قانون: budget تمام شد → چه تغییری در رلیز؟
2. عمداً SLO را *ضعیف‌تر* از رؤیای محصول بنویس و با یک پاراگراف به مدیر محصول دفاع کن — از فصل *Embracing Risk*.

**خوب یعنی:** قابلیت اطمینان را به‌عنوان بودجهٔ ریسک مدیریت می‌کنی، نه مذهب uptime.

**منبع**

- Google SRE Book، فصل ۳ [Embracing Risk](https://sre.google/sre-book/embracing-risk/)؛ فصل ۴ [Service Level Objectives](https://sre.google/sre-book/service-level-objectives/).
- همان کتاب، فصل ۶ [Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/) — Four Golden Signals: latency, traffic, errors, saturation؛ page برای symptom نه cause.
- Nicole Forsgren, Jez Humble, Gene Kim، *Accelerate*، فصل ۲ *Measuring Performance* — چهار متریک: lead time، deployment frequency، time to restore، change fail rate (سرعت و پایداری با هم، نه علیه هم).

### ۳.۳ دمِ تأخیر (tail latency) مقیاس را می‌خورد

**حالت شکست:** میانگین عالی است، p99 فاجعه. یک fan-out به ۲۰ سرویس، p99 سیستم را p50 بدترین وابستگی می‌کند. Hedging و timeout وجود ندارد؛ ظرفیت اضافه می‌کنند و هزینه می‌سوزد.

**سؤال معماری:** کدام مسیر کاربر interactive است، fan-out چقدر است، و چگونه دم را *تحمل* می‌کنی نه انکار؟

**تمرین**

1. درخواستی که به ۱۲ backend می‌رود مدل کن. اگر هر کدام p99 = 50ms مستقل باشد، p99 کل را تخمین بزن (حتی تقریبی). بعد دو تکنیک tail-tolerant پیشنهاد بده (مثلاً hedged request، early termination، جدا کردن مسیر بحرانی).
2. ADR: «این مسیر باید p99 < X باشد؛ بنابراین fan-out همزمان از N بیشتر ممنوع است مگر با بودجهٔ مشخص.»

**خوب یعنی:** latency را توزیع می‌بینی، میانگین را دروغ می‌دانی.

**منبع**

- Jeffrey Dean & Luiz André Barroso، [The Tail at Scale](https://cacm.acm.org/research/the-tail-at-scale/) (CACM، ۲۰۱۳).
- SRE Book، فصل ۶ — بخش *Worrying About Your Tail*.

### ۳.۴ آشوب کنترل‌شده، نه شجاعت در production

**حالت شکست:** اولین بار که یک AZ می‌میرد، در production واقعی و بدون فرضیه است. یا Chaos Monkey بدون steady-state metric، فقط سرور می‌کشد و چیزی یاد نمی‌دهد.

**سؤال معماری:** steady state قابل‌اندازه‌گیری چیست، چه فرضیه‌ای داری، و شعاع انفجار آزمایش چقدر است؟

**تمرین**

1. یک آزمایش آشوب روی کاغذ: steady state (مثلاً نرخ checkout موفق)، فرضیه، متغیر (timeout وابستگی پرداخت)، معیار رد فرضیه، نحوهٔ محدود کردن blast radius.
2. بنویس چرا *نباید* این آزمایش را این هفته در production اجرا کنی — پیش‌نیاز چیست؟

**خوب یعنی:** آشوب را آزمایش علمی می‌دانی، نه نمایش.

**منبع**

- [Principles of Chaos Engineering](https://principlesofchaos.org/) (ریشه در کار Netflix).
- Netflix Tech Blog، [Chaos Engineering Upgraded](https://netflixtechblog.com/chaos-engineering-upgraded-878d341f15fa)
- Newman، *Building Microservices* 2nd ed.، فصل ۱۲ *Resiliency* و فصل ۱۰ *From Monitoring to Observability*.

**خروجی مرحله ۳:** نقشهٔ وابستگی hard/soft + SLI/SLO/error budget + یک طرح آزمایش آشوب روی کاغذ + ADR پایداری برای یک مسیر حیاتی.

---

## مرحله ۴ — تیم‌ها معماری را می‌سازند (Conway دروغ نمی‌گوید)

**هفته ۱۲–۱۵**

**چالش شرکت بزرگ:** ۵۰ تیم روی یک سیستم. یا همه در یک monolith بدون مرز گیر کرده‌اند، یا ۲۰۰۰ microservice ساخته‌اند که هیچ‌کس نمی‌تواند تغییر را تمام کند. ساختار تیم، شکل سیستم است.

### ۴.۱ قانون Conway را سلاح کن، نه بهانه

**حالت شکست:** تیم‌های لایه‌ای (frontend / backend / DBA / QA) یک معماری لایه‌ایِ کند می‌سازند. یا «inverse Conway» را شعار می‌کنند و تیم را عوض می‌کنند بدون اینکه cognitive load را کم کنند. ارتباط همه با همه = هر سرویس به همه وابسته.

**سؤال معماری:** جریان ارزش کدام است، تیم round-trip یک تغییر را چگونه طی می‌کند، و کدام ارتباط *باید* قطع شود؟

**تمرین**

1. یک org فرضی ۸ تیمی بکش (یا سازمان فعلی‌ات). پیش‌بینی کن معماری شش ماه بعد چه شکلی می‌شود اگر هیچ‌کس دخالت نکند. بعد یک *reverse Conway maneuver* پیشنهاد بده: تیم‌ها را طوری بچین که معماری مطلوب ممکن شود.
2. برای یک stream-aligned team، «Team API» بنویس: چه چیزی را مالک است، چگونه درخواست می‌گیرد، SLA تعامل چیست.

**خوب یعنی:** معماری و org را یک مسئله می‌بینی. نمودار C4 بدون نقشهٔ تیم ناقص است.

**منبع**

- Matthew Skelton & Manuel Pais، *Team Topologies*: فصل ۲ *Conway’s Law and Why It Matters* (از جمله reverse Conway و هشدار استفادهٔ ساده‌لوحانه)؛ فصل ۳ *Team-First Thinking* (cognitive load، Team API)؛ فصل ۵ *The Four Fundamental Team Topologies*؛ فصل ۷ *Team Interaction Modes* (Collaboration / X-as-a-Service / Facilitating).
- Melvin Conway، *How Do Committees Invent?* (Datamation، ۱۹۶۸) — نقل‌شده و بسط‌داده‌شده در Team Topologies فصل ۲.
- Newman، 2nd ed.، فصل ۱۵ *Organizational Structures*.

### ۴.۲ Modular monolith یا microservice — انتخاب اشتباه هر دو را می‌کشد

**حالت شکست نوع A:** big ball of mud؛ هر تغییر همه جا را می‌شکند.  
**حالت شکست نوع B:** *distributed monolith* — شبکه‌دار، بدون استقلال deploy، با تراکنش توزیع‌شدهٔ پنهان. بدترین هر دو دنیا.

**سؤال معماری:** کدام مشکل *الان* داری — modularity، مقیاس مستقل، استقلال تیم، یا تنوع تکنولوژی؟ آیا پیش‌نیاز microservice (provision سریع، monitoring، deploy سریع) را داری؟

**تمرین**

1. جدول تصمیم برای یک محصول ۳۰ نفره / یک محصول ۳۰۰ نفره: modular monolith، service-based، microservice. برای هر سلول: چه مشخصه‌ای برنده می‌شود و چه هزینه‌ای می‌پردازی.
2. مطالعهٔ تطبیقی نیم‌صفحه‌ای: Shopify چرا modular monolith را نگه داشت؛ Uber با ~۲۲۰۰ سرویس چرا DOMA آورد. نتیجه: «پس ما باید…» باید از *اندازه و درد شما* بیاید نه از کپی.

**خوب یعنی:** microservice را پاداش استقلال می‌دانی، نه پیش‌فرض. Distributed monolith را می‌شناسی و از آن می‌ترسی.

**منبع**

- Martin Fowler، [Monolith First](https://martinfowler.com/bliki/MonolithFirst.html) و [Microservice Prerequisites](https://martinfowler.com/bliki/MicroservicePrerequisites.html)
- Thoughtworks، [Microservices in Adopt?](https://www.thoughtworks.com/insights/blog/microservices-adopt) — چرا microservices در Radar هیچ‌وقت Adopt نشد.
- Shopify Engineering، [Deconstructing the Monolith](https://shopify.engineering/deconstructing-monolith-designing-software-maximizes-developer-productivity) (Kirsten Westeinde، ۲۰۱۹)؛ [Under Deconstruction: The State of Shopify’s Monolith](https://shopify.engineering/shopify-monolith)؛ [Enforcing Modularity with Packwerk](https://shopify.engineering/enforcing-modularity-rails-apps-packwerk) — مرز بدون واحد deploy جدید.
- Uber Engineering، Adam Gluck، [Introducing Domain-Oriented Microservice Architecture](https://www.uber.com/us/en/blog/microservice-architecture/) (۲۰۲۰) — domain، layer، gateway، extension؛ پیچیدگی ۲۲۰۰ سرویس.
- Newman، 2nd ed.، فصل ۱ (تفاوت monolith / modular monolith / distributed monolith) و فصل ۲ *How to Model Microservices* (information hiding، coupling، cohesion، DDD).
- *The Hard Parts*، فصل ۲ *Discerning Coupling* (architecture quantum، static vs dynamic coupling)؛ فصل ۷ *Service Granularity*.

### ۴.۳ Granularity: بیش‌ازحد خرد، بیش‌ازحد درشت

**حالت شکست:** یک سرویس per entity (UserService، OrderService، ItemService) → chatty، stamp coupling، و workflow شکننده. یا یک «سرویس کسب‌وکار» که شش bounded context را قورت داده و دوباره monolith است، فقط با REST.

**سؤال معماری:** چه نیرویی *جدا می‌کند* (volatility، مقیاس، امنیت، مالکیت داده، concign load تیم) و چه نیرویی *جمع می‌کند* (تراکنش، chatty I/O، دانش دامنه)؟

**تمرین**

1. یک سرویس verdacht «خیلی ریز» و یکی «خیلی درشت» در سیستمی که می‌شناسی نام ببر. با زبان Hard Parts: disintegrator vs integrator.
2. برای «ثبت سفارش»، سه برش پیشنهاد بده و یکی را با ADR انتخاب کن.

**خوب یعنی:** اندازهٔ سرویس را از *نیرو* می‌گیری نه از تعداد خط کد.

**منبع**

- *The Hard Parts*، فصل ۷ *Service Granularity*؛ فصل ۱۳ *Contracts* (strict vs loose، stamp coupling).
- Newman، 2nd ed.، فصل ۲ و ۴ *Microservice Communication Styles* (sync vs async — این یک تصمیم معماری است نه سلیقه).

### ۴.۴ پلتفرم داخلی؛ وگرنه هر تیم یک cloud می‌سازد

**حالت شکست:** ۲۰ تیم، ۲۰ راه برای auth، logging، deploy. cognitive load stream-aligned team منفجر می‌شود. «platform team» تبدیل به ticket queue می‌شود (تعامل Collaboration اجباری) به‌جای X-as-a-Service.

**سؤال معماری:** پلتفرم *درست به‌اندازه* چیست؟ چه چیزی را باید از تیم محصول بگیری تا جریان ارزش تند شود، بدون اینکه پلتفرم خود monolith سازمانی شود؟

**تمرین**

1. فهرست ۱۰ قابلیت مشترک. ۳ تا که باید platform self-service شوند، ۳ تا که نباید (هنوز). دلیل: کاهش cognitive load نه امپراتوری ابزار.
2. یک interaction mode برای رابطهٔ تیم محصول و پلتفرم انتخاب کن و بنویس چه رفتاری *ممنوع* است (مثلاً «بدون تیکت برای هر namespace»).

**خوب یعنی:** پلتفرم را محصول داخلی با UX می‌دانی، نه تیم زیرساختی با صف.

**منبع**

- *Team Topologies*، فصل ۵ (Platform Teams؛ *A Good Platform Is “Just Big Enough”*) و فصل ۷.
- *Accelerate*، فصل ۲ — معماری loosely-coupled و تحویل سریع هم‌جهت‌اند، نه متضاد.
- Fowler، خلاصهٔ [Team Topologies](https://martinfowler.com/bliki/TeamTopologies.html) — فایدهٔ اصلی پلتفرم: کاهش cognitive load.

**خروجی مرحله ۴:** نقشهٔ تیم↔سیستم، یک تصمیم modular monolith vs services با ADR، یک پیشنهاد granularity، یک تعریف platform «به اندازه».

---

## مرحله ۵ — سیستم زنده است و باید عوض شود: تکامل، هزینه، امنیت، مهاجرت

**هفته ۱۶–۲۰**

**چالش شرکت بزرگ:** بازنویسی بزرگ شکست می‌خورد. صورتحساب cloud از محصول جلو می‌زند. سطح حمله با هر سرویس جدید بزرگ‌تر می‌شود. داده و API مشتری را نمی‌توان یک‌شبه شکست. معماری یعنی تغییر در حال حرکت.

### ۵.۱ مهاجرت تدریجی؛ قربانی کردن سیستم کهنه در یک شب

**حالت شکست:** «rewrite از صفر». ۱۸ ماه بعد، سیستم کهنه هنوز منبع حقیقت است، سیستم نو ناقص است، و دو برابر پیچیدگی داری. یا strangler بدون proxy واضح: ترافیک دو مسیر را می‌رود و هیچ‌کس نمی‌داند کدام درست است.

**سؤال معماری:** کدام قابلیت را اول می‌کشی بیرون، façade کجاست، anti-corruption layer چیست، و شرط خاموش کردن کهنه کدام است؟

**تمرین**

1. یک monolith فروشگاهی فرضی. برنامهٔ strangler برای *Billing* در چهار فاز: transform / coexist / eliminate + معیار خروج هر فاز. بنویس چه چیزی را *عمداً* در monolith می‌گذاری.
2. ریسک Newman را لیست کن: premature decomposition، data-first vs code-first. برای Billing کدام را انتخاب می‌کنی و چرا؟

**خوب یعنی:** مهاجرت یک معماری است با SLO مخصوص خودش، نه یک پروژهٔ قهرمانانه.

**منبع**

- Martin Fowler، [Strangler Fig Application](https://www.martinfowler.com/bliki/StranglerFigApplication.html)
- Newman، 2nd ed.، فصل ۳ *Splitting the Monolith* (incremental migration، Strangler Fig، Parallel Run، Feature Toggle، data decomposition).
- *The Hard Parts*، فصل ۴ *Architectural Decomposition* و فصل ۵ *Component-Based Decomposition Patterns* (از جمله tactical forking).
- Stripe، [Online migrations at scale](https://stripe.com/blog/online-migrations) و [APIs as infrastructure: future-proofing Stripe with versioning](https://stripe.com/blog/api-versioning) — تکامل قرارداد بدون شکستن مشتری.

### ۵.۲ هزینه یک مشخصهٔ معماری است، نه فاکتور مالی بعداً

**حالت شکست:** هر تیم «برای مقیاس» provision می‌کند. fan-out و chatty I/O صورتحساب را نمایی می‌کند. دادهٔ مرده در hot store می‌ماند. هیچ‌کس نمی‌داند هزینهٔ یک درخواست چیست. بهینه‌سازی بعد از alarm مالی می‌آید، یعنی دیر.

**سؤال معماری:** واحد هزینه چیست (درخواست، tenant، GB-ماه)؟ کدام تصمیم معماری (chatty sync، over-sharding، dual-write ابدی، retention بدون TTL) هزینه را قفل می‌کند؟

**تمرین**

1. برای همان checkout: سه تصمیم با هزینهٔ پنهان پیدا کن (مثلاً N+1 بین سرویس‌ها، نگهداری event برای همیشه، multi-AZ برای سرویسی که SLO ندارد). یکی را با ADR عوض کن.
2. یک «cost fitness function» بنویس: هزینهٔ هر ۱۰۰۰ checkout نباید از X تجاوز کند؛ اگر شد، چه بررسی معماری اجباری است؟

**خوب یعنی:** cost را کنار latency و availability در جدول مشخصه‌ها می‌گذاری.

**منبع**

- AWS Well-Architected، [Cost Optimization Pillar](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html) — مصرف، تقاضا/عرضه، پرهیز از overprovision.
- FSA 2nd ed.، فصل ۴ — cost به‌عنوان مشخصه (در نسخهٔ ۲ صریحاً در cloud characteristics هم آمده).
- Stripe Dot Dev، [How Stripe’s document databases supported 99.999% uptime with zero-downtime data migrations](https://stripe.dev/blog/how-stripes-document-databases-supported-99.999-uptime-with-zero-downtime-data-migrations) — حرکت داده بین shard بدون downtime؛ مقیاس *به داخل* هم تصمیم معماری است.

### ۵.۳ امنیت در سیستم توزیع‌شده سطح حمله است، نه فایروال

**حالت شکست:** perimeter trust. هر microservice به دیتابیس بقیه وصل است «چون داخلی است». secret در env بیست سرویس کپی شده. یک سرویس کم‌اهمیت، کلید پرداخت را می‌بیند. Compliance بعد از طراحی می‌آید.

**سؤال معماری:** مرز اعتماد کجاست؟ چه داده‌ای در کدام quantum می‌ماند؟ defense in depth یعنی *کدام لایه چه کنترلی* — نه «همه جا TLS» به‌تنهایی.

**تمرین**

1. threat model سبک برای checkout: بازیگران، trust boundary، حساس‌ترین داده، یک مسیر exploit منطقی (نه ابزار هک — مسیر معماری: سرویس توصیه‌گر به جدول پرداخت). یک کنترل برای هر مرز.
2. ADR: «سرویس X حق ندارد به store پرداخت دسترسی داشته باشد؛ تنها رابط، API با least privilege است.» پیامد عملیاتی را بنویس.

**خوب یعنی:** امنیت را مشخصهٔ ساختاری می‌دانی (مالکیت داده، سطح حمله، identity)، نه چک‌لیست انتهای پروژه.

**منبع**

- AWS Well-Architected، [Security Pillar — Security foundations](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/security.html) — least privilege، traceability، defense in depth، protect data، keep people away from data.
- Newman، 2nd ed.، فصل ۱۱ *Security* — سطح حمله بیشتر می‌شود و فرصت defense in depth هم بیشتر؛ این تعادل است.
- FSA 2nd ed.، فصل ۴–۵ — security به‌عنوان مشخصهٔ متقاطع که با دیگران trade-off دارد.

### ۵.۴ تکامل قرارداد و دادهٔ تحلیلی؛ سیستم دو عمر دارد

**حالت شکست:** Breaking change در API عمومی بدون version. یا data lake مرکزی که همه pipelineها به آن وابسته‌اند و دامنهٔ سفارش نمی‌تواند بدون تیم data یک فیلد اضافه کند. Operational data و analytical data با یک مدل و یک تیم اداره می‌شوند تا هر دو ضعیف شوند.

**سؤال معماری:** قرارداد چقدر strict باشد؟ نسخه یعنی چه کسی هزینه می‌پردازد — تو یا مشتری؟ دادهٔ تحلیلی محصول کدام دامنه است؟

**تمرین**

1. سیاست نسخه برای API داخلی و API خارجی بنویس (الهام از Stripe: نسخهٔ تاریخ‌دار، pin حساب، تغییر کوچک، encapsulation هزینهٔ نسخهٔ کهنه).
2. یک «data product» برای دامنهٔ سفارش تعریف کن: مالک، SLA داده، مصرف‌کننده، و آنچه lake مرکزی *دیگر* مالک آن نیست.

**خوب یعنی:** تکامل را طراحی می‌کنی. سیستم موفق، سیستم تغییرپذیر است.

**منبع**

- Stripe، [APIs as infrastructure](https://stripe.com/blog/api-versioning)
- DDIA، فصل ۴ *Encoding and Evolution*؛ فصل ۱۲ *The Future of Data Systems*.
- *The Hard Parts*، فصل ۱۳ *Contracts*؛ فصل ۱۴ *Managing Analytical Data*.
- Dehghani، [Data monolith to mesh](https://www.martinfowler.com/articles/data-monolith-to-mesh.html)
- Newman، 2nd ed.، فصل ۱۶ *The Evolutionary Architect*.

**خروجی مرحله ۵:** برنامهٔ strangler یک زیرسیستم، یک cost fitness function، یک threat model مرزی، یک سیاست نسخه/داده.

---

## ترتیب خواندن (اگر فقط یک خط از هر کتاب)

| ترتیب | چه | چرا همین‌جا |
| --- | --- | --- |
| ۱ | FSA 2nd: فصول ۴–۷ + Hard Parts فصل ۱ و ۱۵ | زبان مشخصه و trade-off |
| ۲ | Fowler ADR + Monolith First + Microservice Prerequisites | جلوگیری از مدزدگی |
| ۳ | DDIA ۱، ۲، ۵، ۶، ۷، ۸ | واقعیت داده و توزیع |
| ۴ | Release It! ۴–۵ + SRE ۳، ۴، ۶، ۲۲ + Tail at Scale | شکست به‌عنوان طراحی |
| ۵ | Team Topologies ۲، ۳، ۵، ۷ + Shopify/Uber posts | معماری = سازمان |
| ۶ | Newman ۳، ۶، ۱۱، ۱۵، ۱۶ + Hard Parts ۶، ۷، ۹، ۱۲ | مرز، saga، امنیت، تکامل |
| ۷ | Accelerate فصل ۲ + AWS Cost/Security pillars + Stripe versioning/migrations | هزینه، امنیت، مهاجرت زنده |

DDIA فصل ۸ *The Trouble with Distributed Systems* را وقتی خواندی که وسوسه شدی «شبکه را قابل‌اعتماد فرض کن». همان‌جا بمان تا ناراحتی‌اش ته بکشد.

---

## تمرینِ کلاه بزرگ (هفتهٔ آخر)

یک شرکت فرضی: marketplace با پرداخت، ۳۰→۳۰۰ مهندس در دو سال، Black Friday، الزام PCI، صورتحساب cloud در حال انفجار، و یک Rails/Java monolith ده ساله.

تحویل در **۱۰ صفحه یا کمتر** (نه اسلاید تزئینی):

1. ۳ مشخصهٔ غیرقابل‌مذاکره و ۲ قربانی آگاهانه
2. نقشهٔ تیم (Team Topologies) و پیش‌بینی Conway
3. تصمیم modular monolith vs استخراج انتخابی سرویس — با پیش‌نیاز Fowler
4. مالکیت دادهٔ Order/Inventory/Payment و یک workflow که saga *نیست*
5. SLO پرداخت + رفتار cascade اگر پرداخت کند شود
6. فازهای strangler برای Billing
7. یک cost fitness function و یک کنترل امنیت مرزی
8. ۳ ADR

اگر نتوانستی trade-offها را نام ببری و فقط الگوها را ردیف کردی، مسیر را از مرحله ۱ تکرار کن.

---

## معیار «این مسیر تمام شد»

- تصمیم را با *نیرو / مشخصه / پیامد* توضیح می‌دهی، نه با نام ابزار.
- Distributed monolith را در یک دیاگرام واقعی تشخیص می‌دهی.
- Failure mode را قبل از happy path می‌نویسی.
- Org chart را بخشی از معماری می‌دانی.
- یک ADR را در برابر مد روز دفاع می‌کنی.

---

## منابع (فقط آنچه در مسیر آمده)

**کتاب**

- Richards & Ford، *Fundamentals of Software Architecture*, 2nd ed., O’Reilly، ۲۰۲۵
- Ford, Richards, Sadalage, Dehghani، *Software Architecture: The Hard Parts*, O’Reilly، ۲۰۲۱
- Kleppmann، *Designing Data-Intensive Applications*, O’Reilly، ۲۰۱۷
- Skelton & Pais، *Team Topologies*, IT Revolution، ۲۰۱۹
- Nygard، *Release It!*, 2nd ed., Pragmatic Bookshelf، ۲۰۱۸
- Newman، *Building Microservices*, 2nd ed., O’Reilly، ۲۰۲۱
- Forsgren, Humble, Kim، *Accelerate*, IT Revolution، ۲۰۱۸
- Beyer et al.، *Site Reliability Engineering*, O’Reilly، ۲۰۱۶ — متن آزاد: [sre.google/sre-book](https://sre.google/sre-book/)
- Ford, Parsons, Kua، *Building Evolutionary Architectures* (مفهوم fitness function)

**مقاله و مهندسی شرکت‌ها**

- Fowler: ADR، Monolith First، Microservice Prerequisites، Strangler Fig، Team Topologies bliki، Data Mesh (میزبان مقالهٔ Dehghani)
- Thoughtworks Radar: Architectural fitness function؛ *Microservices in Adopt?*
- Shopify Engineering: Deconstructing the Monolith؛ Under Deconstruction؛ Packwerk
- Uber Engineering: Domain-Oriented Microservice Architecture
- Netflix / principlesofchaos.org: Chaos Engineering
- Meta: TAO (بلاگ ۲۰۱۳ + ATC ’۱۳)
- Stripe: API versioning؛ Online migrations at scale؛ Stripe Dot Dev: zero-downtime data migrations / DocDB
- Amazon Builders’ Library: Timeouts, retries and backoff with jitter
- AWS Well-Architected: Cost Optimization Pillar؛ Security Pillar
- Dean & Barroso، *The Tail at Scale*, CACM ۲۰۱۳
- InfoQ: *Has Your Architectural Decision Record Lost Its Purpose?*
- Conway، *How Do Committees Invent?*, Datamation ۱۹۶۸

---

*این سند مسیر یادگیری است، نه گواهی معمار. معماری با تصمیم‌های دفاع‌شده در شرایط واقعی ساخته می‌شود.*

</div>
