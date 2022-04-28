
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###################
Dev Fee Aggregation
###################

.. contents::
  :local:
  :depth: 2

Braiins Farm Proxy settles dev fee aggregation automatically in case Braiins OS+ is targeted to Braiins Farm Proxy since Braiins OS+ version 22.02.01. **Therefore it is recommended to use the latest Braiins OS+ version**. For releases older than 22.02.01 you have to give a hint to the worker to aggregate the dev fee. This hint means:

 * Creation of pool group named “bos-management”

  .. |pic3| image:: ../_static/bos_management.png
      :width: 100%
      :alt: BOS Management

  |pic3|

 * Enter the URL of Braiins Farm Proxy and port of configured server within the Braiins Farm Proxy (any of the servers). Restart of BOS miners is required.

  .. |pic4| image:: ../_static/pool_groups.png
      :width: 100%
      :alt: Pool Groups

  |pic4|

It is also possible to use Braiins Farm Proxy purely for dev fee aggregation (and not the rest of your hashrate). It can be useful for farms with their own aggregation proxy but running Braiins OS+ on its devices. In such case, the setting of Braiins Farm Proxy for dev fee only routing depends on the Braiins OS+ version:

**Braiins OS+ 22.02.01 and newer:**

1. Go to configuration of each miner and on the first row fill the URL of the **farm's own proxy** ``stratum+tcp://<own-proxy>:port`` and on the **second row fill the URL of Braiins Farm Proxy** ``stratum+tcp://<farm-proxy>:port``. It will work as a backup for clients' hashrate and at the same time **it will be used for devfee aggregation**.
   
  .. |pic5| image:: ../_static/devfee_aggregation.png
      :width: 100%
      :alt: Devfee Aggregation

  |pic5|

2. In the Braiins Farm Proxy config file set up the **farm's own proxy** as a target endpoint.

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

**Braiins OS+ older than 22.02.01:**

1. Go to configuration of each miner, create a “bos-management” group if it doesn’t already exist and **fill the bos-management group with the URL of Braiins Farm Proxy** ``stratum+tcp://<farm-proxy>:port``. It will be used for devfee aggregation.

2. In the Braiins Farm Proxy config file, set up the **farm's own proxy** as a target endpoint, see previous example.
