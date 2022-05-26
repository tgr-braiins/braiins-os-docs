
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###########
واژه شناسی
###########

.. contents::
  :local:
  :depth: 2

Braiins Farm Proxy یک برنامه نرم افزاری است که هش ریت را از طریق چندین پورت listening می پذیرد **[Servers]** [#f1]_ آن را به چندین نقطه پایانی **[Targets]** با پیروی از قوانین تعریف شده منتقل می کند **[اRouting goals]**. اهداف به فهرستی از نقاط پایانی **[Routing goal levels]** ارجاع می‌شوند. مجموعه ای از سرورها، اهداف مسیریابی و سطوح هدف مسیریابی به عنوان **Routing domain** نامیده می شود.

اتصالات ماینرها به Braiins Farm Proxy به عنوان اتصالات **Downstream** گفته می شود. اتصالات از Braiins Farm Proxy به استخر استخراج به عنوان اتصالات **Upstream** گفته می شود.

اصطلاحات مورد استفاده با استفاده از نمودارهای زیر در متن قرار داده شده است.

**نمودار مسیریابی هش ریت**

  .. |pic1| image:: ../_static/routing_diagram.png
      :width: 100%
      :alt: Routing Diagram

  |pic1|

**تفسیر نمودار**

  .. |pic2| image:: ../_static/diagram_interpretation.png
      :width: 100%
      :alt: Diagram Interpretation

  |pic2|


.. rubric:: پانوشت

.. [#f1] سرورها از نظر Braiins Farm Proxy پورت های listening هستند، آن را با سرور کلاسیک اشتباه نگیرید.
