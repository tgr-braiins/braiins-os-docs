
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###################
开发商费用聚合
###################

.. contents::
  :local:
  :depth: 2

如果使用Braiins OS+固件22.02.01版本或者更新的版本，Braiins矿场代理自动聚合开发费用。**因此，建议使用Braiins OS+固件最新的版本**。对于比22.02.01更早的版本，你必须给矿工提供一个提示，以聚合开发费用。这个提示是：

 * Creation of pool group named “bos-management”

  .. |pic3| image:: ../_static/bos_management.png
      :width: 100%
      :alt: BOS Management

  |pic3|

 * 输入Braiins矿场代理的URL地址并其内配置的服务器的端口（任何一个服务器）。需要重启使用BOS+矿机。

  .. |pic4| image:: ../_static/pool_groups.png
      :width: 100%
      :alt: Pool Groups

  |pic4|

Braiins矿场代理也可以用于聚合总算里的开发商费用部分而已（而不是其他算力）。这个功能对使用自己的聚合代理，但同时使用Braiins OS+固件的矿场很有用。
在这种情况下，Braiins矿场代理的设置取决于Braiins OS+的版本，只用于开发费的布线:


**Braiins OS+ 22.02.01和更新的版本:**

1. 访问每个矿机的配置，在第一行填写**矿场自己的代理**的UR地址``stratum+tcp://<own-proxy>:port``，在**第二行填写Braiins矿场代理的URL地址**``stratum+tcp://<farm-proxy>:port``。这样代理作为客户算力的备份，同时**用于开发商费用的聚合**。

  .. |pic5| image:: ../_static/devfee_aggregation.png
      :width: 100%
      :alt: Devfee Aggregation

  |pic5|

2. 在Braiins矿场代理配置文件中，将**矿场自己的代理**设置为目标端点。

.. code-block:: shell

      [[server]]
      name = "v1"
      port = 3333

      [[target]]
      name = "Farm's own proxy"
      url = "stratum+tcp://<own-proxy>:port"
      user_identity = "userName.workerName"

      [[routing]]
      from = ["v1"]

      [[routing.goal]]
      name = "Goal 1"

      [[routing.goal.level]]
      targets = ["Farm's own proxy"]

**比Braiins OS+ 22.02.01更旧的版本:**

1.访问每个矿机的配置，如果已经不存在，创建一个 "bos-management "组并**将Braiins矿场代理URL地址填写到bos-management组里**``stratum+tcp://<farm-proxy>:port``。这样用于开发商费用的聚合。

2. 在Braiins矿场代理配置文件中，将**矿场自己的代理**设置为目标端点，可以参考以上的例子。
