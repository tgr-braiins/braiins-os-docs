
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

.. _monitoring:

==================================================
مانیتورینگ بر Braiins OS+ با Prometheus و Grafana
==================================================

.. فهرست::
  :local:
  :depth: 2

مقدمه
============

با شروع با Braiins OS + 22.02.2، هر ماینر آماری را به شکلی تولید می کند که به راحتی توسط Prometheus قابل هضم باشد. سپس داده های Prometheus را می توان در Grafana مشاهده کرد.

Prometheus یک جعبه ابزار مانیتورینگ و هشدار سیستم منبع باز است. Prometheus معیارهای خود را به عنوان داده های سری زمانی جمع آوری و ذخیره می کند، یعنی اطلاعات متریک با مهر زمانی که در آن ثبت شده است، در کنار جفت های اختیاری کلید-مقدار به نام برچسب ها ذخیره می شود.

نسخه متن باز Grafana یک نرم افزار تجسم و تجزیه و تحلیل متن باز است. به شما امکان می دهد معیارهای خود را پرس و جو کنید، تجسم کنید، هشدار دهید و کاوش کنید. این ابزارها را در اختیار شما قرار می دهد تا داده های پایگاه داده سری زمانی خود را به نمودارها و تجسم های روشنگر تبدیل کنید.

Prometheus + Grafana تقریباً یک استاندارد صنعتی برای مانیتورینگ، تجسم و هشدار است. برای Braiins OS+، نقطه پایانی معیارهای Prometheus در ``[IP ADDRESS]:8081/metrics`` موجود است.

.. توجه::
   
   مانیتورینگ را می توان به عنوان یک برنامه مستقل اجرا کرد یا به صورت پشته Docker Proxy Braiins Farm همراه شده است.

محدودیت ها
-----------

-  فقط مجموعه محدودی از معیارها برای دستگاه های S9 در دسترس است
-  مانیتورینگ فقط برای ماینرهایی که Braiins OS+ نصب کرده اند کار می کند
-  معماری های پشتیبانی شده برای اسکنر ssh لینوکس AMD64، RPi 64bit یا 32bit هستند.

راه اندازی
=====

شروع سریع
-----------

شروع سریع برای مواردی که مانیتورینگ به عنوان یک برنامه مستقل اجرا می شود (نه به عنوان بخشی از Braiins Farm Proxy).

- ``git clone https://github.com/braiins/bos-farm-monitor.git``
- فهرست محدوده آدرس IP را برای اسکن در ``./scan_crontab`` تغییر دهید
- ``docker-compose up -d``
- به localhost:3000 بروید

پیش نیازها
-------------

**Linux**

-  دستورالعمل‌های `نصب docker را دنبال کنید <https://docs.docker.com/engine/install/ubuntu/>`__
-  مراحل `نصب اختیاری docker post را دنبال کنید <https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user>`__
-  مراحل `نصب docker-compose را دنبال کنید <https://docs.docker.com/compose/install/>`__

**RPi**

-  یکی از راهنماهای متعدد را دنبال کنید - به عنوان مثال. https://jfrog.com/connect/post/install-docker-compose-on-raspberry-pi/

نصب
------------

شروع سریع در صورتی که مانیتورینگ به عنوان یک برنامه مستقل اجرا شود (نه به عنوان بخشی از Braiins Farm Proxy).

**تأیید پیش نیاز نصب شده**

.. code-block::

    docker version
    docker-compose --version
    git --version

**تأیید پیش نیاز نصب شده**

می توانید با استفاده از git مخزن را کلون کنید:

.. code-block::

   sudo apt update
   sudo apt install git
   git clone https://github.com/braiins/bos-farm-monitor.git

می توانید فایل فشرده را با تمام فایل ها دانلود کنید `https://github.com/braiins/bos-farm-monitor/archive/refs/heads/master.zip <https://github.com/braiins/bos-farm-monitor/archive/refs/heads/master.zip>`__

Prometheus پیکربندی
------------------------

قبل از اینکه بتوانید مانیتورینگ بر فارم خود را شروع کنید، باید آماده شوید
:پیکربندی بر اساس مثال هایی در دایرکتوری پیکربندی. فایل ها دو تا هستند


