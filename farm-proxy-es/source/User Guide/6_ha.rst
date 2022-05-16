
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###################
Alta Disponibilidad
###################

.. contents::
  :local:
  :depth: 2

Para mitigar el solo punto de fallo de Braiins Farm Proxy, se recomienda tener instancias múltiples de Braiins Farm Proxy corriendo. Los mineros deberían acceder a esas instancias a través de un registro DNS round-robin, no directamente.
