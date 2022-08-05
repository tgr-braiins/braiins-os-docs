
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

##########
راه اندازی
##########

.. toctree::
   :maxdepth: 3
   :glob:

   *

*******
نصب
*******

نرم افزار Braiins Farm Proxy دارای `مخرن <https://github.com/braiins/farm-proxy>`_ عمومی در Github است و در واقع شامل ۴ کانتینر Docker می باشد. شما می توانید به راحتی با استفاده از خط فرمان دستگاه لینوکسی که به عنوان میزبان پراکسی خواهد بود نصب را انجام دهید.

برای شروع نیاز هست تا یکسری پیش نیاز ها را نصب کنید:

**Linux**

 * مشاهده `مراحل نصب داکر <https://docs.docker.com/engine/install/ubuntu/>`_
 * مشاهده `مراحل نصب داکر پست، اختیاری <https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user>`_
 * مشاهده `docker-compose مراحل نصب <https://docs.docker.com/compose/install/>`_

**RPi**

  * یکی از راهنماهای متعدد را دنبال کنید - برای مثال: https://jfrog.com/connect/post/install-docker-compose-on-raspberry-pi/

**بررسی و تایید نصب پیش نیازها**

::

      docker version
      docker-compose --version
      git --version

**دانلود مخزن Braiins Farm Proxy**

::

      sudo apt update
      sudo apt install git
      git clone https://github.com/braiins/farm-proxy.git

پشته Docker شامل کانتینرهای زیر می باشد:
 * *farm-proxy*: کانتینری شامل فایل اجرایی Braiins Farm Proxy,
 * *bos_scanner*: کانتینری همراه با اسکنر SSH که ماینترهایی که دارای فریم‌ور Braiins OS+ هستند را در شبکه شناسایی و داده های آنها را برای داشبورد Grafana با نام **Farm Dashboard** فراهم می کند,
 * *grafana*: کانتینری شامل برنامه Grafana
 * *prometheus*: کانتینری شامل دیتابیس Prometheus.

یک فایل اجرایی مستقل از Braiins Farm Proxy را می توان از Github `دانلود کرد <https://github.com/braiins/farm-proxy/releases>`_.

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/AXfyDhbx4WY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

*****
Start
*****

هنگامی که عملیات استخراج با استفاده از Braiins Farm Proxy توسط اپراتور فارم پیکربندی می شود، پراکسی می تواند شروع به کار کند (پیکربندی به طور مفصل در متن زیر توضیح داده شده است). دستور ``docker-compose up -d`` را در ترمینال لینوکس اجرا کنید. برای اینکه ببینید آیا همه کانتینرهای Docker در حال اجرا هستند، دستور ``docker ps`` را اجرا کنید تا آنها لیست شوند و وضعیت آنها بررسی شود.

*******
Restart
*******

در صورتی که Braiins Farm Proxy نیاز به راه اندازی مجدد داشته باشد، دستور ``docker restart farm-proxy`` را اجرا کنید. این فرمان فقط کانتینر فارم پروکسی را مجددا راه اندازی می کند. در صورت هر گونه تغییر در فایل پیکربندی TOML، به راه اندازی مجدد farm-proxy نیاز است. اگر می‌خواهید کانتینر Docker دیگری را مجدداً راه‌اندازی کنید، می‌توانید به طور مثال با جایگزین کردن «farm-proxy» با نام کانتینری که نیاز به راه‌اندازی مجدد دارد، انجام دهید.

****
Stop
****

گاهی اوقات اپراتور ماینینگ ممکن است بخواهد Braiins Farm Proxy را متوقف کند. این کار را می توان در ترمینال لینوکس با دستور ``docker stop farm-proxy`` انجام داد. این در صورت هر گونه تغییر در فایل *docker-compose.yml* لازم است. برای اجرای دوباره پراکسی، دستور ``docker-compose up -d farm-proxy`` را اجرا کنید. اگر می‌خواهید کانتینر Docker دیگری را متوقف کنید، می‌توانید به همین ترتیب انجام دهید.

*******
Upgrade
*******

تیم Braiinns توصیه می کند مخزن Braiins Farm Proxy Github را دنبال کنید و در صورت موجود بودن نسخه جدید مطلع شوید. برای ارتقاء به نسخه جدیدتر کافیست دستور ``git pull origin master`` را در ترمینال لینوکس اجرا کنید. اگر تغییری در فایل‌های پیکربندی ایجاد کردید، ممکن است بخواهید فایل‌های اصلاح‌شده را ذخیره کنید یا آنها را با دستور ``git stash`` قبل از ارتقا ذخیره کنید. اگر تغییری در فایل docker-compose.yml ایجاد کرده‌اید، باید بعد از ارتقا دوباره آن‌ها را انجام دهید زیرا ``git pull`` فایل docker-compose.yml را از مخزن Github می‌کشد.

*********
Uninstall
*********

برای حذف کامل نظارت بر فارم و پراکسی از سیستم خود، باید مراحل زیر را انجام دهید:

1. متوقف کردن مانیتورینگ و پراکسی و خارج از جعبه: ``docker-compose down``
2. ظروف را بردارید: ``docker container prune``
3. تصاویر را حذف کنید: ``docker image prune -a``
4. داده های نظارتی ذخیره شده را حذف کنید: ``docker volume prune``
5. پوشه را حذف کنید: ``rm -rf farm-proxy/``
