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

Braiins OS یک سیستم عامل کاملا متن باز برای ماینرهای ASIC است. این سیستم اولین فیرم ور برای اجرای عمومی AsicBoost در سال ۲۰۱۸ بود و اکنون اجرای پروتکل جدید ماینینگ به نام Stratum V2 را ارائه میدهد.
علاوه بر این، Braiins OS هم سو و هم جهت با نرم افزار جدید ما BOSminer کار میکند که این نرم افزار از صفر و از پایه به زبان Rust نوشته شده است تا جایگزین سی جی ماینر قدیمی باشد.

دستگاههایی که در حال حاضر پشتیبانی میشوند عبارتند از: Antminer S9, S9i, و S9j. پشتیبانی دستگاه Antminer S17 در حال برنامه ریزی برای اجرا در آینده نزدیک است.

********
امکانات
********

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
  * `گروه اسپانیایی  <https://t.me/BraiinsOS_ES>`_
  * `گروه چینی <https://t.me/BraiinsOS_ZH>`_

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
  
    * [bug] some devices were experiencing random I2C controller bus lockups and would fail to communicate with hashboard power controllers connected to the shared I2C bus. We have found out that the cause was the Xilinx I2C controller core that we have integrated into the FPGA bitstream. We have switched to the I2C present in the SoC and the bitstream only routes the signal of the peripheral (IIC0) to corresponding FPGA pins.


20.03
---------------------------

برای مشاهده لینک `WHATSNEW.MD <https://github.com/braiins/braiins/blob/master/braiins-os/whatsnew.md>`_ در گیت‌هاب را مطالعه کنید.

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