-  ``/config/prometheus_scan.yml``
-  ``/config/prometheus_static.yml``

تنها تفاوت عمده بین این دو این است که "scan" بهترین کار است
در صورتی که ماینرهای شما دارای آدرس IP اختصاص داده شده توسط DHCP باشند، استفاده می شود
زمانی که ماینرهای شما آدرس IP ایستا دارند، می توان از "static" استفاده کرد.

**پیکربندی پیش فرض**

پیکربندی پیش فرض در دارای ویژگی های زیر است:

-  کار برای اسکرپینگ معیارهای Braiins OS+ با نام braiinsos-data
-  برچسب گذاری مجدد آدرس های نقطه پایانی متریک (حذف پورت 8081)
-  تجزیه آدرس های IP:

   -  اکتت دوم: برچسب ``site_id``
   -  اکتت سوم: برچسب ``subnet_id``
   -  هشتم چهارم: برچسب ``host_id``

-  حذف برخی از معیارهای پرقدرت داده (شما می توانید آنها را دوباره اضافه کنید، فقط مطمئن شوید که اندازه نمونه شما مناسب است)
-  ساخت برچسب استاتیک برای prometheus_static.yml - برچسب زمانی که از prometheus_scan.yml استفاده می شود به صورت پویا اختصاص داده می شود (در ادامه در این مورد بیشتر توضیح خواهیم داد).

**فارم خود را برای مشاهده خوب ساختار دهید**

برای یک فارم بزرگتر، ممکن است بخواهید استخراج کنندگان را به چند گروه منطقی گروه بندی کنید
گروه بندی به طوری که بتوانید عملکرد را بر اساس اجزای جداگانه مشاهده کنید. این
گروه بندی ممکن است بسته به اندازه و ساختار فارم شما متفاوت باشد،
برخی از معمول ترین عناصر در توپولوژی فارم عبارتند از:

-  ساختمان
-  بخش
-  تانکر
-  راهرو
-  ردیف

برای رسیدن به این هدف شما گزینه های زیر را دارید:

 **از زیرشبکه ها استفاده کنید و اکتت آدرس های IP را تجزیه کنید**
   اگر آدرس‌های IP ثابت دارید و از آن‌ها برای سازماندهی ماینرهای خود استفاده می‌کنید، ساده‌ترین راه برای آماده‌سازی داده‌ها برای گزارش‌دهی، بهبود پیکربندی Prometheus با برچسب‌های مجدد مشتق شده از آدرس‌های IP است. مثال زیر نحوه انجام آن را نشان می دهد. بدیهی است که می توانید از نام های متفاوتی نسبت به بخش، مخزن، ماینر استفاده کنید.

   .. code-block::

      relabel_configs:
      # Extract the second octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.(\\d+)\\.\\d+\\.\\d+.*"
        target_label: "section"
      # Extract the third octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.\\d+\\.(\\d+)\\.\\d+.*"
        target_label: "tank"
      # Extract the last octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.\\d+\\.\\d+\\.(\\d+).*"
        target_label: "miner"
 
 **از کارهای جداگانه همراه با یک برچسب سفارشی اختیاری استفاده کنید**
یک پیکربندی از Prometheus (ذخیره شده در prometheus.yml) می تواند شامل چندین کار باشد. به عنوان مثال، می توانید برای هر ساختمان یا کانتینر کارهای جداگانه ایجاد کنید. هر معیار دارای یک برچسب کاری است، که آن را به یک رویکرد بسیار راحت برای نمونه های گروهی (غیراستخراج کننده) تبدیل می کند. در صورتی که کارهای دیگری (غیر استخراج) در پیکربندی خود دارید، ممکن است بخواهید یک برچسب سفارشی به هر کار اضافه کنید تا بتوانید از آن برچسب برای فیلتر کردن/گروه‌بندی استفاده کنید. مثالی که می تواند در بخش relabel_configs برای افزودن یک برچسب ساختمان به هر نمونه ای که توسط کار با مقدار "Building A" مانیتورینگ می شود، استفاده شود:

   .. code-block::

      - target_label: "building"
        replacement: "Building A"

 **از چندین نمونه Prometheus استفاده کنید**
   در مورد هزاران یا بیشتر ماینر، ممکن است تنظیم یک نمونه Prometheus جداگانه برای هر گروه از ماینرها آسان تر باشد. به اسناد پرومته در مورد نحوه راه اندازی `فدراسیون <https://prometheus.io/docs/prometheus/latest/federation/>`__ مراجعه کنید.

 **استفاده از نام کاربری/نام ورکر و برچسب‌های مجدد (توصیه نمی‌شود)**
