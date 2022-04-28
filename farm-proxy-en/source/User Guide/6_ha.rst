
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#################
High Availability
#################

.. contents::
  :local:
  :depth: 2

To mitigate the single point of failure of Braiins Farm Proxy, it is recommended to have multiple instances of Braiins Farm Proxy running. Miners should access these instances through a round-robin DNS record, not directly.