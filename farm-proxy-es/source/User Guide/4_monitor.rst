
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

##############
Monitorización
##############

.. contents::
  :local:
  :depth: 2

***************
Tablero Grafana
***************

Grafana se encuentra en el puerto 3000 y se puede acceder desde su navegador ``http://<su_host>:3000/``.Grafana se alimenta de los datos reunidos por la base de datos Prometheus. Un tablero sencillo llamado “Client Dashboard” ya está preparado.

  .. |pic6| image:: ../_static/dashboard.png
      :width: 100%
      :alt: Dashboard

  |pic6|

Los tableros muestran las siguientes métricas y gráficas:

 * Del lado a mano izquierda del tablero se puede cambiar de tasa de hash normal a la tasa de hash de la tarifa de desarrollo.

   * Tasa de hash en el tiempo: tasas de hash aguas abajo y aguas arriba en los últimos 5 minutos, 1 hora y 24horas,
   * Tasa de hash según la validez: tasas de hash aguas abajo y aguas arriba por participaciones aceptadas o invalidas en las últimas 3 horas,
   * Tasa de hash serie de tiempo según la validez: tasas de hash aguas abajo y aguas arriba categorizadas por validez en las últimas 3 horas.

 * El lado a mano derecha es estático.

   * Versión del Braiins Farm Proxy,
   * Tiempo de empezar Braiins Farm Proxy,
   * Número de conexiones aguas abajo y aguas arriba,
   * Agregación Correspondiente,
   * Serie de tiempo de agregación en las últimas 3 horas.

Grafana también contiene un segundo tablero predeterminado llamado Debug Dashboard FP que presta atención a métricas detalladas con fines de depuración.

Las granjas pueden hacer sus propios tableros basados en los datos disponibles la base de datos Prometheus para alcanzar sus necesidades específicas.

**************************************
Enriquecimiento de Tableros Existentes
**************************************

En caso de que la granja ya esté corriendo Prometheus y Grafana y quiere enriquecerlo con las métricas de Braiins Farm Proxy metrics y sus tableros, los pasos siguientes pueden hacerse para lograrlo:

* agregación de la configuración de recolección de datos para Prometheus,

   * farm-proxy: ``http://<farm_proxy>:8080/metrics``,
   * nodeexporter (si está corriendo): ``http://<farm_proxy>:9100/metrics``,
* importación de tableros a Grafana desde farm-proxy/monitoring/grafana/dashboards.

**************
API de reporte
**************

Los usuarios de Braiins Farm Proxy pueden perder la visibilidad de equipos individuales en el tablero del pool debido a la agregación. Por lo tanto, Braiins Farm Proxy incluye una API de reporte que contiene datos sobre equipos individuales en formato JSON. El reporte del conjunto de datos consiste en franjas de tiempo de 5-minutos acumulando participaciones aceptadas/rechazadas que han sido entregadas a los equipos individuales. La cantidad de franjas es configurable y por defecto es 288 lo cual es equivalente a un solo día. En cada borde de 5-minutos, la franja mas vieja es descartada y se genera una nueva. Los equipos que no enviaron durante la franja no son incluidos en el resultado (y se asume que no entregó ninguna participación).

La API puede se llaada con ``curl localhost:8080/report``. Un conjunto de datos de ejemplo se muestra a continuación:

.. code-block:: json

      [
        {
          "timestamp": "2022-03-11T18:00:00Z",
          "streams": [
            {
              "name": "v1",
              "direction": "downstream",
              "workers": [
                {
                  "id": "antminer.w1",
                  "shares": {
                    "accepted": 288444,
                    "stale": 0,
                    "invalid": 0
                  },
                  "submits": {
                    "accepted": 7,
                    "stale": 0,
                    "invalid": 0
                  }
                },
                {
                  "id": "antminer.w2",
                  "shares": {
                    "accepted": 0,
                    "stale": 10000,
                    "invalid": 0
                  },
                  "submits": {
                    "accepted": 0,
                    "stale": 2,
                    "invalid": 0
                  },
                }
              ]
            },
            {
              "name": "SP-EU-G1",
              "direction": "upstream",
              "workers": [
                {
                  "id": "btcpmxyz.goal_1",
                  "shares": {
                    "accepted": 288444,
                    "rejected": 0
                  },
                  "submits": {
                    "accepted": 3,
                    "rejected": 0
                  },
                }
              ]
            }
          ]
        },
        {
          "timestamp": "2022-03-11T18:05:00Z",
          "streams": [
            {
              "name": "v1",
              "direction": "downstream",
              "workers": [
                {
                  "id": "antminer.w1",
                  "shares": {
                    "accepted": 300200,
                    "stale": 0,
                    "invalid": 0
                  },
                  "submits": {
                    "accepted": 2,
                    "stale": 0,
                    "invalid": 0
                  }
                }
              ]
            },
            {
              "name": "SP-EU-G1",
              "direction": "upstream",
              "workers": [
                {
                  "id": "btcpmxyz.goal_1",
                  "shares": {
                    "accepted": 300200,
                    "rejected": 0
                  },
                  "submits": {
                    "accepted": 2,
                    "rejected": 0
                  },
                }
              ]
            }
          ]
        }
      ]

*********
Registros
*********

Braiins Farm Proxy guarda sus registros dentro de un contenedor Docker. Docker está configurado para almacenar un máximo de 5 GB de registros. La compresión y rotación de registros está puesta. El número de archivos de registro está fijado en 50 y la lógica es que el archivo mas viejo es descartado y uno nuevo es generado. El tamaño máximo de 1 archivo es 100 MB. Aquí hay algunos comandos útiles para la investigación de los registros (para mas detalles, vea ``docker logs --help``):

 * todos los registros disponibles: ``docker logs farm-proxy``
 * últimos 200 registros: ``docker logs farm-proxy –-tail 200``
 * registros de los últimos 20 minutos: ``docker logs farm-proxy --since "20m"``
 * registros desde la última marca de tiempo: ``docker logs farm-proxy --since "2022-03-30T05:20:00"``
 * registros en un intervalo de tiempo: ``docker logs farm-proxy --since "2022-03-30T05:20:00" --until 2022-03-30T05:21:36"``

Los registros se guardan en */var/lib/docker/containers/<container_id>/<container_id>-json.log*.
