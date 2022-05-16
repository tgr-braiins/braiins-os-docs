
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#############
پیکربندی
#############

.. contents::
  :local:
  :depth: 2

پیکربندی Braiins Farm Proxy را می توان به دو دسته اصلی تقسیم کرد - پیکربندی مسیریابی و پیکربندی برنامه های همراه (که Prometheus و Grafana هستند).

*********************
پیکربندی مسیریابی
*********************

پیکربندی مسیریابی در فایل های TOML با استفاده از یک ساختار خاص انجام می شود. ساختار فایل پیکربندی مطابق با نمودار مسیریابی هش ریت است و از اصطلاحات توضیح داده شده در بالا استفاده می کند. پیکربندی در تنظیمات نمونه در متن بعدی توضیح داده شده است.

Braiins Farm Proxy دارای ۳ نمونه از پیش تعریف شده TOML است که در فولدر *./farm-proxy/config* قرار گرفته است.

* *minimal.toml*: کوچکترین فایل پیکربندی که می توانید با یک سرور، یک هدف و تجمیع پیش فرض داشته باشید.
* *sample.toml*: شامل برخی پارامترها و فیلدهای دیگر است که آنها را توضیح می دهد. به عنوان فایل پیکربندی پیش فرض در *docker-compose.yml* استفاده می شود. همچنین شامل یک سرور، یک هدف است.
* *two_servers_two_targets.toml*: مثال برای نحوه نمایش سرورهای بیشتر برای ماینرها یا تعریف یک سرور پشتیبان برای Braiins Farm Proxy.


پیکربندی مسیریابی از ۵ بخش تشکیل شده است: سرور، هدف، مسیریابی، هدف مسیریابی و سطح هدف مسیریابی.

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/grZ3aQG9mHE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Server 
=======

سرور یک قالب برای اتصالات downstream است. هر سرور فقط در یک دامنه مسیریابی قابل استفاده است.

.. code-block:: shell

      [[server]]
      name = "s1"
      port = 3336
      slushpool_bos_bonus = "<slushpool username>"
      bos_referral_code = "<Braiins OS+ referral code>"



* **name**: نام سرور. این به عنوان یک مقدار بعد "سرور" در تمام معیارهای مربوط به پایین دست (submits, shares, connections) در نظارت Grafana قابل مشاهده است.
* **port**: پورتی را تعریف می کند که Braiins Farm Proxy باز می شود و اتصالات ماینر را می پذیرد.
* **slushpool_bos_bonus**: نام کاربری Slush Pool که پاداش +Braiins OS برای آن اعمال می شود.
* **bos_referral_code**: کد ارجاع +Braiins OS.
   
Target
=======

Target الگویی برای اتصالات upstream است. می توان آن را بین چندین دامنه مسیریابی به اشتراک گذاشت.

.. code-block:: shell

      [[target]]
      name = "MP-GL1"
      url = "stratum+tcp://<mining-pool-address>"
      user_identity = "<userName.workerName>"

* **name**: نام هدف به عنوان یک مقدار بعد “upstream” در تمام معیارهای مربوط به upstream یعنی (submits, shares, connections) در مانیتورینگ Grafana قابل مشاهده است.
* **url**: آدرس استخر ماینینگ.
* **user_identity**: هویتی که بر اساس آن هش ریت باید ارسال شود. **userName** باید در استخر مورد نظر وجود داشته باشد در غیر این صورت استخر کلیدی برای پیوند هش ریت شما به حساب شما ندارد.

Routing Domain
===============

دامنه مسیریابی مرزها و اولویت ها را برای تخصیص هش ریت به اهداف مورد نظر تعریف می کند.

.. code-block:: shell

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal 1"
      hr_weight = 100
      [[routing.goal.level]]
      targets = ["MP-GL1"]

* **from**: لیست سرورهایی که در Braiins Farm Proxy به عنوان پراکسی های تجمیع استفاده می شوند.
* **goal**: فهرست قوانین مسیریابی ویژگی **name** هدف در داشبورد Grafana برای اقدامات مربوط به بالادست قابل مشاهده است. ویژگی **hr_weight** مخفف اولویت نسبت توزیع هش است. مراقب وزن باشید نه درصد. به عنوان مثال، نسبت وزن 2:1 هش ریت را در نقاط پایانی هدف توزیع می کند. 67 درصد از هش ریت به هدف با وزن 2 و 33 درصد هش ریت به هدف با وزن 1 می رود. در پیکربندی های مثال پایین تر، می توانید نحوه توزیع هش ریت را در چندین هدف مشاهده کنید.
* سطح هدف مسیریابی **targets** را فهرست می کند که باید به عنوان نقاط پایانی upstream اعمال شوند.

