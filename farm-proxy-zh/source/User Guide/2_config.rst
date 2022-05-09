
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
* **user_identity**: 提交算力的身份。该**用户名**需要在目标矿池存在，要不矿池无法将您的算力连接到您的账户。

布线域
==============

布线域定义了向所需目标分配算力的边界和偏好。

.. code-block:: shell

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal 1"
      hr_weight = 100
      [[routing.goal.level]]
      targets = ["MP-GL1"]

* **from**: 在Braiins矿场代理中作为聚合代理使用的服务器列表。
* **goal**: 布线规则的列表。 目标的**名称**属性在Grafana仪表盘中可见，它用于上游相关措施。**hr_weight**属性指算力分布比例的偏好。要注意的是权重而不是百分比。例如，权重2:1的比例将把算力分配到目标端点，大约67%的算力进入权重2的目标，33%的算力进入权重1的目标。在以下的配置例子，您可以看如何将算力分配到几个目标。
* 布线目标级别列出用在上游端点应用的**目标**。

如果矿机上使用Braiins OS+固件，则**开发商费用的布线是自动的**  

矿工配置
=====================

为了将矿场的算力指向Braiins矿场代理，矿工必须重新配置。矿工的固件配置中的矿池URL地中必须设置为：

 * 阶层Stratum V1协议: ``stratum+tcp://<farm-proxy-url>:<server_port>``
 * 阶层Stratum V2协议: ``stratum2+tcp://<farm-proxy-url>:<server_port>/<public_key>``

建议您矿机上配置一个备份矿池连接，以防Braiins矿场代理不工作。

配置的例子
======================

为更好地理解Braiins矿场代理的使用和配置，以下有3个例子。

* **最低配置**: the easiest possible configuration, one server, one target pool. It is not suitable for the real world for its simplicity but it describes the logic of the configuration. * **Minimal configuration**最简单的配置，一个服务器，一个目标矿池。它的简单性不适合用在现实世界，但能描述配置的逻辑。

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


* **基本配置**: 一个欧洲的矿场为例。主要目标是Slush Pool（EU URL挖矿地址），使用Slush Pool矿池的通用和俄罗斯的挖矿URL地址作为备份。矿场有7万台ASIC矿机，其期望的聚集度为100。这意味着，应该有6到7个上游连接到目标。该矿场使用BOS+固件提高算力并在Slush Pool矿池上挖矿。

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

* **矿机有多个所有者**。矿场的一部分矿机在Slush Pool上挖矿，监听端口为3336，其他矿机连接到蚂蚁矿池上，使用3337端口。蚂蚁矿池要求超额随机数 (extraNonce）为4，所以这个需要在Braiin矿场代理配置。这个配置的例子适用于矿机有2个主人的情况，因此需要定义和使用多个服务器。Braiins矿场代理的多个实例（在我们的例子是2台Raspberry Pi机器），可以使用2种不同的配置。
   
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

