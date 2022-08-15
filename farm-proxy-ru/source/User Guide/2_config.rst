
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

Конфигурацию Braiins Farm Proxy можно разделить на две основные категории — настройка маршрутизации и настройка сопутствующих программ (таких как BOS Scanner, Prometheus и Grafana).

***********************
Настройка маршрутизации
***********************

Конфигурация маршрутизации выполняется в файлах TOML с использованием определенной структуры. Структура конфигурационного файла соответствует схеме маршрутизации хешрейта и использует терминологию, описанную выше. Конфигурация объясняется в примерах настроек в дальнейшем тексте.

Braiins Farm Proxy имеет 3 предопределенных примера конфигурации TOML, которые размещены в каталоге *./farm-proxy/config*:

  * *minimal.toml*: самый маленький файл конфигурации с одним сервером, одной целью и агрегацией по умолчанию.
  * *sample.toml*: содержит некоторые другие параметры и поясняющие их комментарии. Он используется в качестве файла конфигурации по умолчанию в *docker-compose.yml*. Также содержит один сервер, одну цель.
  * *two_servers_two_targets.toml*: пример, как открыть больше серверов для майнеров или определить резервный сервер для Braiins Farm Proxy.

Конфигурация маршрутизации состоит из 5 сегментов: сервер, цель, маршрутизация, цель маршрутизации и уровень цели маршрутизации.

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/grZ3aQG9mHE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Сервер
======

Сервер (server) — это шаблон для нисходящих подключений. Каждый сервер может использоваться только в одном домене маршрутизации.

.. code-block:: shell

      [[server]]
      name = "s1"
      port = 3336
      slushpool_bos_bonus = "<Braiins Pool username>"
      bos_referral_code = "<Braiins OS+ referral code>"



* **name**: имя сервера. Он виден как значение измерения «сервер» во всех метриках, связанных с нисходящим потоком (отправки, общие ресурсы, подключения) в мониторинге Grafana.
* **port**: определяет порт, который Braiins Farm Proxy будет открывать и принимать соединения майнера.
* **slushpool_bos_bonus**: имя пользователя Braiins Pool, для которого применяется бонус Braiins OS+.
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


* **Базовая конфигурация**: Пример майнинга на одном объекте, расположенном в Европе. Основным пулом является Slush Pool (URL-адрес ЕС), но он поддерживается общими и российскими URL-адресами Braiins Pool. На ферме 700 сотен ASIC-машин, а желаемая агрегация — 100. Это означает, что к цели должно быть от 6 до 7 восходящих подключений. Доход фермы увеличивается за счет использования прошивки BOS+ и майнинга на Braiins Pool.

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

* **Несколько владельцев воркеров**: на ферме есть выделенные воркеры для майнинга на Braiins Pool с портом прослушивания 3336 и другие воркеры, выделенные для майнинга на Antpool на порту 3337. Antpool требует, чтобы максимальное экстранонс было равно 4, и это необходимо настроить в конфигурации Braiins Farm Proxy. Этот пример конфигурации подходит в том случае, если у воркеров 2 владельца и, таким образом, определено и используется несколько серверов. Можно использовать несколько экземпляров Braiins Farm Proxy (скажем, в нашем примере это 2 машины Raspberry Pi) с 2 различными конфигурациями.
   
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
      user_identity = "braiinsPoolUser.proxy"
      aggregation = 50

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "braiinsPoolUser.proxy"
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
      # Primary Braiins Pool
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
      user_identity = "braiinsPoolUser.proxy"
      aggregation = 50

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "braiinsPoolUser.proxy"
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
      # Primary Braiins Pool
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
 * **extranonce_size**: целое число (необязательно), дополнительный одноразовый номер, предоставляемый нисходящему устройству (ASIC), должен быть как минимум на 2 меньше, чем *extranonce_size* *цели*, по умолчанию *4*,
 * **validates_hash_rate**: логический (true/false, необязательный), параметр, определяющий, должен ли прокси-сервер проверять отправку из нисходящего потока, по умолчанию *true*,
 * **use_empty_extranonce1**: логическое значение (true/false, необязательный параметр), параметр, определяющий, можно ли использовать еще 1 дополнительный байт nonce (не каждое устройство поддерживает это), по умолчанию *false*,
 * **submission_rate**: реальная (необязательно), желаемая скорость отправки в нисходящем направлении (майнер -> прокси), определяемая как количество отправок в одну секунду, по умолчанию *0,2* (1 отправка за 5 секунд),
 * **slushpool_bos_bonus**: строка: с учетом регистра, минимальная длина 0 (необязательно), имя пользователя на Braiins Pool, для которого применяется бонус Braiins OS+,
 * **bos_referral_code**: строка: с учетом регистра, минимальная длина 6 (необязательно), необходимо предоставить реферальный код Braiins OS+ полной длины.
   