در صورتی که فارم از +‌Braiins OS بر روی دستگاه های خود استفاده کند، **مسیریابی dev fee به صورت خودکار انجام می شود.**

Workers پیکربندی
=====================

برای هدایت هش ریت فارم به Braiins Farm Proxy، ورکرها باید دوباره پیکربندی شوند. URL Pool در پیکربندی فریم‌ور ورکر باید به صورت زیر تنظیم شود:

 * Stratum V1: ``stratum+tcp://<farm-proxy-url>:<server_port>``
 *  Stratum V2: ``stratum2+tcp://<farm-proxy-url>:<server_port>/<public_key>``

توصیه می شود در صورتی که Braiins Farm Proxy کار نمی کند، یک اتصال استخر پشتیبان نیز در ماینر خود داشته باشید.

پیکربندی‌های نمونه
======================

برای درک بهتر استفاده از Braiins Farm Proxy و پیکربندی، اجازه دهید ۳ مثال را مرور کنیم.

* **پیکربندی حداقلی**: ساده ترین پیکربندی ممکن، یک سرور، یک استخر هدف. به دلیل سادگی آن برای دنیای واقعی مناسب نیست، اما منطق پیکربندی را توصیف می کند.

.. code-block:: shell

      # Minimal sample configuration
      [[server]]
      name = "s1"                                
      port = 3336

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "simpleFarm.worker"

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal 1"
      [[routing.goal.level]]
      targets = ["SP-GL"]


* **پیکربندی پایه**: به عنوان مثال با یک عملیات ماینینگ در یک مرکز واحد واقع در اروپا. هدف اصلی Slush Pool (URL اتحادیه اروپا) است، اما توسط URL های عمومی و روسی Slush Pool پشتیبانی می شود. فارم دارای ۷۰۰۰۰ دستگاه ASIC است و تجمع مورد نظر آن 100 است. یعنی باید بین ۶ تا ۷ اتصال upstream به هدف وجود داشته باشد. درآمد فارم با استفاده از فریم‌ور +Braiins OS و استخراج در Slush Pool افزایش می یابد.

.. code-block:: shell

      # Basic sample configuration
      [[server]]
      name = "s1"
      port = 3336

      [[target]]
      name = "SP-EU"
      url = "stratum+tcp://eu.stratum.slushpool.com"
      user_identity = "basicFarm.proxy"
      aggregation = 100

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "basicFarm.proxy"
      aggregation = 100

      [[target]]
      name = "SP-RU"
      url = "stratum+tcp://ru-west.stratum.slushpool.com"
      user_identity = "basicFarm.proxy"
      aggregation = 100

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal 1"
      # Primary
      [[routing.goal.level]]
      targets = ["SP-EU"]
      # Back-up 1
      [[routing.goal.level]]
      targets = ["SP-GL"]
      # Back-up 2
      [[routing.goal.level]]
      targets = ["SP-RU"]

* **مالک‌های چندگانه ورکرها**: فارم دارای ورکرهای اختصاص داده شده برای استخراج در Slush Pool با پورت 3336 و سایر ورکرها به استخراج در Antpool در پورت 3337 اختصاص داده شده اند. این پیکربندی مثال برای مواردی مناسب است که ورکرها ۲ مالک داشته باشند و بنابراین چندین سرور تعریف و استفاده می شود. چندین سخت‌افزار پیاده سازی شده از Braiins Farm Proxy (بگذارید در مثال ما بگوییم ۲ دستگاه Raspberry Pi هستند) با ۲ پیکربندی مختلف می توانند استفاده شوند.
   
.. code-block:: shell

      # Advanced sample configuration
      [[server]]
      name = "s1"
      port = 3336

      [[server]]
      name = "s2"
      port = 3337
      extranonce_size = 2

      [[target]]
      name = "SP-EU"
      url = "stratum+tcp://eu.stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50                                                      

      [[target]]
      name = "Antpool-1"
      url = "stratum+tcp://ss.antpool.com:3333"
      user_identity = "antPoolUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[target]]
      name = "Antpool-2"
      url = "stratum+tcp://ss.antpool.com:443"
      user_identity = "antPoolUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[routing]]
      from = ["s1","s2"]
      [[routing.goal]]
      name = "Goal SP"
      # Primary Slush Pool
      [[routing.goal.level]]
      targets = ["SP-EU"]
      # Back-up Slush Pool
      [[routing.goal.level]]
      targets = ["SP-GL"]
      #
      [[routing.goal]]
      name = "Goal Ant"
      # Primary Antpool
      [[routing.goal.level]]
      targets = ["Antpool-1"]
      # Back-up Antpool
      [[routing.goal.level]]
      targets = ["Antpool-2"]

