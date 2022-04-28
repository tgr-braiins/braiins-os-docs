
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

############
Connectivity
############

.. contents::
  :local:
  :depth: 2

Braiins Farm Proxy supports downstream connections via unsecured Stratum V1, secured Stratum V1 and secured Stratum V2 in header-only mode. For the upstream, only V1 connections are used (due to job limitation for header-only V2). Secured V1 is used for dev fee, unsecured V1 for client hashrate. Therefore Braiins Farm Proxy has a secured certificate storage.