Цель
----

 * **name**: строка: с учетом регистра, с минимальной длиной 1 (обязательно), имя целевой конечной точки,
 * **url**: строка (обязательно), URL пула майнинга,
 * **user_identity**: строка: с учетом регистра, минимальной длины 1 (обязательно),
 * **identity_pass_through**: логическое значение (true/false, необязательно), распространение идентификатора отдельного воркера в целевой пул (отправка функции в восходящий поток), по умолчанию *false*,
 * **extranonce_size**: целое число (необязательно), экстранонс принудительно применяется к целевому пулу, должен быть как минимум на 2 больше, чем *extranonce_size* *сервера*, по умолчанию *6* (**некоторые пулы требуют не более 4 экстранонсов!: AntPool, Binance Pool , Луксор**),
 * **aggregation**: целое число (необязательно), количество объединенных рабочих процессов (ASIC) на одно восходящее соединение, по умолчанию *50*.
   
Маршрутизация
-------------

 * **name**: строка: с учетом регистра, минимальная длина 1 (обязательно), имя домена маршрутизации,
 * **from**: список (обязательно), список серверов, которые используются в качестве прокси агрегации.
   
Цель маршрутизации
------------------

 * **name**: строка: с учетом регистра, минимальная длина 1 (обязательно), имя цели маршрутизации,
 * **hr_weight:** целое число (необязательно), вес для предпочтительного соотношения распределения хешрейта.
   
Целевой уровень маршрутизации
-----------------------------

 * **targets**: список (обязательный), список целей, которые применяются в качестве целевых конечных точек в домене маршрутизации.

**************************
Сопутствующая конфигурация
**************************

Другая конфигурация предопределена в файле *docker-compose.yml*, который является важным приложением для запуска Braiins Farm Proxy в качестве многоконтейнерного стека Docker. Этот файл конфигурации разработан таким образом, чтобы требовалось как можно меньше правок. Docker-compose состоит из настройки этих сервисов:

 * **BOS Scanner**: сканирует сеть на ssh-порту 8081, на котором майнеры с прошивкой Braiins OS+ предоставляют метрики для очистки Prometheus и   визуализации на информационных панелях Grafana, которые находятся в каталоге ``./monitoring/grafana/provisioning/default_dashboards/farm-monitor/``. Настройка диапазонов IP-адресов для сканирования выполняется в файле **scan_crontab**, более подробная информация об этом содержится в следующей главе :ref:`Мониторинг Braiins OS+ с помощью Prometheus и Grafana`,
 * **Prometheus**: работает на порту **9090**, к нему можно получить доступ в вашем браузере, например ``http://<your-host>:9090/``
 * **Grafana**: работает на порту **3000**, к нему можно получить доступ в вашем браузере, например ``http://<your-host>:3000/``

Grafana имеет решающее значение для мониторинга майнинга с помощью Braiins Farm Proxy. Prometheus передают данные Grafarna и может быть полезен, если пользователь хочет построить свои собственные графики для информационных панелей Grafana.

.. attention::

   Файл *docker-compose.yml* ссылается на файл конфигурации **sample.toml** в конфигурации контейнера прокси фермы. Если оператор фермы имеет свой собственный файл конфигурации и хочет адресовать его прокси-серверу фермы, файл sample.toml необходимо заменить этим файлом. Ниже вы можете увидеть конфигурацию фермы-прокси в файле *docker-compose.yml.*


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

