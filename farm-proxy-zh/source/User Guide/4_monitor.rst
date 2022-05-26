
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

##########
监控
##########

.. contents::
  :local:
  :depth: 2

*****************
Grafana仪表板
*****************

Grafana使用端口3000并可以通过浏览器fangwen ``http://<your_host>:3000/``。Grafana使用从Prometheus数据库所刮取的数据。一个名为 "客户仪表板"的简单仪表板已经准备好了。

  .. |pic6| image:: ../_static/dashboard.png
      :width: 100%
      :alt: Dashboard

  |pic6|

仪表板显示以下指标和图表：

 * 仪表板的左侧可以切换从标准算力到开发生费用算力。

   * 时间上的算力：过去5分钟、1小时和24小时内的下游和上游算力。
   * 按照有效性的算力：在过去的3小时内，下游和上游的算力由接受或无效的股份。
   * 按照有效性的算力时间序列：在过去3小时内，下游和上游的算力按有效性分类。

 * 右手边是不变的。

   * Braiins矿场代理的版本。
   * 启动Braiins矿场代理的时间。
   * 下游和上游连接的数量。
   * 对应的聚合。
   * 过去3小时内的聚集时间序列。

Grafana还包含第二个默认的仪表盘，叫做Debug Dashboard FP，关注的是用于调试的详细指标。

矿场可以发挥Prometheus数据库中的可用数据来制作自己的仪表盘，以满足他们的具体需求。

*****************************
改进存在的仪表板
*****************************

如果矿场已经在运行Prometheus和Grafana，并希望用Braiins矿场代理的指标和仪表盘来改进它们，可以通过以下步骤来实现:

* 为Prometheus添加刮削配置，

   * farm-proxy。``http://<farm_proxy>:8080/metrics``。
   * nodeexporter（如果正在运行）。``http://<farm_proxy>:9100/metrics``。
* 从farm-proxy/monitoring/grafana/dashboards导入仪表盘到Grafana。

*************
报告API
*************

Braiins矿场代理可能会因算力聚合而失去在矿池仪表板上的单个矿工的可见性。因此，Braiins矿场代理包括一个报告API，其中包含JSON格式的单个矿机的数据。报告数据集由5分钟的时间段组成，累积单个矿工交付的接受/拒绝的份额。时间段的数量是可以配置的，默认是288，相当于一天。在每个5分钟的边缘，最旧的时段被解散，新的时段被生成。没有在时间段提交的矿工不包括在结果中（并假设没有交付任何股份）。

API叫做 ``curl localhost:8080/report``. 以下有例子:

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
日志
****

Braiins矿场代理正在Docker容器内保存其日志。Docker配置为存储最大5GB的日志。使用日志旋转和压缩。日志文件的数量设置为50个日志，其逻辑是，最旧的文件被解散，从而可以输入新的文件。1个文件的最大尺寸是100MB。以下有一些调查日志的有用命令（为更多细节写 ``docker logs --help``）：

 * 所有可看的日志: ``docker logs farm-proxy``
 * 最近200日志: ``docker logs farm-proxy –-tail 200``
 * 过去20分钟的日志: ``docker logs farm-proxy --since "20m"``
 * 自时间戳以来的日志: ``docker logs farm-proxy --since "2022-03-30T05:20:00"``
 * 时间间隔的日志: ``docker logs farm-proxy --since "2022-03-30T05:20:00" --until 2022-03-30T05:21:36"``

日志保存在 */var/lib/docker/containers/<container_id>/<container_id>-json.log* 文件里。
