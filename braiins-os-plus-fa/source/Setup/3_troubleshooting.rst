
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#########
عیب یابی
#########

*****************************************************************
بازیابی دستگاههای غیر قابل بوت شدن با استفاده از کارت حافظه SD
*****************************************************************

اگر اشکالی پی بیاید و دستگاه شما غیر قابل بوت شدن شود، شما میتوانید با استفاده از ایمیج کارت حافظه SD که قبلا ساخته شده، فریمور اصلی سازنده را با دنبال کردن دستورات زیر بازیابی کنید:

.. code:: bash

   cd braiins-os_am1-s9_ssh_VERSION
   source .env/bin/activate
   python3 restore2factory.py backup/2ce9c4aab53c-2018-09-19/ your-miner-hostname-or-ip

پس از آنکه اجرای اسکریپت تمام شد، چند دقیقه صبر کنید و جامپر را برای بوت از NAND (حافظه داخلی) تنظیم کنید.

******************
عملکرد BOSminer
******************

BOSminer میتواند با استفاده از ترمینال خط فرمان یا از طریق وبسایت کنترل شود.

برای شروع به کار یا توقف BOSminer, از دستورات زیر استفاده کنید:

::

	/etc/init.d/bosminer stop
	/etc/init.d/bosminer start

همچنین BOSminer میتواند از صفحه `System > Startup` کنترل شود. هر بار کاربر بر روی `Save and Apply` در صفحه `Miner -> Configuration` کلیک کند، ریستارت میشود.