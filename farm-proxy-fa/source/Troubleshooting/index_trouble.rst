
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

.. toctree::
   :maxdepth: 3
   :glob:

   *

**مشکلات در ارتباطات Downstream**

اگر مشکلی در اتصال به Braiins Farm Proxy وجود داشته باشد، BOSminer ممکن است مدتی طول بکشد تا دوباره وصل شود. هر چه مدت زمان طولانی تری مشکل باقی بماند، مدت زمان بیشتری برای تلاش مجدد طول می کشد. کندی برازنده ای دارد: بلافاصله دوباره امتحان کنید، اگر ناموفق بود، بعد از 1 ثانیه دوباره امتحان کنید، اگر بعد از 2 ثانیه شکست خورد، اگر بعد از 4 ثانیه شکست خورد... بین تلاش های مجدد می تواند تا 1 ساعت طول بکشد. در چنین مواردی راه حل یا راه اندازی مجدد BOSminer است یا صبر کنید.

**Raspberry Pi**

وقتی صحبت از منابع Raspberry Pi به میان می آید، Prometheus و Grafana منابع زیادی را به خود اختصاص می دهند. برای اینکه پراکسی بدون مشکل کار کند، توصیه می شود Prometheus و Grafana را از *docker-compose.yml* حذف کنید و آن را روی دستگاه دیگری اجرا کنید.

**Rejected Hashrate**

در صورت شناسایی Rejected hashrate از downstream یا upstream لطفاً با پشتیبانی Braiins تماس بگیرید. برای حل مشکلات، اطلاعات Grafana > Debug Dashboard و logs کمک زیادی به تیم پشتیبانی ما می کند.