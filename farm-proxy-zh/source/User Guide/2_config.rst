
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

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/grZ3aQG9mHE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

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
* **slushpool_bos_bonus**: 应用Braiins OS+推荐计划的Slush Pool上的用户名。
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

* **最低配置**：最简单的配置，一个服务器，一个目标矿池。它的简单性不适合用在现实世界，但能描述配置的逻辑。

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

* **矿池的多样化**。一个矿场使用1个Braiins矿场代理实例和1个服务器以及多个上游目标终端，将算力分配到3个矿池上，算力分配比例为100:80:20~约50%的算力分配到目标 "Goal SP"，40%的算力分配到目标 "Goal Ant"，10%分配到目标 "Goal BTC.com"。

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

* **不同的矿场地点**。一家矿场在不同地点有多个物理挖矿箱或者建筑，该矿场在每个地点或每个挖矿箱使用一个Braiins矿场代理实例，在每个地点/挖矿箱有一个下游服务器和一个上游目标，有不同的矿工标识符，以分别每个地点/挖矿箱的算力。通过另一个Braiins矿场代理实例，可以将矿场代理分层连接起来，从单个容器的矿场代理中汇总算力。
   
配置参数
========================

在Braiins矿场配置中有强制性和可选性参数的列表。参数分配到相应的配置部分。

服务器
------

 * **name**: 串: 大小写敏感，最小长度为1 (强制的），服务器的名称，
 * **port**: 整数 (强制的)，专供Braiins矿场代理的端口，
 * **extranonce_size**: 整数 (可选的)，下游设备（ASIC）所提供的超额随机数，必须至少*target*的*extranonce_size*标值少2， 默认为 *4*，
 * **validates_hash_rate**: 布尔值 (真/假，可选的)， 代理是否需要验证来自下游的提交的参数， 默认为 *true*，
 * **use_empty_extranonce1**: 布尔值 (真/假，可选的)， 定义是否可以使用多一个字节的超额随机数（不是每个设备都支持这个）的参数，默认为 *false*,
 * **submission_rate**: real (可选的)，所需的下游提交率（矿工 → 代理）定义为每1秒的提交数量，默认为*0.2*（每5秒1次提交）。
 * **slushpool_bos_bonus**: 串: 大小写敏感，最小长度为0 (可选的), 适用于Braiins OS+推荐计划的Slush Pool用户名，
 * **bos_referral_code**: 串: 大小写敏感，最小长度为6 (可选的), 为获得优惠要提供全长的Braiins OS+推荐计划号。
   
目标
------

 * **name**: 串: 大小写敏感，最小长度为1 (强制的），目标终端的名称，
 * **url**: 串 (强制的), 矿池的挖矿URL地址，
 * **user_identity**: 串: 大小写敏感，最小长度为1 (强制的)，
 * **identity_pass_through**: 布尔值 (真/假，可选的)，将单个矿工身份传播到目标矿池上（向上游提交功能）， 默认为 *false*,
 * **extranonce_size**: 整数 (可选的)，向目标矿池所强制的超额随机数， 必须比*server*的*extranonce_size*标至少高2，默认为*6*（**一些矿池需要超额随机数至多4!: AntPool, Binance Pool, Luxor**）
 * **aggregation**: 整数 (可选的)，每上游连接聚合矿工（ASIC矿机）的数字，默认为*50*。
   
布线
-------

 * **name**: 串: 大小写敏感，最小长度为1 (强制的），布线域的名称。
 * **from**: 列表 (强制的)， 用作聚合代理的服务器的列表。
   
布线目标
------------

 * **name**: 串: 大小写敏感，最小长度为1 (强制的），布线目标的名称。
 * **hr_weight:** 整数 (可选的)，首选算力分布比例的权重。
   
布线目标级别
------------------

 * **targets**: 列表 (强制的)，在布线域中作为目标端点应用的目标列表。

**************************
监控配置
**************************

其他配置是在*docker-compose.yml*文件中预定义的，这是运行Braiins矿场代理作为多容器Docker堆栈的一个基本应用。这个配置文件的设计使它需要尽可能少的编辑。Docker-compose包括这些服务的配置:
 * **Prometheus**: 在**9090**端口运行，可以通过浏览器访问，例如 ``http://<your-host>:9090/``
 * **Node Exporter**: 在**9100**端口运行，可以通过浏览器访问，例如 ``http:/<your-host>:9100/``
 * **Grafana**: 在**3000**端口运行，可以通过浏览器访问，例如 ``http://<your-host>:3000/``

Grafana对于监控Braiins矿场代理的挖矿很重要。如果用户想为Grafana仪表盘建立自己的图表，Prometheus就很有用。Node Exporter是Prometheus数据库的操作系统和服务器指标的导出器。

.. 注意::

   The file *docker-compose.yml* refers to a configuration file **sample.toml** in the configuration of the farm-proxy container. If the farm operator has his own configuration file and wants to address it to the farm-proxy, sample.toml must be replaced by that file. Below you can see the farm-proxy configuration in the *docker-compose.yml.*

 *docker-compose.yml*文件指的是矿场代理容器配置中的一个配置**sample.toml*的文件。如果矿场经营者有自己的配置文件想用，那s需要用这个文件来代替sample.toml。下面你可以看到*docker-compose.yml.*中的矿场代理配置。

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