استفاده از نام کاربری/نام ورکر برای رمزگذاری اطلاعات مربوط به مکان فیزیکی ماینرها یک رویکرد معمولی است که در برنامه های نظارت قدیمی استفاده می شود. این رویکرد با نحوه مدیریت و ذخیره سری های زمانی Prometheus که هیچ شباهتی به پایگاه داده های سنتی رابطه ای ندارد، به خوبی کار نمی کند. ما استفاده از نام کاربری/نام ورکر را برای ساختار مزرعه خود با Prometheus به دلایل زیر توصیه نمی کنیم:

   -  اکثر معیارها دارای نام ورکر به عنوان برچسب نیستند و پیوندها باید در پرس و جو ایجاد شوند (کارها را کند می کند، مستعد خطا)
   -  ممکن است چندین نام کاربری / نام ورکر مرتبط با یک ماینر وجود داشته باشد. این پیوندها را حتی دشوارتر می کند (پیش تجمیع ضروری با منطق که کدام مقدار را انتخاب کنید)

 **از چندین محدوده IP با رویکرد اسکن استفاده کنید**
   اگر ماینرهایی با IP اختصاص داده شده توسط DHCP دارید و از اسکن شبکه خود برای انتقال ماینرها به Prometheus استفاده می کنید، می توانید چندین محدوده شبکه را تعریف کنید و هر محدوده می تواند یک مقدار منحصر به فرد تعریف شده و به برچسب اختصاص داده شود (در بخش بعدی در مورد آن بیشتر توضیح دهید. ).

**افزودن ماینرها به پیکربندی**

گزینه های اساسی زیر برای اضافه کردن ماینرها به پیکربندی وجود دارد:

-  از گزینه های کشف خدمات ارائه شده توسط Prometheus استفاده کنید
-  آدرس های IP را در فایل پیکربندی به صورت دستی فهرست کنید

فهرست کردن آدرس های IP به طور مستقیم زمانی بهترین کار را دارد که آدرس های IP اختصاص داده شده به ماینرها ثابت باشند. در مورد DHCP، کشف سرویس گزینه بهتری است.

**Service Discovery**

کشف سرویس مبتنی بر فایل گزینه ای است که به طور پیش فرض فعال شده است. برای شروع
با استفاده از آن، باید فایل ``./scan_crontab`` را در یک پیکربندی کنید
ویرایشگر متن نمونه های فعلی عبارتند از:

.. code-block::

    * */3 * * * * * ssh_scan.sh "1.2.3.0-255" "Building A"
    * */3 * * * * * ssh_scan.sh "1.2.0-255.3" "Building B"

هر خط محدوده IP تعریف شده را برای ماینرهای پاسخگو اسکن می کند و لیست را ذخیره می کند تا در دسترس Prometheus باشد. رشته "Building A" / "Building B" می تواند یک نام دلخواه باشد. در حال حاضر، به صورت پویا به ساختمان برچسب نگاشت می شود. اسکن هر سه دقیقه انجام می شود - می توانید آن را بر اساس اندازه مزرعه و نیازهای خود تغییر دهید. در صورتی که با سینتکس cron آشنایی ندارید، در `اینجا <https://www.netiq.com/documentation/cloud-manager-2-5/ncm-reference/data/bexyssf.html>`__ توضیح داده شده است.

**آدرس های IP را فهرست کنید**

برای استفاده از یک لیست ثابت از آدرس های IP، باید فایل ``docker-compose.yml`` را تغییر دهید،

ابتدا، crontab را کامنت کنید تا اسکن پویا غیرفعال شود:

.. code-block::

   # bos_scanner:
   # image: braiinssystems/bos_monitor:v1.0.0
   # container_name: bos_scanner
   # volumes:
   #  - ./scan_crontab:/usr/local/share/scan_crontab
   #  - scanner_data:/mnt:rw
   # network_mode: "host"

