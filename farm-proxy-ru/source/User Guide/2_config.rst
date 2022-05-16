
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

############
Конфигурация
############

.. contents::
  :local:
  :depth: 2

Конфигурацию Braiins Farm Proxy можно разделить на две основные категории — настройка маршрутизации и настройка сопутствующих программ (таких как Prometheus и Grafana).

***********************
Настройка маршрутизации
***********************

Конфигурация маршрутизации выполняется в файлах TOML с использованием определенной структуры. Структура конфигурационного файла соответствует схеме маршрутизации хешрейта и использует терминологию, описанную выше. Конфигурация объясняется в примерах настроек в дальнейшем тексте.

Braiins Farm Proxy имеет 3 предопределенных примера конфигурации TOML, которые размещены в каталоге *./farm-proxy/config*:

  * *minimal.toml*: самый маленький файл конфигурации с одним сервером, одной целью и агрегацией по умолчанию.
  * *sample.toml*: содержит некоторые другие параметры и поясняющие их комментарии. Он используется в качестве файла конфигурации по умолчанию в *docker-compose.yml*. Также содержит один сервер, одну цель.
  * *two_servers_two_targets.toml*: пример, как открыть больше серверов для майнеров или определить резервный сервер для Braiins Farm Proxy.

Конфигурация маршрутизации состоит из 5 сегментов: сервер, цель, маршрутизация, цель маршрутизации и уровень цели маршрутизации..

Сервер
======

Сервер (server) — это шаблон для нисходящих подключений. Каждый сервер может использоваться только в одном домене маршрутизации.

.. code-block:: shell

      [[server]]
      name = "s1"
      port = 3336
      slushpool_bos_bonus = "<slushpool username>"
      bos_referral_code = "<Braiins OS+ referral code>"



* **name**: имя сервера. Он виден как значение измерения «сервер» во всех метриках, связанных с нисходящим потоком (отправки, общие ресурсы, подключения) в мониторинге Grafana.
* **port**: определяет порт, который Braiins Farm Proxy будет открывать и принимать соединения майнера.
* **slushpool_bos_bonus**: имя пользователя Slush Pool, для которого применяется бонус Braiins OS+.
* **bos_referral_code**: Braiins OS+ реферал.
   
Цель
====

Цель (terget) — это шаблон для восходящих соединений. Он может использоваться совместно несколькими доменами маршрутизации.

.. code-block:: shell

      [[target]]
      name = "MP-GL1"
      url = "stratum+tcp://<mining-pool-address>"
      user_identity = "<userName.workerName>"

* **name**: имя цели. Оно видно как значение параметра «восходящий соединения» (upstream) во всех показателях, связанных с восходящим потоком (отправки, общие ресурсы, подключения) в мониторинге Grafana.
* **url**: URL майнинг-пула.
* **user_identity**: идентификатор, под которым должен быть представлен хешрейт. **userName** должен существовать в целевом пуле, иначе у пула нет ключа для привязки вашего хешрейта к вашей учетной записи.

Домен маршрутизации
===================

Домен маршрутизации определяет границы и предпочтения для выделения хешрейта желаемым целям.

.. code-block:: shell

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal 1"
      hr_weight = 100
      [[routing.goal.level]]
      targets = ["MP-GL1"]

* **from**: Список серверов, которые используются в Braiins Farm Proxy в качестве прокси-агрегаторов.
* **goal**: Список правил маршрутизации. Атрибут **name** цели виден на панели управления Grafana для показателей, связанных с восходящим потоком. Атрибут **hr_weight** означает предпочтительный коэффициент распределения хешрейта. обратите внимание на вес, а не процента. Например, соотношение весов 2:1 будет распределять хэшрейт по целевым конечным точкам прибл. 67% хэшрейта идет на цель с весом 2, а 33% хешрейта идет на цель с весом 1. В приведенных ниже примерах конфигураций вы можете увидеть, как распределить хэшрейт на несколько целей.
* На уровне целей маршрутизации перечислены **цели**, которые следует применять в качестве конечных точек восходящего потока данных.

В случае, если в ферме используется Braiins OS+, **маршрутизация devfee выполняется автоматически.**

Конфигурация воркеров
=====================

