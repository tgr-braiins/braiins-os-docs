
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#####################
Solución de Problemas
#####################

.. toctree::
   :maxdepth: 3
   :glob:

   *

**Problemas Con la Conexión aguas abajo**

Si hay un problema para conectarse a Braiins Farm Proxy, a BOSminer le puede tomar un tiempo para reconectar. Entre mas persista el problema, mas tiempo tomará para los re-intentos. Tiene ralentización elegante: re-intenta inmediatamente, si falla, re-intenta luego de 1 segundo, si falla luego de 2 segundos, si falla luego de 4 segundos... Podría tardar hasta 1 hora entre re-intentos. En ese caso la solución es o reiniciar BOSminer o esperar.

**Raspberry Pi**

Prometheus y Grafana son bastante exigentes cuando se trata de recursos Raspberry Pi. Por el bien de que el funcione sin problemas, se recomienda remover Prometheus y Grafana del *docker-compose.yml* y correrlos en un dispositivo diferente.

**Tasa de hash Rechazada**

En caso de detectar tasa de hash rechazada desde aguar arriba o aguas abajo, por favor contacte a soporte de Braiins. Para resolver los problemas, la información de “Debug Dashboard” y los registros (logs) proveerá mucha ayuda a nuestro equipo de soporte.