دوم، اسکن پویا را کامنت کنید و استفاده از یک فایل پیکربندی متفاوت را فعال کنید. پس از تغییرات باید به این صورت باشد:

.. code-block::

   #- '--config.file=/etc/prometheus/prometheus_scan.yml'
   - '--config.file=/etc/prometheus/prometheus_static.yml'

آدرس های IP به صورت یک آرایه در فایل پیکربندی فهرست شده اند
`prometheus_static.yml`. ورودی ها را با لیستی از ماینرهای خود تغییر دهید:

.. code-block:

   - targets: ['10.35.31.2:8081','10.35.32.2:8081']

Note that:

- پورت باید در انتهای آدرس IP اضافه شود. پورت 8081 جایی است که معیارهای Prometheus در دسترس هستند
- آدرس های IP داخل '' و با کاما از هم جدا می شوند

در صورتی که آدرس IP ثابت ندارید، آدرس IP هر ماینر می تواند تغییر کند. اگر همچنان می‌خواهید از این رویکرد استاتیک استفاده کنید، سعی کنید زمان اجاره را برای سرور DHCP خود به مقدار بالایی (مثلاً 48 ساعت) افزایش دهید، به طوری که حتی زمانی که ماینر برای مدتی آفلاین است، آدرس IP دوباره تخصیص داده شود.

برای اینکه همه ماینرها را به لیست بیاورید، می‌توانید مزرعه خود را برای دستگاه‌هایی با استفاده از جعبه ابزار BOS اسکن کنید و از نتایج پیکربندی ایجاد کنید. برای دریافت لیست می توانید از UX یا خط فرمان استفاده کنید.

مثال خط فرمان (لینوکس):

.. code-block::

   ./bos-toolbox scan -o ips.txt 10.10.0.0/16
   cat ips.txt \| sed "s/.*/'&:8081'/" \| paste -sd',' \| sed "s/.*/[&]/"

دستور اول تمام آدرس های IP در محدوده 10.10.0.0 و 10.10.255.255 را اسکن می کند. دومی آرایه‌ای با آدرس‌های IP چاپ می‌کند که می‌توانید آن را در پیکربندی جای‌گذاری کنید.

فقط ماینرهایی که دارای فریم‌ور Braiins OS+ هستند قابل نظارت هستند. اگر از ماینرهای بدون Braiins OS+ استفاده می کنید، بهتر است از موارد زیر استفاده کنید:

.. code-block::
   
   ./bos-toolbox scan 10.10.0.0/16 &> ips.txt
   grep "\| bOS" ips.txt \| cut -d"(" -f2 \| cut -"d)" -f1 \| sed "s/.*/'&:8081'/" \| paste -sd',' \| sed "s/.*/[&]/"

برای محدوده های IP مختلف می توانید از:

-  10.10.10.0/24 for range 10.10.10.0 - 10.10.10.255
-  10.10.0.0/16 for range 10.10.0.0 to 10.10.255.255
-  10.0.0.0/8 for range 10.0.0.0 to 10.255.255.25

شروع مانیتورینگ
----------------

.. code-block::

   docker-compose up -d

می‌توانید تأیید کنید که container با استفاده از آن در حال اجرا است `docker ps`.

حالا به مسیر مقابل مراجعه کنید: `http://<your_host>:3000`.

عملیات
----------

**تغییر پیکربندی**

فایل پیکربندی را با توجه به نیاز خود تغییر دهید

.. code-block::

   docker-compose restart prometheus

**به روز رسانی به نسخه جدیدترn**

.. code-block::

   git pull origin master
   docker-compose up -d

داشبوردها
==========

در مخزن خود، ما داشبوردهای نمونه ارائه می دهیم که می تواند شما را به آماده سازی نظارت برای مزرعه خود که به بهترین وجه با نیازهای شما مطابقت دارد، شروع کند.

داشبورد فارم
--------------