* **تنوع بخشیدن به استخرها**: فارمی که با استفاده از ۱ نمونه Braiins Farm Proxy با ۱ سرور و چندین نقطه پایانی هدف upstream با تخصیص هش ۱۰۰:۸۰:۲۰ ~ تقریباً، هش را به ۳ استخر اختصاص می دهد. ۵۰ درصد هش ریت به هدف “Goal SP”  و ۴۰ درصد هش به هدف “Goal Ant” و ۱۰ درصد به هدف “Goal BTC.com” می رسد.

.. code-block:: shell

      # Diversification of pools
      [[server]]
      name = "s1"
      port = 3336
      extranonce_size = 2

      [[target]]
      name = "SP-EU"
      url = "stratum+tcp://eu.stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50

      [[target]]
      name = "Antpool-1"
      url = "stratum+tcp://ss.antpool.com:3333"
      user_identity = "antUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[target]]
      name = "Antpool-2"
      url = "stratum+tcp://ss.antpool.com:443"
      user_identity = "antUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[target]]
      name = "BTCcom-1"
      url = "stratum+tcp://eu.ss.btc.com:1800"
      user_identity = "btcUser.proxy"
      aggregation = 50

      [[target]]
      name = "BTCcom-2"
      url = "stratum+tcp://eu.ss.btc.com:443"
      user_identity = "btcUser.proxy"
      aggregation = 50

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal SP"
      hr_weight = 100
      # Primary Slush Pool
      [[routing.goal.level]]
      targets = ["SP-EU"]
      # Back-up Slush Pool
      [[routing.goal.level]]
      targets = ["SP-GL"]
      #
      [[routing.goal]]
      name = "Goal Ant"
      hr_weight = 80
      # Primary Antpool
      [[routing.goal.level]]
      targets = ["Antpool-1"]
      # Back-up Antpool
      [[routing.goal.level]]
      targets = ["Antpool-2"]
      #
      [[routing.goal]]
      name = "Goal BTC.com"
      hr_weight = 20
      # Primary BTC.com
      [[routing.goal.level]]
      targets = ["BTCcom-1"]
      # Back-up BTC.com
      [[routing.goal.level]]
      targets = ["BTCcom-2"]

* **مکان های مختلف عملیات ماینینگ**: فارم‌های ماینینگ با چندین کانتینر استخراج فیزیکی یا ساختمان در مکان‌های مختلف، از یک نمونه Braiins Farm Proxy در هر یک از مکان‌ها یا برای هر کانتینر با یک سرور downstream و یک هدف upstream با شناسه‌های ورکر مختلف در هر مکان / کانتینر استفاده می‌کنند تا هش ریت را از هم متمایز کند. در هر مکان / کانتینر می‌توان از طریق نمونه دیگری از Braiins Farm Proxy، پراکسی‌های فارم را به‌صورت سلسله مراتبی به هش‌ریت جمع‌آوری شده از پروکسی‌های فارم کانتینرهای جداگانه پیوند داد.
   
پیکربندی پارامترها
========================

فهرستی از پارامترهای اجباری و اختیاری موجود در پیکربندی Braiins Farm Proxy. پارامترها به بخش های پیکربندی مربوطه اختصاص داده می شوند.

Server
------

 * **name**: string: حساس به حروف کوچک و بزرگ با حداقل طول ۱ (اجباری)، نام سرور،
 * **port**: integer (اجباری)، پورت اختصاص داده شده به Braiins Farm Proxy،
 * **extranonce_size**: integer (اختیاری)، extranonce ارائه شده به دستگاه پایین دست (ASIC)، باید حداقل ۲ برابر کمتر از *extranonce_size* در *target* باشد، پیش فرض *4* است،
 * **validates_hash_rate**: boolean (true/false, اختیاری), پارامتر تعیین می کند که آیا پروکسی باید ارسال را از پایین دست اعتبار سنجی کند، پیش فرض *true* است،
 * **use_empty_extranonce1**: boolean (true/false, اختیاری), پارامتر تعیین می کند که آیا می توان از 1 بایت بیشتر از nonce اضافی استفاده کرد (هر دستگاهی آن را پشتیبانی نمی کند)، پیش فرض *false* است،
 * **submission_rate**: real (اختیاری), نرخ ارسال مورد نظر پایین‌دستی (miner -> proxy) که به‌عنوان تعداد ارسال‌ها در یک ثانیه تعریف می‌شود، پیش‌فرض *0.2* (1 ارسال در هر 5 ثانیه) است.
 * **slushpool_bos_bonus**: string: حساس به حروف کوچک با حداقل طول 0 (اختیاری)، نام کاربری Slush Pool که برای آن تخفیف +‌Braiins OS اعمال می شود،
 * **bos_referral_code**: string: حساس به حروف کوچک و بزرگ با حداقل طول 6 (اختیاری)، کد ارجاع +Braiins OS در طول کامل برای دریافت جایزه ارائه می شود.
   
