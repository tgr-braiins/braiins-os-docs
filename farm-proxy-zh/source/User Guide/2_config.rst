
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#############
配置
#############

.. contents::
  :local:
  :depth: 2

Braiins矿场代理idea配置可以分位两个主要部分：布线配置和附属程序的配置（就是Prometheus和Grafana）。

*********************
布线配置
*********************

布线配置是在TOML文件中使用一个特定的结构完成的。配置文件的结构与算力布线的图表相对应，并使用上面解释的术语。在以下的文章中解释配置的例子。

Braiin矿场代理在*./farm-proxy/config*路径具有三个预定义的TOML配置实例：

  * *minimal.toml*: 这是一个最小的配置文件，使用一个服务器、一个目标和默认聚合。
  * *sample.toml*: 包含一些其他参数和它们解释。用在*docker-compose.yml*的默认配置文件。也包含一个服务器和一个目标。
  * *two_servers_two_targets.toml*: 如何给矿工部署多个服务器，或者为Braiins矿场代理定义一个备份服务器的例子。

布线配置包含5个部分：服务器、目标、布线、布线目标，布线目标级别。

服务器
======

服务器是下游连接的一个模板。每个服务器只能在一个布线域上使用。

.. code-block:: shell

      [[server]]
      name = "s1"
      port = 3336
      slushpool_bos_bonus = "<slushpool上的用户名>"
      bos_referral_code = "<Braiins OS+推荐计划号>"



* **name**: 服务器的名称。在Grafana监控的所有下游相关指标（提交、份额、连接）可见它作为"服务器"维度的值。
* **port**: 指Braiins矿场代理所打开并其上接受矿工连接的端口。
* **slushpool_bos_bonus**: 应用Braiins OS+推荐计划的Slushpool上的用户名。
* **bos_referral_code**: Braiins OS+推荐计划号。
   
目标
======

目标是一个上游连接的模板。多个布线域可以用它。

.. code-block:: shell

      [[target]]
      name = "MP-GL1"
      url = "stratum+tcp://<mining-pool-address>"
      user_identity = "<userName.workerName（用户名.矿工名）>"

* **name**: 目标的名称。在Grafana监控的所有下游相关指标（提交、份额、连接）可见它作为"服务器"维度的值。
* **user_identity**: identity under which hashrate shall be submitted. The **userName** must exist on the target pool otherwise the pool does not have a key to link your hashrate to your account.

布线域
==============

Routing domain defines boundaries and preferences for hashrate allocation to the desired targets.

.. code-block:: shell

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal 1"
      hr_weight = 100
      [[routing.goal.level]]
      targets = ["MP-GL1"]

* **from**: List of servers which are used in the Braiins Farm Proxy as aggregation proxies.
* **goal**: List of routing rules. Attribute **name** of the goal is visible in the Grafana dashboard for upstream related measures. Attribute **hr_weight** stands for hashrate distribution ratio preference. Beware of the weight and not the percentage. For example, the ratio of weights 2:1 will distribute the hashrate into target endpoints approx. 67% of hashrate goes into target with weight 2 and 33% of hashrate goes into target with weight 1. In the example configurations further down, you can see how to distribute hashrate into several targets.
* Routing goal level lists the **targets** which should be applied as upstream endpoints.

In case the farmer uses Braiins OS+ on his devices, **routing of dev fee is done automatically.**

矿工配置
=====================

To point the farm’s hashrate to the Braiins Farm Proxy, the workers have to be reconfigured. The URL of the Pool in the workers’ firmware configuration has to be set as:

 * Stratum V1: ``stratum+tcp://<farm-proxy-url>:<server_port>``
 *  Stratum V2: ``stratum2+tcp://<farm-proxy-url>:<server_port>/<public_key>``

It is recommended to have a backup pool connection on your miner too in case Braiins Farm Proxy is not working.

配置的例子
======================

To make a better understanding of Braiins Farm Proxy usage and configuration, let’s go through 3 examples.

* **Minimal configuration**: the easiest possible configuration, one server, one target pool. It is not suitable for the real world for its simplicity but it describes the logic of the configuration.

.. code-block:: shell

      # Minimal sample configuration
      [[server]]
      name = "s1"                                
      port = 3336

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "simpleFarm.worker"

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal 1"
      [[routing.goal.level]]
      targets = ["SP-GL"]


* **Basic configuration**: Example with a mining operation in a single facility located in Europe. The primary target is Slush Pool (EU URL), but it is backed up by general and Russian Slush Pool URLs. The farm has 700 hundred ASIC machines and its desired aggregation is 100. It means that there should be between 6 and 7 upstream connections to the target. The farm’s revenue is increased by utilizing BOS+ firmware and mining on Slush Pool.