این داشبورد سطح بالایی است که همه ماینرهای فارم شما را زیر نظر دارد. در صورتی که چندین نمونه Prometheus در حال اجرا هستید، دارای یک انتخابگر منبع داده داخلی است. همچنین دارای چندین گزارش تمرینی است که در تصویر زیر مشخص شده اند:

  .. |pic3| image:: ../_static/monitoring_dashboard.png
      :width: 100%
      :alt: Dashboard

  |pic3|

قسمت‌هایی که با رنگ قرمز مشخص شده‌اند شما را به یک گزارش تمرینی هدایت می‌کنند که نمونه‌ها را فهرست می‌کند. قسمت هایی که با رنگ آبی مشخص شده اند مستقیماً به ماینر UX می روند.

نمونه داشبورد فارم - بر اساس ساختمان
-------------------------------------

داشبورد دارای ویژگی است که در آن ردیف هایی از پانل های Grafana به طور خودکار برای هر ساختمان تعریف شده نمایش داده می شود. این به صورت پویا بر اساس مقادیر برچسب ساختمان ایجاد می شود. جریان کامل در پیکربندی مثال به شرح زیر است:

- دو شغل مجزا در prometheus.yml ایجاد می شود
- هر شغل دارای یک ساختمان برچسب است که با یک مقدار نشان دهنده ساختمان اضافه شده است
- داشبورد Grafana دارای پارامتر ساختمان تعریف شده است که به برچسب ساختمان مرتبط است
- هدر ردیف دارای $building به عنوان نام است - این با مقادیر برچسب گسترش می یابد
- هر پنل دارای $building به عنوان فیلتر است

معیارها و برچسب ها
==================
هر سری زمانی منحصراً با نام متریک و جفت‌های اختیاری کلید-مقدار به نام برچسب‌ها مشخص می‌شود. نام متریک ویژگی کلی یک سیستم را که اندازه گیری می شود مشخص می کند. برچسب‌ها مدل داده‌های ابعادی پرومتئوس را فعال می‌کنند: هر ترکیب معینی از برچسب‌ها برای همان نام متریک، نمونه ابعادی خاصی از آن متریک را مشخص می‌کند. زبان پرس و جو امکان فیلتر کردن و تجمیع را بر اساس این ابعاد می دهد.

بررسی اجمالی:

-  ``application_version_details (instance, version_full, toolchain)``
-  ``client_status (instance, connection_type, host, protocol, user, worker)``
-  ``hashboard_nominal_hashrate_gigahashes_per_second (instance, hashboard)``
-  ``hashboard_shares (instance, hashboard, type: valid | invalid | duplicate)``
-  ``miner_metadata (instance, model, os_version)``
-  ``miner_power (instance, type: wall | estimate | limit, socket)``
-  ``temperature (instance, chip_addr, chip_in_domain, voltage_domain,hashboard, location: chip | pcb)``
-  ``stratum_accepted_shares_counter (instance, host, user, worker, protocol, connection_type)``
-  ``stratum_rejected_shares_counter (instance, host, user, worker, protocol, connection_type)``
-  ``stratum_accepted_submits_counter (instance, host, user, worker, protocol, connection_type)``
-  ``stratum_rejected_submits_counter (instance, host, user, worker, protocol, connection_type)``
-  ``tuner_stage (instance, hashboard)``

جزئیات نسخه برنامه
---------------------------

نسخه برنامه ای که در حال تولید سری های زمانی است.

``application_version_details``

**برچسب‌ها**

-  instance: آدرس IP ماینر
-  version_full: نسخه برنامه
-  toolchain
   
وضعیت کلاینت
-------------

وضعیت کلاینت: (متوقف = 0, در حال اجرا = 1 , متوقف = -1)

``client_status``

**برچسب‌ها**

-  instance: آدرس IP ماینر
-  connection_type: نوع اتصال، که می تواند یکی باشد *user* یا *dev-fee*
-  host: URL میزبان، معمولاً URL استخر یا پروکسی
-  protocol: پروتکل استخراج
-  user: معمولاً نام کاربری استخر ماینینگ مشتری
-  worker: نام ورکر


هش‌ریت اسمی هشبورد (Gh/s)
---------------------------------

هش‌ریت اسمی برای هر هشبورد بر حسب Gh/s.

``hashboard_nominal_hashrate_gigahashes_per_second``

**برچسب‌ها**

-  instance: آدرس IP ماینر
-  hashboard: رنک هشبورد

