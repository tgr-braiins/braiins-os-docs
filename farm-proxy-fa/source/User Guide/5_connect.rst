
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

############
ارتباطات
############

.. contents::
  :local:
  :depth: 2

Braiins Farm Proxy در اتصالات downstream از طریق Stratum V1 ناامن، Stratum V1 ایمن و Stratum V2 ایمن، فقط در حالت header-only پشتیبانی می کند. برای upstream فقط از اتصالات V1 استفاده می شود (به دلیل محدودیت job برای header-only V2 فقط هدر). V1 ایمن، برای کارمزد توسعه و V1 ناامن برای هش ریت مشتری استفاده می شود. بنابراین Braiins Farm Proxy دارای یک ذخیره سازی گواهی امن است.