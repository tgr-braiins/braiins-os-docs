.. toctree::
   :hidden:
   :maxdepth: 3
   :caption: فهرست مطالب:
   :glob:

   self
   Setup/index*
   Configuration/index*
   Basic User's Guide/index*

---------------

#####
مقدمه
#####

Braiins OS+ سیستم عاملی برای ماینرهای ASIC است که بر اساس محصول Braiins OS بوده و الگوریتم های اختصاصی برای اتوتیونینگ ماینرها فراهم میکند. وقتی کاربری بیشترین مقدار مصرف برق را در مقیاس وات استفاده میکند، سیستم به طور خودکار پروسه ماینینگ را برای بالا بردن هش‌ریت بهینه سازی میکند. این فرایند در طیف گسترده ای از ورودی‌ها کار میکند و به شما اجازه میدهد تا برای بهترین بهره وری ممکن یا بیشترین نرخ هش ریت بر اساس ملاحظات اقتصادی بهینه سازی کنید. آزمایش داخلی نشان میدهد که برای Antminer S9 میتوان به اثربخشی 70J/THs یا حتی بهتر برای تنظیمات وات پایین دست یافت. برای مصرف برق بالا، هش ریت میتواند تا ۲۰٪ + افزایش یابد. (در مقایسه  Antminer S9, 13.5 TH/s با تنظیمات کارخانه در حدود 94J/TH است)


درحال حاضر دستگاه‌های شرکت Bitmain Antminer مدلهای S9, S9i, S9j پشتیبانی می‌شوند. در آینده نزدیک دستگاه Antminer S17 نیز پشتیبانی خواهد شد.

********
امکانات
********

 * بهینه سازی اتوتونینگ پیشرفته برای به حداکثر رساندن نرخ هش یا کارآیی
 * سیستم عامل متن باز
 * اجرای Stratum V2 با بازده اطلاعاتی بهبود یافته و جلوگیری از دزدی هش ریت
 * جایگزینی سی جی ماینر (ماینر BOS) که به زبان Rust از پایه نوشته شده
 * آماده به کار شدن سریع سیستم ( ۵ تا ۷ ثانیه) 
 * بدون خرابی ناگهانی به دلایل نامعلوم
 * نصب جمعی و گروهی
 * به روز رسانی خودکار با سیستم opkg 
 * کنترل فن کاملا قابل شخصی سازی ( توانایی خنک کنندگی با مایع یا خنک کنندگی غوطه وری)
 * مانیتورینگ پیشرفته برای جلوگیری از داغ شدن بیش از حد دستگاه و سایر مشکلات

************************
پشتیبانی و ارتباط با ما
************************

سوالی دارید؟
تیم های پشتیبانی و پشتیبانی ما همیشه برای کمک در دسترس هستند.

به گروه تلگرام ما بپیوندید:

  * `گروه انگلیسی <https://t.me/BraiinsOS>`_
  * `گروه فارسی <https://t.me/BraiinsOS_SlushPool_FA>`_
  * `گروه روسی <https://t.me/BraiinsOS_RU>`_
  * `گروه چینی <https://t.me/BraiinsOS_ZH>`_

  همچنین می‌توانید `درخواست پشتیبانی VIP <https://slushpool.kayako.com/en-us/conversation/new/11>`_  به تیم پشتیبانی ارسال کنید.


************
تغییرات نسخه
************

20.04
---------------------------

This release covers mostly user facing issues, installation/deinstalation difficulties and 1 major problem with I2C controller on S9's. Also, we now have nightly builds that are easy to enable via **bos** tool.

  * All mining hardware types

    * [feature] support for reconnect - we have implemented support for `client.reconnect` (stratum V1) and reconnect message for V2
    * [feature] installation/deinstallation (aka **upgrade2bos** and **restore2factory**) process (transition from factory firmware to Braiins OS or vica versa) has been improved:
    * [feature] custom pool user (`--pool-user`) can be set on command line
    * [feature] pool settings from the factory firmware are now automatically being migrated to BOSminer configuration. Migration can be disabled by specifying (`--no-keep-pools`)
    * [feature] we now provide binary form of **upgrade2bos** (based on pyinstaller) that contains the latest Braiins OS installation image
    * [feature] similarly, **restore2factory** (based on pyinstaller) is now available in binary form and doesn't require any longer downloading/finding out the correct factory firmware.
    * [feature] disk space and time consuming backup of the original firmware is now disabled by default (can be enabled by `--backup`)
    * [feature] keeping host name while performing first time install is now driving by 2 options `--keep-hostname` and `--no-keep-hostname` allowing to force override and automatic hostname generation based on MAC address
    * [feature] support for enabling/disabling nightly builds has been integrated into **bos** utility (and its legacy **miner** counterpart).
    * [feature] system now provides **logs** covering **longer timespan** of **BOSminer** operation due to enabling **log rotation** and compression of '/var/log/syslog.old' when it is bigger than 32 KiB
    * [bug] SD card image now contains slushpool authority public key that was missing
    * [bug] rejection rate is now correctly being displayed
    * [bug] unknown stratum V1 messages received from the server are now being logged for diagnostics

  * Antminer S9
  
    * [feature] Tuner status is now shown in the GUI. TUNERSTATUS API command was added.
    * [bug] some devices were experiencing random I2C controller bus lockups and would fail to communicate with hashboard power controllers connected to the shared I2C bus. We have found out that the cause was the Xilinx I2C controller core that we have integrated into the FPGA bitstream. We have switched to the I2C present in the SoC and the bitstream only routes the signal of the peripheral (IIC0) to corresponding FPGA pins.


20.03
---------------------------

  * تغییرات اعمال شده روی همه دستگاه‌های پشتیبانی شده

    * [feature] فایل پیکربندی اجازه میدهد تا محدودیت توان PSU را مشخص کنیم تا این محدودیت توسط الگوریتم اتوتیونیگ برای به حداکثر رساندن TH/W تولید شده توسط دستگاه ماینینگ در نظر گرفته شود.

  * Antminer S9 دستگاه

    * [feature] اتوتیونیگ بر اساس محدودیت توان برق مشخص شده توسط کاربر

********************
مشکلات شناسایی شده
********************

آنچه در ادامه میبینید لیستی از ایراداتی است که در نسخه منتشر شده وجود دارند.

20.03 (بروز شده در 3/30/2020)
-------------------------------

  * رابط کاربری گرافیکی

   * خط رجوع در جدول هش ریت، ارزش  نادرستی برای هش ریت میانگین دارد. مشکل زمانی وجود دارد که کمتر از سه زنجیره هش ریت قابل استفاده هستند.
   * نسبت رد شدن ضرب در ۱۰۰ است. برای مثال زمانی که نسبت رد شدن، ۰/۱٪ است، ۱۰٪ نمایش داده خواهد شد.

  * پیکربندی

    * نصب SD Card عدم وجود کلید احراز هویت  دستگاه Strautum V2 را در تنظیمات ماینر گزارش میدهد. (Error: missing upstream authority key for securing stratum2+tcp connection in pool") کاربر میتواند اتصال ( شامل کلید) را در قسمت تنظیمات یا به طور مستقیم در فایل ``/etc/bosminer.toml`` مشخص کند