سهام های هشبورد
------------------

تعداد سهام معتبر تولید شده توسط هشبورد. از اشتراک های هشبورد می توان برای محاسبه هش ریت واقعی برای هشبورد، ماینر یا گروه های دیگر استفاده کرد. این معیار اطلاعاتی در مورد اینکه آیا سهام توسط هدف پذیرفته شده است ارائه نمی دهد - stratum_accepted_shares_counter باید برای این مورد استفاده شود.

``hashboard_shares (counter)``

**برچسب‌ها**

-  instance: آدرس IP ماینر
-  hashboard: رنک هشبورد
-  type: نوع سهام از نظر اعتبار، *valid* - سهام معتبر، * invalid* - سهام نامعتبر، * duplicate* - سهام تکراری

**مثال‌ها**

میانگین تعداد هش در هر ثانیه در 20 ثانیه گذشته برای همه موارد:

.. code-block::

   sum(rate(hashboard_shares[20s])) * 2^32

میانگین تعداد هش در هر ثانیه در 20 ثانیه گذشته برای یک مورد:

.. code-block::

   sum by(instance) (rate(hashboard_shares[20s])) * 2^32

میانگین تعداد هش در هر ثانیه در 20 ثانیه گذشته برای همه نمونه‌ها بر اساس نوع ماینر:

.. code-block::

   sum by (model) (
      (sum by (instance)((rate(hashboard_shares[20s]))) * 2^32)
      * on(instance) group_left(model) count by (instance, model) (miner_metadata)
   )

فراداده ماینر
--------------

``miner_metadata``

**برچسب‌ها**

-  instance: آدرس IP ماینر
- model: مدل ماینر
- os_version: نسخه فریم‌ور

**مثال‌ها**

تعداد ماینرها بر اساس مدل:

.. code-block::

   count_values by (model) ("x", miner_metadata)

توان ماینر
-----------

``miner_power``

**برچسب‌ها**

-  instance: IP address of the miner
-  type: 3 types, *estimated* - estimated power, *limit* - power limit, *psu* - measured power, *wall*
-  socket

**مثال‌ها**

کل مصرف برق تخمینی برای همه موارد:

.. code-block::

   sum(miner_power{type="estimated"})

محدودیت توان کل برای همه موارد:

.. code-block::

  sum(miner_power{type="limit"})

شمارشگر سهام پذیرفته شده Stratum
---------------------------------

تعداد کل سهام پذیرفته شده توسط هدف. به عنوان مثال، معمولاً اهداف بیشتری وجود دارد که توسط برچسب میزبان نشان داده می شود.

``stratum_accepted_shares_counter (counter)``

**برچسب‌ها**

-  instance: آدرس IP ماینر
-  connection_type: نوع اتصال، که می تواند یکی باشد *user* یا *dev-fee*
-  host: URL میزبان، معمولاً URL استخر یا پروکسی
-  protocol: پروتکل استخراج
-  user: معمولاً نام کاربری استخر ماینینگ مشتری
-  worker: نام ورکر

**مثال‌ها**

میانگین تعداد سهام پذیرفته شده در هر ثانیه در 20 ثانیه گذشته برای همه موارد بر اساس هدف:

.. code-block::

   sum by(host) (rate(stratum_accepted_shares_counter[20s]))

شمارشگر سهام رد شده Stratum
-------------------------------

Total number of shares rejected by target.

``stratum_rejected_shares_counter (counter)``

**برچسب‌ها**

-  instance: آدرس IP ماینر
-  connection_type: نوع اتصال، که می تواند یکی باشد *user* یا *dev-fee*
-  host: URL میزبان، معمولاً URL استخر یا پروکسی
-  protocol: پروتکل استخراج
-  user: معمولاً نام کاربری استخر ماینینگ مشتری
-  worker: نام ورکر

**Examples**

Average number of rejected shares per second over last 20 seconds for all instances by target:

.. code-block::

   sum by(host) (rate(stratum_rejected_shares_counter[20s]))

Stratum Accepted Submits Counter
--------------------------------

تعداد کل ارسال های پذیرفته شده توسط هدف. به عنوان مثال، معمولاً اهداف بیشتری وجود دارد که توسط برچسب میزبان نشان داده می شود.