.. code-block:: shell

      # Basic sample configuration
      [[server]]
      name = "s1"
      port = 3336

      [[target]]
      name = "SP-EU"
      url = "stratum+tcp://eu.stratum.slushpool.com"
      user_identity = "basicFarm.proxy"
      aggregation = 100

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "basicFarm.proxy"
      aggregation = 100

      [[target]]
      name = "SP-RU"
      url = "stratum+tcp://ru-west.stratum.slushpool.com"
      user_identity = "basicFarm.proxy"
      aggregation = 100

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal 1"
      # Primary
      [[routing.goal.level]]
      targets = ["SP-EU"]
      # Back-up 1
      [[routing.goal.level]]
      targets = ["SP-GL"]
      # Back-up 2
      [[routing.goal.level]]
      targets = ["SP-RU"]

* **Multiple owners of the workers**: The farm has dedicated workers for mining on Slush Pool with listening port 3336 and other workers dedicated to Antpool mining on port 3337. Antpool requires maximal extranonce to be 4 and it has to be configured in Braiins Farm Proxy configuration. This example configuration is suitable in the case that the workers have 2 owners and thus multiple servers are defined and used. Multiple instances of Braiins Farm Proxy (let’s say in our example it’s 2 Raspberry Pi machines) with 2 different configurations can be used.
   
.. code-block:: shell

      # Advanced sample configuration
      [[server]]
      name = "s1"
      port = 3336

      [[server]]
      name = "s2"
      port = 3337
      extranonce_size = 2

      [[target]]
      name = "SP-EU"
      url = "stratum+tcp://eu.stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50                                                      

      [[target]]
      name = "Antpool-1"
      url = "stratum+tcp://ss.antpool.com:3333"
      user_identity = "antPoolUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[target]]
      name = "Antpool-2"
      url = "stratum+tcp://ss.antpool.com:443"
      user_identity = "antPoolUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[routing]]
      from = ["s1","s2"]
      [[routing.goal]]
      name = "Goal SP"
      # Primary Slush Pool
      [[routing.goal.level]]
      targets = ["SP-EU"]
      # Back-up Slush Pool
      [[routing.goal.level]]
      targets = ["SP-GL"]
      #
      [[routing.goal]]
      name = "Goal Ant"
      # Primary Antpool
      [[routing.goal.level]]
      targets = ["Antpool-1"]
      # Back-up Antpool
      [[routing.goal.level]]
      targets = ["Antpool-2"]

* **Diversification of pools**: A farm which allocates hashrate into 3 pools using 1 Braiins Farm Proxy instance with 1 server and multiple upstream target endpoints with hashrate allocation 100:80:20 ~ approx. 50% of hashrate goes to the goal “Goal SP”, 40% of hashrate goes to the goal “Goal Ant” and 10% goes to the goal “Goal BTC.com”.

.. code-block:: shell

      # Diversification of pools
      [[server]]
      name = "s1"
      port = 3336
      extranonce_size = 2

      [[target]]
      name = "SP-EU"
      url = "stratum+tcp://eu.stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50

      [[target]]
      name = "Antpool-1"
      url = "stratum+tcp://ss.antpool.com:3333"
      user_identity = "antUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[target]]
      name = "Antpool-2"
      url = "stratum+tcp://ss.antpool.com:443"
      user_identity = "antUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[target]]
      name = "BTCcom-1"
      url = "stratum+tcp://eu.ss.btc.com:1800"
      user_identity = "btcUser.proxy"
      aggregation = 50

      [[target]]
      name = "BTCcom-2"
      url = "stratum+tcp://eu.ss.btc.com:443"
      user_identity = "btcUser.proxy"
      aggregation = 50

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal SP"
      hr_weight = 100
      # Primary Slush Pool
      [[routing.goal.level]]
      targets = ["SP-EU"]
      # Back-up Slush Pool
      [[routing.goal.level]]
      targets = ["SP-GL"]
      #
      [[routing.goal]]
      name = "Goal Ant"
      hr_weight = 80
      # Primary Antpool
      [[routing.goal.level]]
      targets = ["Antpool-1"]
      # Back-up Antpool
      [[routing.goal.level]]
      targets = ["Antpool-2"]
      #
      [[routing.goal]]
      name = "Goal BTC.com"
      hr_weight = 20
      # Primary BTC.com
      [[routing.goal.level]]
      targets = ["BTCcom-1"]
      # Back-up BTC.com
      [[routing.goal.level]]
      targets = ["BTCcom-2"]

