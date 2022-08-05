
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###########
محدودیت
###########

.. contents::
  :local:
  :depth: 2

اولین نسخه Braiins Farm Proxy دارای چند محدودیت شناخته شده است که در نسخه های بعدی برطرف خواهند شد.

 1. **Linux 64bit** و **ARM 64bit , 32bit** پشتیبانی می شوند،
 2. حداقل سخت افزار مورد نیاز **Raspberry Pi 3** است،
 3. Braiins Farm Proxy فقط *1 routing domain** را پشتیبانی می کند. این می تواند فارمی را با نیازهای گسترده به چندین قانون مسیریابی محدود کند. این محدودیت را می توان با استفاده از چند نمونه Braiins Farm Proxy کاهش داد. برای مثال، اگر چندین مکان دارید یا نیاز به ارسال هش ریت های مختلف به استخرهای مختلف دارید، باید از Braiins Farm Proxy برای هر یک از مکان ها/مقصدها استفاده کنید. البته، شما می توانید هر دو روش را ترکیب کنید،
 4.  Linux has usually **1,024** limit on the number of open files. You can check your limit with command ``ulimit -Hn``. If you plan to connect about 1,000+ miners it is needed to increase this limit, it can be done with command ``ulimit -n 10000`` (increase to 10,000 open files as an example),
 5. هیچ **رابط گرافیکی** برای پیکربندی مسیریابی هش ریت وجود ندارد. پیکربندی باید در فایل پیکربندی TOML انجام شود،
 6. داشبوردهای Farm Monitor که برای نظارت کامل بر کل فارم اختصاص داده شده اند، از ماینرهای فعلی با سیستم عامل Braiins OS+ پشتیبانی می کنند.
 7. **ماینرهای** آزمایش شده Antminers S9، X17، X19، Whatsminers M2x/M3x و Avalon ماینرهایی با فریم‌ور کارخانه و +Braiins OS بودند.
 8. یک **تعادل ** بین تجمیع هش ریت و نظارت بر ماینرهای به صورت انفرادی در داشبورد استخر وجود دارد. این فقط برای مواردی مرتبط است که از ویژگی *identity_pass_through = true* در فایل پیکربندی TOML (در تعریف [target]) استفاده شده باشد. سطح بالاتر تجمیع به این معنی است که ماینرها به دلیل دشواری بالا در upstream ارسال‌ها را کندتر ارسال می‌کنند (پراکسی با ضریب تجمع بالا تسکی با دشواری بالا از استخر هدف دریافت می‌کند). در چنین مواردی، ماینرها و ورکرهای آنها می توانند در داشبورد استخر به عنوان غیرفعال ظاهر شوند. این یک محدودیت برای Braiins Farm Proxy نیست، بلکه ویژگی منطقی آن است.
