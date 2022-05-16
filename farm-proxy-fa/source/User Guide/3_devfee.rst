
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###################
تجمیع کارمزد توسعه
###################

.. contents::
  :local:
  :depth: 2

در صورتی که +‌Braiins OS نسخه  22.02.01، یا بالاتر باشد، Braiins Farm Proxy به طور خودکار هش‌ریت dev fee   را تجمیع میکند. **بنابراین توصیه می شود از آخرین نسخه +‌Braiins OS** استفاده کنید. برای نسخه‌های قدیمی‌تر از 22.02.01، باید به ورکر را به صورت دستی (مثال پایین) تنظیم نمایید تا هش‌ریت dev fee را تجمیع کند. مثال:

 * ایجاد گروه استخر با نام “bos-management”

  .. |pic3| image:: ../_static/bos_management.png
      :width: 100%
      :alt: BOS Management

  |pic3|

 * آدرس URL مرتبط با Braiins Farm Proxy و پورت سرور پیکربندی شده را در Braiins Farm Proxy (هر یک از سرورها) وارد کنید. راه اندازی مجدد BOSminer مورد نیاز است.

  .. |pic4| image:: ../_static/pool_groups.png
      :width: 100%
      :alt: Pool Groups

  |pic4|

همچنین می توان از Braiins Farm Proxy صرفاً برای تجمیع هش ریت dev fee (و نه بقیه هش ریت) استفاده کرد. این می تواند برای فارم هایی که پراکسی خود را دارند اما +Braiins OS را روی دستگاه های خود اجرا می کنند، مفید باشد. در چنین حالتی، تنظیم Braiins Farm Proxy برای مسیریابی فقط dev fee به نسخه +Braiins OS بستگی دارد:

**Braiins OS+ 22.02.01 و جدیدتر:**

1. به پیکربندی هر ماینر بروید و در ردیف اول آدرس **پراکسی فارم خود** ``stratum+tcp://<own-proxy>:port`` را پر کنید و در **ردیف دوم URL مربوط به Braiins Farm Proxy را پر کنید ``stratum+tcp://<farm-proxy>:port``. این به عنوان یک پشتیبان برای هش ریت کلاینت ها کار می کند و در عین حال **برای تجمیع dev fee** استفاده می شود.
   
  .. |pic5| image:: ../_static/devfee_aggregation.png
      :width: 100%
      :alt: Devfee Aggregation

  |pic5|

2. در فایل پیکربندی Braiins Farm Proxy **پراکسی فارم خود** را به عنوان نقطه پایانی هدف تنظیم کنید.

.. code-block:: shell

      [[server]]
      name = "v1"
      port = 3333

      [[target]]
      name = "Farm's own proxy"
      url = "stratum+tcp://<own-proxy>:port"
      user_identity = "userName.workerName"

      [[routing]]
      from = ["v1"]

      [[routing.goal]]
      name = "Goal 1"

      [[routing.goal.level]]
      targets = ["Farm's own proxy"]

**Braiins OS+ قدیمی تر از 22.02.01:**

1. به پیکربندی هر ماینر بروید، یک گروه "bos-management" در صورتی که قبلا وجود ندارد ایجاد کنید و **گروه bos-management را با URL Proxy Braiins Farm پر کنید** ``stratum+tcp://<farm-proxy>:port``. برای جمع آوری devfee استفاده خواهد شد.

2. در فایل پیکربندی Braiins Farm Proxy، **پراکسی فارم خود** را به عنوان نقطه پایانی هدف تنظیم کنید، مثال قبلی را ببینید.
