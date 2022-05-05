
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

##########
Monitoring
##########

.. contents::
  :local:
  :depth: 2

*****************
Grafana Dashboard
*****************

Grafana sits on port 3000 and can be accessed in your browser ``http://<your_host>:3000/``.Grafana is fed by scraped data from the Prometheus database. A simple dashboard called “Client Dashboard” is already prepared.

  .. |pic6| image:: ../_static/dashboard.png
      :width: 100%
      :alt: Dashboard

  |pic6|

The dashboards shows following metrics and graphs:

 * Left hand side of the dashboard can be switched for standard hashrate to dev fee hashrate.

   * Hashrate in time: downstream and upstream hashrates in the last 5 minutes, 1 hour and 24hours,
   * Hashrate according to the validity: downstream and upstream hashrates by accepted or invalid shares in the last 3 hours,
   * Hashrate time series according to the validity: downstream and upstream hashrates categorized by validity in the last 3 hours.

 * Right hand side is static.

   * Version of the Braiins Farm Proxy,
   * Time of starting Braiins Farm Proxy,
   * Number of downstream and upstream connections,
   * Corresponding Aggregation,
   * Aggregation time series in the last 3 hours.

Grafana also contains a second default dashboard called Debug Dashboard FP which pays attention to detailed metrics for debugging purposes.

Farms can make their own dashboards based on the available data in Prometheus database to meet their specific needs.

*****************************
Enriching Existing Dashboards
*****************************

In case the farm is already running Prometheus and Grafana and wants to enrich it with Braiins Farm Proxy metrics and dashboards, the following steps can be done to achieve it:

* adding scrapping configuration for Prometheus,

   * farm-proxy: ``http://<farm_proxy>:8080/metrics``,
   * nodeexporter (if running): ``http://<farm_proxy>:9100/metrics``,
* importing dashboards to Grafana from farm-proxy/monitoring/grafana/dashboards.

*************
Reporting API
*************

Users of Braiins Farm Proxy can lose visibility of individual workers in the pool dashboard because of the aggregation. Therefore, Braiins Farm Proxy includes a reporting API which contains data about individual workers in JSON format. The reporting dataset consists of 5-minute time slots accumulating accepted/rejected shares delivered by individual miners. The amount of slots is configurable and the default is 288 which is equivalent to a single day. On each 5-minute edge, the oldest slot is dismissed and a new one is spawned. Workers which did not submit within the slot are not included in the result (and assumed delivered no shares whatsoever).

The API can be called as ``curl localhost:8080/report``. Example dataset is shown below:

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

****
Logs
****

Braiins Farm Proxy is saving its logs within a Docker container. Docker is configured to store a maximum of 5 GB of logs. Log rotation and compression is in place. The number of log files is set to be 50 and the logic is that the oldest file is dismissed and a new one is spawned. The maximum size of 1 file is 100 MB. Here are some useful commands for investigating the logs (for more detail see ``docker logs --help``):

 * all available logs: ``docker logs farm-proxy``
 * last 200 logs: ``docker logs farm-proxy –-tail 200``
 * logs from last 20 minutes: ``docker logs farm-proxy --since "20m"``
 * logs since timestamp: ``docker logs farm-proxy --since "2022-03-30T05:20:00"``
 * logs in time interval: ``docker logs farm-proxy --since "2022-03-30T05:20:00" --until 2022-03-30T05:21:36"``

Logs are saved in */var/lib/docker/containers/<container_id>/<container_id>-json.log*.
