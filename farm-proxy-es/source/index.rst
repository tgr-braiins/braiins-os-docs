.. toctree::
   :hidden:
   :maxdepth: 3
   :caption: Table of Contents:
   :glob:

   self
   Setup/index*
   User Guide/index*
   Troubleshooting/index*
   Support/index*

---------------

.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>


############
Introducción
############

Braiins Farm Proxy es una aplicación gratuita, independiente que se corre localmente en el sitio de la granja. El objetivo de este proxy es conservar ancho de banda mientras agrega la tasa de hash SHA256 de equipos individuales y los dirije hacia los puntos finales, que usualmente son pools de minería. Los equipos son configurados para conectarse al proxy. Braiins Farm Proxy puede configurarse para conectar muchos pool(s) objetivo con pool(s) de respaldo como un failover. El proxy optimiza las direcciones IP y selecciona los puntos finales con la menor latencia y pérdida de paquetes. También es capaz de cifrar la conexión al pool para mejor privacidad y seguridad en caso de usar Slush Pool como un punto final.

***************
Caracteristicas
***************

 * **Conservación de ancho de banda** debido a la agregación de la tasa de hash y por lo tanto aguas arriba la mayor `dificultad de participaciones <https://braiins.com/blog/bitcoin-mining-pools-luck-shares-estimated-hashrate>`_, resultando en menores transferencias de datos para enviar la misma cantidad de participaciones.

 * **Optimización de La Red** basada en un componente de Braiins Farm Proxy que vigila constantemente la calidad y automáticamente dirige la tasa de hash al punto final con rendimiento óptimo.

 * Braiins Farm Proxy puede usarse con **cualquier pool** para minería bitcoin.

 * **Cambio automático** para el punto final de respaldo si el pool objetivo deja de responder.

 * **Vigilancia regular** en el tablero Grafana incluido en el Braiins Farm Proxy, con la posibilidad de construir su propia solución personalizada de vigilancia mediante una **API de vigilancia**.
 * Los usuarios de Braiins OS+ pueden beneficiarse de la **agregación de la tasa de desarrollo** para mas ahorro de ancho de banda. Braiins Farm Proxy puede agregar tanto la tasa de hash de la granja como la tasa de hash de la tasa de desarrollo.
 * Si el punto final objetivo es SlushPool, una **conexión cifrada** tiene soporte para asegurar la privacidad de los datos y la protección del secuestro de la tasa de hash.
 * Braiins Farm Proxy es completamente software **gratuito**.