* **Different location of the mining operation**: Mining farms with several physical mining containers or buildings in different locations would use a Braiins Farm Proxy instance in each of the locations or for each container with one downstream server and one upstream target with different worker identifiers at each location / container to differentiate the hashrate from each location / container. It is possible to link the Farm Proxies hierarchically to aggregate hashrate from Farm Proxies of individual containers via another Braiins Farm Proxy instance.
   
Configuration Parameters
========================

List of both mandatory and optional parameters available in the Braiins Farm Proxy configuration. Parameters are assigned to the corresponding configuration sections.

Server
------

 * **name**: string: case-sensitive with minimal length 1 (mandatory), name of the server,
 * **port**: integer (mandatory), port dedicated to the Braiins Farm Proxy,
 * **extranonce_size**: integer (optional), extranonce provided to the downstream device (ASIC), must be at least by 2 less than *extranonce_size* of the *target*, default is *4*,
 * **validates_hash_rate**: boolean (true/false, optional), parameter defining if the proxy has to validate submit from downstream, default is *true*,
 * **use_empty_extranonce1**: boolean (true/false, optional), parameter defining if 1 more byte of extra nonce can be used (not every device supports it), default is *false*,
 * **submission_rate**: real (optional), desired downstream submission rate (miner -> proxy) defined as number of submits per one seconds, default is *0.2* (1 submit per 5 seconds),
 * **slushpool_bos_bonus**: string: case-sensitive with minimal length 0 (optional), Slushpool username for which Braiins OS+ discount is applied,
 * **bos_referral_code**: string: case-sensitive with minimal length 6 (optional), Braiins OS+ referral code in the full length shall be provided to get the bonus.
   
Target
------

 * **name**: string: case-sensitive with minimal length 1 (mandatory), name of the target endpoint,
 * **url**: string (mandatory), URL of the mining pool,
 * **user_identity**: string: case-sensitive with minimal length 1 (mandatory),
 * **identity_pass_through**: boolean (true/false, optional), propagation of an individual worker identity to the target pool (submitting feature to upstream), default is *false*,
 * **extranonce_size**: integer (optional), extranonce enforced to the target pool, must be at least by 2 higher than *extranonce_size* of the *server*, default is *6* (**some pools require extranonce at most 4!: AntPool, Binance Pool, Luxor**),
 * **aggregation**: integer (optional), number of aggregated workers (ASICs) per one upstream connection, default is *50*.
   
Routing
-------

 * **name**: string: case-sensitive with minimal length 1 (mandatory), name of the routing domain,
 * **from**: list (mandatory), list of servers which are used as aggregation proxies.
   
Routing Goal
------------

 * **name**: string: case-sensitive with minimal length 1 (mandatory), name of the routing goal,
 * **hr_weight:** integer (optional), weight for the preferred ratio of hashrate distribution.
   
Routing Goal Level
------------------

 * **targets**: list (mandatory), list of targets which are applied as target endpoints in the routing domain.

**************************
Accompanying Configuration
**************************

Other configuration is predefined in the file *docker-compose.yml* which is an essential application for running Braiins Farm Proxy as a multi-container Docker stack. This config file is designed in a way to require as few edits as possible. Docker-compose consists of the configuration of these services:

 * **Prometheus**: runs on port **9090**, it can be accessed in your browser, e.g. ``http://<your-host>:9090/``
 * **Node Exporter**: runs on port **9100**, it can be accessed in your browser, e.g. ``http:/<your-host>:9100/``
 * **Grafana**: runs on port **3000**, it can be accessed in your browser, e.g. ``http://<your-host>:3000/``

Grafana is crucial for the monitoring of mining with Braiins Farm Proxy. Prometheus can be useful in case the user wants to build their own graphs for Grafana dashboards. Node Exporter is an exporter of OS and server metrics for Prometheus database.

.. attention::

   The file *docker-compose.yml* refers to a configuration file **sample.toml** in the configuration of the farm-proxy container. If the farm operator has his own configuration file and wants to address it to the farm-proxy, sample.toml must be replaced by that file. Below you can see the farm-proxy configuration in the *docker-compose.yml.*


.. code-block:: shell

      farm-proxy:
      image: braiinssystems/farm-proxy:v1.0.0-rc4
      container_name: farm-proxy
      network_mode: "host"
      volumes:
      - "./config/sample.toml:/conf/farm_proxy.yml"
      environment:
      - CONF_PATH=/conf/farm_proxy.yml
      - RUST_LOG=debug
      - RUST_BACKTRACE=full
      restart: unless-stopped
      logging:
      driver: "json-file"
      options:
      max-size: "100m"
      max-file: "50"
      compress: "true"