Target
------

 * **name**: string: حساس به حروف کوچک و بزرگ با حداقل طول ۱ (اجباری)، نام نقطه پایانی هدف،
 * **url**: string (اجباری), آدرس استخر استخراج،
 * **user_identity**: string: حساس به حروف کوچک و بزرگ با حداقل طول ۱ (اجباری)،
 * **identity_pass_through**: boolean (true/false, اختیاری), انتشار یک هویت ورکر تکی به استخر هدف (ارسال ویژگی به بالادست)، پیش‌فرض *false* است،
 * **extranonce_size**: integer (اختیاری), Extranonce اعمال شده در استخر هدف، باید حداقل 2  برابر بیشتر از *extranonce_size* موجود در *server* باشد، پیش فرض *6* است (**بعضی از استخرها حداکثر به 4 نیاز دارند: AntPool، Binance Pool، Luxor**) ،
 * **aggregation**: integer (اختیاری), تعداد ورکرها انبوه (ASIC) در هر یک اتصال بالادست، پیش‌فرض *50* است.
   
Routing
-------

 * **name**: string: حساس به حروف کوچک و بزرگ با حداقل طول ۱ (اجباری)، نام دامنه مسیریابی،
 * **from**: لیست (اجباری)، لیست سرورهایی که به عنوان پراکسی های تجمیع کننده استفاده می شوند.
   
Routing Goal
------------

 * **name**: string: حساس به حروف کوچک و بزرگ با حداقل طول ۱ (اجباری)، نام هدف مسیریابی،
 * **hr_weight:** integer (اختیاری), وزن برای نسبت ترجیحی توزیع هش ریت.
   
Routing Goal Level
------------------

 * **targets**: فهرست (اجباری)، فهرست اهدافی که به عنوان نقاط پایانی هدف در دامنه مسیریابی اعمال می شوند.

**************************
پیکربندی همراه
**************************

پیکربندی دیگر در فایل *docker-compose.yml* از پیش تعریف شده است که یک برنامه ضروری برای اجرای Braiins Farm Proxy به عنوان یک پشته Docker چند کانتینری است. این فایل پیکربندی به گونه ای طراحی شده است که به حداقل ویرایش ممکن نیاز دارد. Docker-compose شامل پیکربندی این خدمات است:

 * **Prometheus**: runs on port **9090**, it can be accessed in your browser, e.g. ``http://<your-host>:9090/``
 * **Node Exporter**: runs on port **9100**, it can be accessed in your browser, e.g. ``http:/<your-host>:9100/``
 * **Grafana**: runs on port **3000**, it can be accessed in your browser, e.g. ``http://<your-host>:3000/``

Grafana برای مانیتورینگ بر استخراج با Braiins Farm Proxy بسیار مهم است. Prometheus می تواند مفید باشد در صورتی که کاربر بخواهد نمودارهای خود را برای داشبورد Grafana بسازد. Node Exporter صادرکننده پارامترهای سیستم عامل و سرور برای پایگاه داده Prometheus است.

.. attention::

   فایل *docker-compose.yml* به یک فایل پیکربندی **sample.toml** در پیکربندی کانتینر farm-proxy اشاره دارد. اگر اپراتور فارم، فایل پیکربندی خود را داشته باشد و بخواهد آن را به farm-proxy آدرس دهی کند، sample.toml باید با آن فایل جایگزین شود. در زیر می توانید پیکربندی farm-proxy را در *docker-compose.yml* مشاهده کنید.


.. code-block:: shell

      farm-proxy:
      image: braiinssystems/farm-proxy:v1.0.0-rc4
      container_name: farm-proxy
      network_mode: "host"
      volumes:
      - "./config/sample.toml:/conf/farm_proxy.yml"
      environment:
      - CONF_PATH=/conf/farm_proxy.yml
      - RUST_LOG=debug
      - RUST_BACKTRACE=full
      restart: unless-stopped
      logging:
      driver: "json-file"
      options:
      max-size: "100m"
      max-file: "50"
      compress: "true"

