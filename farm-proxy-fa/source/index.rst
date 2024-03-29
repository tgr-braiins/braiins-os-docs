.. toctree::
   :hidden:
   :maxdepth: 3
   :caption: فهرست مطالب:
   :glob:

   self
   Setup/index*
   User Guide/index*
   Monitoring Braiins OS+ with Prometheus and Grafana/index*
   Troubleshooting/index*
   Support/index*

---------------

.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>


#######
مقدمه
#######

Braiins Farm Proxy یک برنامه رایگان و مستقل است که به صورت محلی در فارم اجرا می شود. هدف پراکسی حفظ پهنای باند و در عین حال جمع آوری هش‌ریت SHA256 از تک تک ورکرها(دستگاه‌های ماینر) و مسیریابی آن به مقاصد هدف است که معمولاً استخرهای استخراج هستند. ورکرها برای اتصال به پراکسی پیکربندی شده اند. Braiins Farm Proxy را می توان برای اتصال به چندین استخر (های) هدف با استخر(های) پشتیبان به عنوان failover تنظیم کرد. پراکسی آدرس های IP استخر را بهینه می کند و نقاط پایانی با کمترین تأخیر یا از دست دادن بسته را انتخاب می کند. همچنین می‌تواند اتصال استخر را برای حفظ حریم خصوصی و امنیت بهتر در صورت استفاده از Braiins Pool به عنوان نقطه پایانی رمزگذاری کند.

********
امکانات
********

* **حفظ پهنای باند** به دلیل تجمیع شدن هش ریت و در نتیجه به خاطر `سختی سهام <https://braiins.com/blog/bitcoin-mining-pools-luck-shares-estimated-hashrate>`_ بالاتر در upstream، که منجر به کاهش مصرف داده برای ارسال همان مقدار سهام می گردد.

 * **بهینه سازی شبکه** بر اساس این امکان Braiins Farm Proxy دائماً کیفیت اتصال را کنترل می کند و به طور خودکار هش ریت را به نقطه پایانی با عملکرد مطلوبتر هدایت می کند.

 * Braiins Farm Proxy را می توان با **هر استخر** برای استخراج بیت کوین استفاده کرد.

 * **سوئیچ خودکار** به استخر پشتیبان اگر استخر اصلی دیگر پاسخ ندهد.

 * **نظارت منظم** در داشبورد Grafana همراه با Braiins Farm Proxy، با امکان ساخت مانیتورینگ سفارشی خود از طریق **API monitoring**.
 * کاربران +Braiins OS برای صرفه جویی در پهنای باند بیشتر می توانند از **تجمیع هش ریت کارمزد توسعه** بهره مند شوند. Braiins Farm Proxy می تواند هم هش ریت فارم و هم هش ریت کارمزد توسعه را تجمیع کند.
 * اگر نقطه پایانی هدف Braiins Pool باشد، یک **اتصال رمزگذاری شده** برای اطمینان از حریم خصوصی داده ها و محافظت در برابر ربودن هش ریت پشتیبانی می شود.
 * Braiins Farm Proxy یک نرم افزار کاملا **رایگان** است.
