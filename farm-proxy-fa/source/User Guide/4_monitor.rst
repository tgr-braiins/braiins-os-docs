
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

##########
مانیتورینگ
##########

.. contents::
  :local:
  :depth: 2

*****************
 داشبورد Grafana
*****************

Grafana بر روی پورت 3000 قرار دارد و می توان به آن در مرورگر خود ``http://<your_host>:3000`` دسترسی داشت. Grafana توسط داده های پاک شده از پایگاه داده Prometheus تغذیه می شود. یک داشبورد ساده به نام "Client Dashboard" از قبل آماده شده است.

  .. |pic6| image:: ../_static/dashboard.png
      :width: 100%
      :alt: Dashboard

  |pic6|

داشبورد معیارها و نمودارهای زیر را نشان می دهد:

 * از سمت چپ داشبورد را می‌توان از هش‌ریت استاندارد به هش ریت dev fee تغییر داد.

  * هش ریت در زمان: هش های downstream و upstream در 5 دقیقه، 1 ساعت و 24 ساعت گذشته،
  * هش ریت بر اساس اعتبار: هش های downstream و upstream توسط سهام پذیرفته شده یا نامعتبر در 3 ساعت گذشته،
  * سری های زمانی هش ریت بر اساس اعتبار: هش های downstream و upstream که بر اساس اعتبار در 3 ساعت گذشته طبقه بندی شده اند.

 * سمت راست آمار را نشان میدهد.

  * نسخه Braiins Farm Proxy،
  * زمان شروع Braiins Farm Proxy،
  * تعداد اتصالات downstream و upstream
  * تجمیع مربوطه،
  * مجموعه زمانی تجمیع در 3 ساعت گذشته.

Grafana همچنین دارای یک داشبورد پیش‌فرض دوم به نام Debug Dashboard FP است که به معیارهای دقیق برای اهداف عیب یابی توجه می‌کند.

فارم‌ها می توانند داشبوردهای خود را بر اساس داده های موجود در پایگاه داده Prometheus برای رفع نیازهای خاص خود بسازند.

*****************************
غنی سازی داشبوردهای موجود
*****************************

در صورتی که فارم قبلاً Prometheus و Grafana را اجرا می کند و می خواهد آن را با معیارها و داشبوردهای Braiins Farm Proxy غنی کند، مراحل زیر را می توان برای دستیابی به آن انجام داد:

* افزودن پیکربندی اسکراپ برای Prometheus،

   * farm-proxy: ``http://<farm_proxy>:8080/metrics``,
   * nodeexporter (if running): ``http://<farm_proxy>:9100/metrics``,
* وارد کردن داشبورد به Grafana از farm-proxy/monitoring/grafana/dashboards.

*************
API گزارشگیری
*************

کاربران Braiins Farm Proxy می‌توانند دید یک به یک ورکرها را در داشبورد استخر به دلیل تجمیع از دست بدهند. بنابراین، Braiins Farm Proxy شامل یک API گزارش‌دهی است که حاوی داده‌های مربوط به تک تک ورکرها در قالب JSON است. مجموعه داده گزارش شامل شکاف‌های زمانی ۵ دقیقه‌ای است که سهام‌های پذیرفته‌شده/ردشده را جمع‌آوری می‌کند که توسط دستگاه‌ها یک به یک تحویل داده می‌شود. تعداد اسلات ها قابل تنظیم است و پیش فرض ۲۸۸ است که معادل یک روز است. در هر لبه ۵ دقیقه ای، قدیمی ترین شکاف حذف می شود و یک شکاف جدید ایجاد می شود. ورکرهایی که در اسلات ثبت نام نکرده اند در نتیجه شامل نمی شوند (و فرض می شود که هیچ سهمی تحویل داده نشده اند).

API را می توان به عنوان ``curl localhost:8080/report`` نامید. مجموعه داده نمونه در زیر نشان داده شده است:

.. code-block:: json

      [
        {
          "timestamp": "2022-03-11T18:00:00Z",
          "streams": [
            {
              "name": "v1",
              "direction": "downstream",
              "workers": [
                {
                  "id": "antminer.w1",
                  "shares": {
                    "accepted": 288444,
                    "stale": 0,
                    "invalid": 0
                  },
                  "submits": {
                    "accepted": 7,
                    "stale": 0,
                    "invalid": 0
                  }
                },
                {
                  "id": "antminer.w2",
                  "shares": {
                    "accepted": 0,
                    "stale": 10000,
                    "invalid": 0
                  },
                  "submits": {
                    "accepted": 0,
                    "stale": 2,
                    "invalid": 0
                  },
                }
              ]
            },
            {
              "name": "SP-EU-G1",
              "direction": "upstream",
              "workers": [
                {
                  "id": "btcpmxyz.goal_1",
                  "shares": {
                    "accepted": 288444,
                    "rejected": 0
                  },
                  "submits": {
                    "accepted": 3,
                    "rejected": 0
                  },
                }
              ]
            }
          ]
        },
        {
          "timestamp": "2022-03-11T18:05:00Z",
          "streams": [
            {
              "name": "v1",
              "direction": "downstream",
              "workers": [
                {
                  "id": "antminer.w1",
                  "shares": {
                    "accepted": 300200,
                    "stale": 0,
                    "invalid": 0
                  },
                  "submits": {
                    "accepted": 2,
                    "stale": 0,
                    "invalid": 0
                  }
                }
              ]
            },
            {
              "name": "SP-EU-G1",
              "direction": "upstream",
              "workers": [
                {
                  "id": "btcpmxyz.goal_1",
                  "shares": {
                    "accepted": 300200,
                    "rejected": 0
                  },
                  "submits": {
                    "accepted": 2,
                    "rejected": 0
                  },
                }
              ]
            }
          ]
        }
      ]

********
گزارشات
********

Braiins Farm Proxy لاگ های خود را در یک کانتینر Docker ذخیره می کند. Docker برای ذخیره حداکثر ۵ گیگابایت گزارش پیکربندی شده است. چرخش لاگ و فشرده سازی نیز وجود دارد. تعداد فایل های log روی ۵۰ تنظیم شده است و منطق این است که قدیمی ترین فایل حذف می شود و یک فایل جدید ایجاد می شود. حداکثر حجم ۱ فایل ۱۰۰ مگابایت است. در اینجا چند دستور مفید برای بررسی گزارش‌ها وجود دارد (برای جزئیات بیشتر دستور ``Docker logs --help`` را اجرا کنید):

* همه گزارش های موجود: ``docker logs farm-proxy``
* ۲۰۰ گزارش آخر: ``docker logs farm-proxy –-tail 200``
* گزارش‌های ۲۰ دقیقه گذشته: ``docker logs farm-proxy --since "2m"``
* گزارش‌ها از از زمان مشخص: ``docker logs farm-proxy --since "2022-03-30T05:20:00"``
* گزارش‌ها در بازه زمانی: ``docker logs farm-proxy --since "2022-03-30T05:20:00" --until 2022-03-30T05:21:36"``

گزارش‌ها در مسیر */var/lib/docker/containers/<container_id>/<container_id>-json.log* ذخیره می‌شوند.