Чтобы направить хешрейт фермы на Braiins Farm Proxy, необходимо перенастроить рабочие процессы. URL-адрес пула в конфигурации воркеров должен быть установлен как:

 * Stratum V1: ``stratum+tcp://<farm-proxy-url>:<server_port>``
 *  Stratum V2: ``stratum2+tcp://<farm-proxy-url>:<server_port>/<public_key>``

Рекомендуется также иметь соединение с резервным пулом на вашем майнере на случай, если Braiins Farm Proxy не работает.

Примеры конфигураций
====================

Чтобы лучше понять использование и настройку Braiins Farm Proxy, давайте рассмотрим 3 примера.

* **Минимальная конфигурация**: максимально простая конфигурация, один сервер, один целевой пул. Она не подходит для реального мира из-за своей простоты, но описывает логику конфигурации.

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


* **Базовая конфигурация**: Пример майнинга на одном объекте, расположенном в Европе. Основным пулом является Slush Pool (URL-адрес ЕС), но он поддерживается общими и российскими URL-адресами Slush Pool. На ферме 700 сотен ASIC-машин, а желаемая агрегация — 100. Это означает, что к цели должно быть от 6 до 7 восходящих подключений. Доход фермы увеличивается за счет использования прошивки BOS+ и майнинга на Slush Pool.

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

* **Несколько владельцев воркеров**: на ферме есть выделенные воркеры для майнинга на Slush Pool с портом прослушивания 3336 и другие воркеры, выделенные для майнинга на Antpool на порту 3337. Antpool требует, чтобы максимальное экстранонс было равно 4, и это необходимо настроить в конфигурации Braiins Farm Proxy. Этот пример конфигурации подходит в том случае, если у воркеров 2 владельца и, таким образом, определено и используется несколько серверов. Можно использовать несколько экземпляров Braiins Farm Proxy (скажем, в нашем примере это 2 машины Raspberry Pi) с 2 различными конфигурациями.
   
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

* **Диверсификация пулов**: ферма, которая распределяет хэшрейт на 3 пула, используя 1 экземпляр Braiins Farm Proxy с 1 сервером и несколькими целевыми конечными точками восходящего потока с распределением хешрейта 100:80:20 ~ прибл. 50% хешрейта идет на цель «Цель SP», 40% хешрейта идет на цель «Цель Ant» и 10% идет на цель «Цель BTC.com».

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

* **Разное расположение майнинг-ферм**: майнинговые фермы с несколькими физическими майнинговыми контейнерами или зданиями в разных местах будут использовать экземпляр Braiins Farm Proxy в каждом из местоположений или для каждого контейнера с одним подчиненным сервером и одной вышестоящей целью с разными воркер-процессами. Устанавливаются разные идентификаторы в каждом местоположении/контейнере, чтобы отличать хешрейт от каждого местоположения/контейнера. Можно иерархически связать прокси-серверы фермы для агрегирования хэшрейта от прокси-серверов ферм отдельных контейнеров через другой экземпляр Braiins Farm Proxy.
   
Параметры конфигурации
======================

Список обязательных и необязательных параметров, доступных в конфигурации Braiins Farm Proxy. Параметры назначаются соответствующим разделам конфигурации.

Сервер
------

 * **name**: строка: с учетом регистра, минимальной длины 1 (обязательно), имя сервера,
 * **port**: целое число (обязательно), порт, выделенный для Braiins Farm Proxy,
 * **extranonce_size**: целое число (необязательно), дополнительный одноразовый номер, предоставляемый нисходящему устройству (ASIC), должен быть как минимум на 2 меньше *extranonce_size* цели (*target*), по умолчанию *4*,
 * **validates_hash_rate**: логический (true/false, необязательный), параметр, определяющий, должен ли прокси-сервер проверять отправку из нисходящего потока, по умолчанию *true*,
 * **use_empty_extranonce1**: логическое значение (true/false, необязательный параметр), параметр, определяющий, можно ли использовать еще 1 дополнительный байт nonce (не каждое устройство поддерживает это), по умолчанию *false*,
 * **submission_rate**: реальная (необязательно), желаемая скорость отправки в нисходящем направлении (майнер -> прокси), определяемая как количество отправок в одну секунду, по умолчанию *0,2* (1 отправка за 5 секунд),
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