``stratum_accepted_submits_counter (counter)``

**برچسب‌ها**

-  instance: آدرس IP ماینر
-  connection_type: نوع اتصال، که می تواند یکی باشد *user* یا *dev-fee*
-  host: URL میزبان، معمولاً URL استخر یا پروکسی
-  protocol: پروتکل استخراج
-  user: معمولاً نام کاربری استخر ماینینگ مشتری
-  worker: نام ورکر

**مثال‌ها**

میانگین تعداد ارسال های پذیرفته شده در هر ثانیه در 20 ثانیه گذشته برای
همه موارد بر اساس هدف:

.. code-block::

   sum by(host) (rate(stratum_accepted_submits_counter[20s]))

شمارشگر ارسال‌های رد شده Stratum
--------------------------------

تعداد کل ارسال‌های رد شده توسط هدف.

``stratum_rejected_submits_counter (counter)``

**برچسب‌ها**

-  instance: آدرس IP ماینر
-  connection_type: نوع اتصال، که می تواند یکی باشد *user* یا *dev-fee*
-  host: URL میزبان، معمولاً URL استخر یا پروکسی
-  protocol: پروتکل استخراج
-  user: معمولاً نام کاربری استخر ماینینگ مشتری
-  worker: نام ورکر

**مثال‌ها**

میانگین تعداد ارسال‌های رد شده در هر ثانیه در 20 ثانیه گذشته برای همه موارد بر اساس هدف:

.. code-block::

   sum by(host) (rate(stratum_rejected_submits_counter[20s]))


دما
------

هر سنسور دمای موجود داده ها را ارائه می دهد. ممکن است سنسورهایی در مکان های مختلف (PCB یا تراشه) وجود داشته باشد.

``temperature``

**برچسب‌ها**

-  instance: IP address of the miner
-  chip_addr
-  chip_in_domain
-  voltage_domain
-  hashboard
-  location: chip|pcb

**مثال‌ها**

میانگین حداکثر دما در همه موارد (ماینرها):

.. code-block::

   avg(max by (instance) (temperature))

میانگین حداکثر دما در همه نمونه ها (ماینرها) بر اساس نوع ماینر:

.. code-block::

   avg by (model) (
     (max by (instance) (temperature)) * on (instance)
     group_left(model) count by (instance, model) (miner_metadata)
   )

مراحل تیونر
-----------

مرحله های تیونر:

-  2: testing performance profile
-  3: tuning individual chips
-  4: stable
-  6: manual configuration running

``tuner_stage``

**برچسب‌ها**

-  instance: آی پی آدرس ماینر
-  hashboard: رنک هشبورد

**مثال‌ها**

تعداد instatceها بر اساس مرحله:

.. code-block::

   count_values ("Stage", max by (instance) (tuner_stage))

مثال‌های دیگر
--------------

**Extracting parts of IP address**

اگر فارم خود را با تخصیص محدوده IP های مختلف به بخش های مختلف مزرعه خود مدیریت می کنید، گروه بندی معیارها بر اساس یک هشتاد آدرس IP ممکن است مفید باشد. مثالی برای حداکثر دمای تراشه تا اکتت سوم:

.. code-block::

   max by (segment) (label_replace(
     temperature{location="chip"}, "segment", "$1", "instance","\\d+\\.\\d+\\.(\\d+)\\.\\d+.*"
   ))

اگر برای بسیاری از معیارها نیاز به انجام این کار دارید، بهتر است قسمت هایی از آدرس IP را به عنوان برچسب های سفارشی داشته باشید. برای مثال به بخش پیکربندی مراجعه کنید.

 دریافت کمک
============

برای اطلاعات بیشتر در مورد Prometheus و Grafana، لطفاً به مستندات رسمی مراجعه کنید:

-  `Prometheus مستندات <https://prometheus.io/docs/introduction/overview/>`__
-  `Grafana مستندات <https://grafana.com/docs/>`__

اگر سؤالی دارید که مربوط به مانیتورینگ بر ماینرهای Braiins OS+ با Prometheus و Grafana است، لطفاً با تیم پشتیبانی ما در تلگرام تماس بگیرید..