.. toctree::
   :hidden:
   :maxdepth: 3
   :caption: Table of Contents:
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

  * `EN group <https://t.me/BraiinsOS>`_
  * `FA group <https://t.me/BraiinsOS_SlushPool_FA>`_
  * `RU group <https://t.me/BraiinsOS_RU>`_
  * `ZH group <https://t.me/BraiinsOS_ZH>`_

************
تغییرات نسخه
************

20.03
---------------------------

See WHATSNEW.MD (Will be published 3/31 on github)

************
Known Issues
************

The following lists issues that are known to be present in released version.

20.03 (Updated 3/30/2020)
-------------------------

  * رابط کاربری گرافیکی

   * خط رجوع در جدول هش ریت، ارزش  نادرستی برای هش ریت میانگین دارد. مشکل زمانی وجود دارد که کمتر از سه زنجیره هش ریت قابل استفاده هستند.
   * نسبت رد شدن ضرب در ۱۰۰ است. برای مثال زمانی که نسبت رد شدن، ۰/۱٪ است، ۱۰٪ نمایش داده خواهد شد.

  * پیکربندی

    * SD Card installation will report missing Stratum V2 authentication key in the Miner/Configuration
      section (Error: missing upstream authority key for securing stratum2+tcp connection in pool").
      User can configure connection (including the key) in the configuration, or directly in
      the ``/etc/bosminer.toml`` file.
