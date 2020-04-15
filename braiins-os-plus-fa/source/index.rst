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

Braiins OS+ is an operating system for ASIC miners. It is based on the `Braiins OS <https://braiins-os.com/community-edition>`_ product and provides additional proprietary algorithms for autotuning of miners. When a user provides maximum allowed power consumption in Watts, the system will automatically optimize the mining process to maximize hash rate. This process works across a wide spectrum of inputs, allowing you to optimize for the best possible efficiency or maximum hash rate based on economical considerations. Internal testing shows that for the Antminer S9, it’s possible to achieve efficiency of 70J/THs or even better for low Watts setting. For high power consumption, hash rate can increase by 20%+ (comparing to Antminer S9, 13.5 TH/s stock setting with ~ 94J/TH).

Currently supported devices are Bitmain’s Antminer S9, S9i, and S9j. Antminer S17 support is planned for the near future.

********
Features
********

 * State-of-the-art autotuning optimization to maximize hash rate or efficiency
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

20.03
---------------------------

  * All mining hardware types

    * [feature] configuration file allows specifying a power limit of the PSU that the autotuning algorithm
      will take into account in order to maximize the TH/W produced by the mining device

  * Antminer S9

    * [feature] Autotuning based on a user-specified power limit

********************
مشکلات شناسایی شده
********************

آنچه در ادامه میبینید لیستی از ایراداتی است که در نسخه منتشر شده وجود دارند.

20.03 (بروز شده در 3/30/2020)
-------------------------

  * رابط کاربری گرافیکی

   * خط رجوع در جدول هش ریت، ارزش  نادرستی برای هش ریت میانگین دارد. مشکل زمانی وجود دارد که کمتر از سه زنجیره هش ریت قابل استفاده هستند.
   * نسبت رد شدن ضرب در ۱۰۰ است. برای مثال زمانی که نسبت رد شدن، ۰/۱٪ است، ۱۰٪ نمایش داده خواهد شد.

  * پیکربندی

    * نصب SD Card عدم وجود کلید احراز هویت  دستگاه Strautum V2 را در تنظیمات ماینر گزارش میدهد. (Error: missing upstream authority key for securing stratum2+tcp connection in pool") کاربر میتواند اتصال ( شامل کلید) را در قسمت تنظیمات یا به طور مستقیم در فایل ``/etc/bosminer.toml`` مشخص کند
