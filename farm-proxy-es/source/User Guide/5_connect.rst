
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

############
Conectividad
############

.. contents::
  :local:
  :depth: 2

Braiins Farm Proxy soporta conexiones aguas abajo mediante Stratum V1 inseguro, Stratum V1 seguro y Stratum V2 seguro en modo solo-encabezado. Para las aguas arriba, solo se usan conexiones V1 (debido a la limitación de trabajo para V2 solo-encabezado). V1 seguro se usa para la tarifa de desarrollo, V1 inseguro para la tasa de hash del cliente. Por lo tanto Braiins Farm Proxy tiene un almacenamiento de certificado seguro.
