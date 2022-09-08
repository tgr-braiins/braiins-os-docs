
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

.. _monitoring:

=============================================
Мониторинг Braiins OS+ с Prometheus и Grafana
=============================================

.. contents::
  :local:
  :depth: 2

Введение
========

Начиная с Braiins OS+ 22.02.2, каждый майнер выдает статистику в форме, которую Prometheus легко усваивает. Затем данные Prometheus можно визуализировать в Grafana.

Prometheus — это набор инструментов для мониторинга и оповещения систем с открытым исходным кодом. Prometheus собирает и хранит свои метрики в виде данных временных рядов, т. е. информация о метриках хранится с отметкой времени, в которую она была записана, наряду с необязательными парами ключ-значение, называемыми метками.

Grafana с открытым исходным кодом — это программное обеспечение для визуализации и аналитики с открытым исходным кодом. Оно позволяет вам запрашивать, визуализировать, предупреждать и исследовать ваши показатели. Оно предоставляет вам инструменты для преобразования данных базы данных временных рядов в подробные графики и визуализации.

.. attention::
   
   Мониторинг можно запускать либо как отдельное приложение, либо оно уже включено в стек докеров Braiins Farm Proxy.

Ограничения
-----------

- Для устройств S9 доступен только ограниченный набор показателей.
- Мониторинг работает только для майнеров с установленной Braiins OS+
- Поддерживаемые архитектуры для ssh-сканера: Linux AMD64, RPi 64-бит или 32-бит.

Установка
=========

Быстрый запуск
--------------

Быстрый запуск в случае, если мониторинг выполняется как отдельное приложение (не как часть Braiins Farm Proxy).

- ``git clone https://github.com/braiins/bos-farm-monitor.git``
- Измените список диапазонов IP-адресов для сканирования ``./scan_crontab``
- ``docker-compose up -d``
- Перейдите в localhost:3000

Предварительные требования
--------------------------

**Linux**

-  Слудуйте `инструкции по установке докера <https://docs.docker.com/engine/install/ubuntu/>`__
-  Слудуйте `необязательным шагам установки поста докера <https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user>`__
-  Слудуйте `установке docker-compose <https://docs.docker.com/compose/install/>`__

**RPi**

-  Следуйте одному из многих руководств - например https://jfrog.com/connect/post/install-docker-compose-on-raspberry-pi/

Установка
---------

Быстрый старт, если мониторинг запускается как отдельное приложение (не как часть Braiins Farm Proxy).

**Убедитесь, что необходимые компоненты установлены**

.. code-block::

    docker version
    docker-compose --version
    git --version

**Скачайте репозиторий Braiins**

Вы можете клонировать репозиторий, используя git:

.. code-block::

   sudo apt update
   sudo apt install git
   git clone https://github.com/braiins/bos-farm-monitor.git

Вы можете скачать zip файл со всеми файлами `https://github.com/braiins/bos-farm-monitor/archive/refs/heads/master.zip <https://github.com/braiins/bos-farm-monitor/archive/refs/heads/master.zip>`__

Конфигурация Prometheus
-----------------------

Прежде чем вы сможете начать мониторинг своей фермы, вам необходимо подготовить конфигурацию на основе примеров в каталоге config. 
Есть два файла:

-  ``/config/prometheus_scan.yml``
-  ``/config/prometheus_static.yml``

Единственная существенная разница между ними состоит в том, что «scan» лучше всего.
используется, если ваши майнеры имеют IP-адреса, назначенные DHCP, в то время как
«static» можно использовать, когда ваши майнеры имеют статические IP-адреса.

**Конфигурация по умолчанию**

Конфигурация по умолчанию имеет следующие функции:

- Задание по сбору метрик Braiins OS+ под названием braiinsos-data.
- Перемаркировка адресов конечных точек метрик (удаление порта 8081)
- Парсинг IP-адресов:

   -  Второй октет: label ``site_id``
   -  Третий октет: label ``subnet_id``
   -  Четвертый октет: label ``host_id``

- Удаление некоторых более интенсивных данных метрик (вы можете добавить их обратно, просто убедитесь, что размер вашего экземпляра соответствует размеру)
- Создание статической метки для prometheus_static.yml - метка назначается динамически при использовании prometheus_scan.yml (подробнее об этом позже).

**Организуйте свою ферму так, чтобы ее было легко мониторить**

Для более крупной фермы вы можете сгруппировать майнеров в несколько логических
групп, чтобы вы могли видеть производительность отдельных компонентов. 
Группировка может отличаться в зависимости от размера и структуры вашей фермы,
некоторые из наиболее типичных элементов топологии фермы:

-  Здание
-  Секция
-  Танк
-  Ряд

Для этого у вас есть следующие варианты:

 **Использовать подсети и анализировать октеты IP-адресов**
   Если у вас есть статические IP-адреса и вы используете их для организации своих майнеров, самый простой способ подготовить данные для отчетов — улучшить конфигурацию prometheus с помощью переименования, полученного из IP-адресов. Пример ниже показывает, как это сделать. Вы, конечно, можете использовать другие названия, кроме раздела, резервуара, майнера.

   .. code-block::

      relabel_configs:
      # Extract the second octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.(\\d+)\\.\\d+\\.\\d+.*"
        target_label: "section"
      # Extract the third octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.\\d+\\.(\\d+)\\.\\d+.*"
        target_label: "tank"
      # Extract the last octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.\\d+\\.\\d+\\.(\\d+).*"
        target_label: "miner"
 
 **Используйте отдельные задания вместе с дополнительной пользовательской меткой**
   OКонфигурация Prometheus (хранится в prometheus.yml) может содержать несколько заданий. Например, вы можете создавать отдельные задания для каждого здания или контейнера. Каждая метрика имеет метку задания, что делает ее очень удобным подходом к групповым экземплярам (майнерам). В случае, если в вашей конфигурации есть другие (не связанные с майнингом) задания, вы можете добавить к каждому заданию собственную метку, чтобы использовать эту метку для фильтрации/группировки. Пример, который можно использовать в разделе relabel_configs для добавления метки здания к каждому экземпляру, отслеживаемому заданием, со значением «Здание A».:

   .. code-block::

      - target_label: "building"
        replacement: "Building A"
        
 **Используйте несколько экземпляров prometheus**
   В случае тысяч или более майнеров может быть проще настроить отдельный экземпляр Prometheus для каждой группы майнеров. Обратитесь к документации Prometheus о том, как настроить `федерацию <https://prometheus.io/docs/prometheus/latest/federation/>`__.

 **Использовать имя пользователя/рабочее имя и переназначать ярлыки (не рекомендуется)**
   Использование username/workername для кодирования информации о физическом местоположении майнеров — обычно используемый подход с устаревшими приложениями мониторинга. Этот подход плохо работает с тем, как Prometheus управляет временными рядами и хранит их, что не имеет ничего общего с традиционной реляционной базой данных. Мы не рекомендуем использовать username/workername для структурирования вашей фермы с prometheus по следующим причинам.:

   - большинство метрик не имеют workername, так как метки и соединения должны быть созданы в запросах (замедляет работу, подвержено ошибками)
   - с одним майнером может быть связано несколько username/workername; это делает соединения еще более сложными (необходима предварительная агрегация с логикой, какое значение выбрать)

 **Используйте несколько диапазонов IP-адресов со сканированием**
   Если у вас есть майнеры с IP-адресом, назначенным DHCP, и вы используете сканирование своей сети для доставки майнеров в Prometheus, вы можете определить несколько сетевых диапазонов, и каждый диапазон может иметь уникальное значение, определенное и присвоенное метке (подробнее об этом в следующем разделе).

**Добавление майнеров в конфигурацию**

Существуют следующие основные варианты, как добавить свои майнеры в
конфигурация:

- Используйте параметры обнаружения сервисов, предоставляемые Prometheus.
- Добавьте IP-адресоа в файл конфигурации вручную

Список IP-адресов напрямую работает лучше всего, когда IP-адреса, назначенные майнерам, являются статическими. В случае DHCP обнаружение службы является лучшим вариантом.

**Обнаружение службы**

Обнаружение служб на основе файлов — это параметр, включенный по умолчанию. Чтобы начать использовать его, вам нужно настроить файл ``./scan_crontab`` в текстовом редакторе. Текущие примеры::

.. code-block::

    * */3 * * * * * ssh_scan.sh "1.2.3.0-255" "Building A"
    * */3 * * * * * ssh_scan.sh "1.2.0-255.3" "Building B"

Каждая строка будет сканировать определенный диапазон IP-адресов в поисках отвечающих майнеров и сохранять список, чтобы он был доступен для Prometheus. Строка "Здание А" / "Здание Б" может быть произвольным названием. В настоящее время он будет динамически сопоставляться с созданием меток. Сканирование выполняется каждые три минуты — вы можете изменить его в зависимости от размера вашей фермы и ваших потребностей. Если вы не знакомы с синтаксисом cron, это объясняется `здесь <https://www.netiq.com/documentation/cloud-manager-2-5/ncm-reference/data/bexyssf.html>`__.

**Список IP-адресов**

Для того, чтобы использовать статический список IP-адресов, вам необходимо изменить файл ``docker-compose.yml``,

Во-первых, закомментируйте изображение crontab, чтобы отключить динамическое сканирование:

.. code-block::

   # bos_scanner:
   # image: braiinssystems/bos_monitor:v1.0.0
   # container_name: bos_scanner
   # volumes:
   #  - ./scan_crontab:/usr/local/share/scan_crontab
   #  - scanner_data:/mnt:rw
   # network_mode: "host"

Во-вторых, закомментируйте динамическое сканирование и включите использование другого файла конфигурации. Так должно выглядеть после изменений:

.. code-block::

   #- '--config.file=/etc/prometheus/prometheus_scan.yml'
   - '--config.file=/etc/prometheus/prometheus_static.yml'

IP-адреса перечислены в виде массива в файле конфигурации.
`prometheus_static.yml`. Измените записи со списком ваших майнеров:

.. code-block:

   - targets: ['10.35.31.2:8081','10.35.32.2:8081']

Обратите внимание, что:

- Порт должен быть добавлен в конце IP-адреса. Порт 8081 — это место, где доступны метрики для Prometheus.
- IP-адреса указаны в кавычках и разделены запятой.

В случае, если у вас нет статических IP-адресов, IP-адрес любого майнера может измениться. Если вы все еще хотите использовать этот статический подход, попробуйте увеличить время аренды до высокого значения (например, 48 часов) для вашего DHCP-сервера, чтобы IP-адрес переназначался, даже если майнер некоторое время находится в автономном режиме.

Чтобы добавить все майнеры в список, вы можете просканировать свою ферму на наличие устройств с помощью BOS Toolbox и сгенерировать конфигурацию на основе результатов. Вы можете использовать либо UX, либо командную строку, чтобы получить список.

Пример командной строки (linux):

.. code-block::

   ./bos-toolbox scan -o ips.txt 10.10.0.0/16
   cat ips.txt \| sed "s/.*/'&:8081'/" \| paste -sd',' \| sed "s/.*/[&]/"

Первая команда просканирует все IP-адреса в диапазоне 10.10.0.0 и 10.10.255.255. Вторая напечатает массив с IP-адресами, которые вы можете вставить в конфигурацию.

Мониторинг возможен только для майнеров с Braiins OS+. Если вы используете майнеры без Braiins OS+, лучше использовать:

.. code-block::
   
   ./bos-toolbox scan 10.10.0.0/16 &> ips.txt
   grep "\| bOS" ips.txt \| cut -d"(" -f2 \| cut -"d)" -f1 \| sed "s/.*/'&:8081'/" \| paste -sd',' \| sed "s/.*/[&]/"

Для разных диапазонов IP вы можете использовать:

-  10.10.10.0/24 for range 10.10.10.0 - 10.10.10.255
-  10.10.0.0/16 for range 10.10.0.0 to 10.10.255.255
-  10.0.0.0/8 for range 10.0.0.0 to 10.255.255.25

Начать мониторинг
----------------

.. code-block::

   docker-compose up -d

Вы можете убедиться, что контейнер работает, используя `docker ps`.

Теперь вы можете перейти к: `http://<your_host>:3000`.

Операции
--------

**Изменение конфигурации**

Измените файл конфигурации в соответствии с вашими потребностями

.. code-block::

   docker-compose restart prometheus

**Обновление до более новой версии**

.. code-block::

   git pull origin master
   docker-compose up -d

Панели мониторинга
==================

В нашем репозитории мы предоставляем образцы информационных панелей, которые помогут вам начать подготовку мониторинга для вашей фермы в соответствии с вашими потребностями.

Панель управления фермой
------------------------

Это панель инструментов высокого уровня, которая отслеживает всех майнеров на вашей ферме. Он имеет встроенный селектор источника данных на случай, если у вас запущено несколько экземпляров prometheus. Он также содержит несколько детализированных отчетов, выделенных на снимке экрана ниже:

  .. |pic3| image:: ../_static/monitoring_dashboard.png
      :width: 100%
      :alt: Dashboard

  |pic3|

Части, выделенные красным, приведут вас к детализированному отчету со списком экземпляров. Части, выделенные синим цветом, перейдут непосредственно в UX майнера.

Пример панели управления фермой — по строению
---------------------------------------------

Панель инструментов имеет функцию, при которой ряды панелей Grafana автоматически отображаются для каждого определенного здания. Это создается динамически на основе значений метки здания. Полный поток выглядит следующим образом в примере конфигурации:

- в prometheus.yml создаются два отдельных задания
- к каждой работе добавлено здание метки со значением, представляющим здание
- на панели инструментов grafana определено построение параметров, которое связано с меткой здания.
- заголовок строки имеет $building в качестве имени - он будет расширен значениями метки
- каждая панель имеет фильтр $building

Метрики и ярлыки
================
Каждый временной ряд однозначно идентифицируется своим именем метрики и необязательными парами ключ-значение, называемыми метками. Имя метрики определяет общую характеристику измеряемой системы. Метки включают многомерную модель данных Prometheus: любая заданная комбинация меток для одного и того же имени метрики идентифицирует конкретное многомерное воплощение этой метрики. Язык запросов позволяет выполнять фильтрацию и агрегирование на основе этих параметров.

Обзор:

-  ``application_version_details (instance, version_full, toolchain)``
-  ``client_status (instance, connection_type, host, protocol, user, worker)``
-  ``hashboard_nominal_hashrate_gigahashes_per_second (instance, hashboard)``
-  ``hashboard_shares (instance, hashboard, type: valid | invalid | duplicate)``
-  ``miner_metadata (instance, model, os_version)``
-  ``miner_power (instance, type: wall | estimate | limit, socket)``
-  ``temperature (instance, chip_addr, chip_in_domain, voltage_domain,hashboard, location: chip | pcb)``
-  ``stratum_accepted_shares_counter (instance, host, user, worker, protocol, connection_type)``
-  ``stratum_rejected_shares_counter (instance, host, user, worker, protocol, connection_type)``
-  ``stratum_accepted_submits_counter (instance, host, user, worker, protocol, connection_type)``
-  ``stratum_rejected_submits_counter (instance, host, user, worker, protocol, connection_type)``
-  ``tuner_stage (instance, hashboard)``

Сведения о версии приложения
----------------------------

Версия приложения, создающего временные ряды.

``application_version_details``

**Ярлыки**

- instance: IP-адрес майнера
- version_full: версия приложения
- toolchain
   
Статус клиента
--------------

Статус клиента: (stopped = 0, running = 1 , failed = -1)

``client_status``

**Ярлыки**

- instance: IP-адрес майнера
- connection_type: тип соединения, который может быть либо *user*, либо *dev-fee*
- host: URL-адрес хоста, обычно URL-адрес пула или прокси
- protocol: протокол майнинга
- user: обычно имя пользователя майнинг-пула клиента
- worker: имя воркера


Hashboard Номинальный хешрейт (Gh/s)
---------------------------------

Номинальный хэшрейт для каждого хэшборда в Gh/s.

``hashboard_nominal_hashrate_gigahashes_per_second``

**Ярлыки**

- instance: IP-адрес майнера
- hashboard: ранг хэшборда

Решения хэшборда
----------------

Количество действительных решений, созданных хэшбордами. Решения хэшборда можно использовать для расчета реального хэшрейта для хэшборда, майнера или другой группы. Эта метрика не предоставляет информацию о том, были ли решения приняты целью — для этого следует использовать stratum_accepted_shares_counter.

``hashboard_shares (counter)``

**Ярлыки**

-  instance: IP address of the miner
-  hashboard: rank of the hashboard
-  type: type of the shares with respect to its validity, *valid* - valid shares, *invalid* - invalid shares, *duplicate* - duplicated shares

**Examples**

Average number of hashes per second over last 20 seconds for all instances:

.. code-block::

   sum(rate(hashboard_shares[20s])) * 2^32

Average number of hashes per second over last 20 seconds by instance:

.. code-block::

   sum by(instance) (rate(hashboard_shares[20s])) * 2^32

Average number of hashes per second over last 20 seconds for all instances by miner type:

.. code-block::

   sum by (model) (
      (sum by (instance)((rate(hashboard_shares[20s]))) * 2^32)
      * on(instance) group_left(model) count by (instance, model) (miner_metadata)
   )

Miner Metadata
--------------

``miner_metadata``

**Labels**

- instance: IP address of the miner
- model: model of the miner
- os_version: version of the firmware

**Examples**

Number of miners by model:

.. code-block::

   count_values by (model) ("x", miner_metadata)

Miner Power
-----------

``miner_power``

**Labels**

-  instance: IP address of the miner
-  type: 3 types, *estimated* - estimated power, *limit* - power limit, *psu* - measured power, *wall*
-  socket

**Examples**

Total estimated power consumption for all instances:

.. code-block::

   sum(miner_power{type="estimated"})

Total power limit for all instances:

.. code-block::

  sum(miner_power{type="limit"})

Stratum Accepted Shares Counter
-------------------------------

Total number of shares accepted by target. For one instance, there are
typically more targets, represented by host label.

``stratum_accepted_shares_counter (counter)``

**Labels**

-  instance: IP address of the miner
-  connection_type: type of the connection, which could be either *user* or *dev-fee*
-  host: URL of the host, usually URL of the pool or proxy
-  protocol: mining protocol
-  user: usually mining pool username of the client
-  worker: name of the worker

**Examples**

Average number of accepted shares per second over last 20 seconds for
all instances by target:

.. code-block::

   sum by(host) (rate(stratum_accepted_shares_counter[20s]))

Stratum Rejected Shares Counter
-------------------------------

Total number of shares rejected by target.

``stratum_rejected_shares_counter (counter)``

**Labels**

-  instance: IP address of the miner
-  connection_type: type of the connection, which could be either *user* or *dev-fee*
-  host: URL of the host, usually URL of the pool or proxy
-  protocol: mining protocol
-  user: usually mining pool username of the client
-  worker: name of the worker

**Examples**

Average number of rejected shares per second over last 20 seconds for all instances by target:

.. code-block::

   sum by(host) (rate(stratum_rejected_shares_counter[20s]))

Stratum Accepted Submits Counter
--------------------------------

Total number of submits accepted by target. For one instance, there are
typically more targets, represented by host label.

``stratum_accepted_submits_counter (counter)``

**Labels**

-  instance: IP address of the miner
-  connection_type: type of the connection, which could be either *user* or *dev-fee*
-  host: URL of the host, usually URL of the pool or proxy
-  protocol: mining protocol
-  user: usually mining pool username of the client
-  worker: name of the worker

**Examples**

Average number of accepted submits per second over last 20 seconds for
all instances by target:

.. code-block::

   sum by(host) (rate(stratum_accepted_submits_counter[20s]))

Stratum Rejected Submits Counter
--------------------------------

Total number of submits rejected by target.

``stratum_rejected_submits_counter (counter)``

**Labels**

-  instance: IP address of the miner
-  connection_type: type of the connection, which could be either *user* or *dev-fee*
-  host: URL of the host, usually URL of the pool or proxy
-  protocol: mining protocol
-  user: usually mining pool username of the client
-  worker: name of the worker

**Examples**

Average number of rejected submits per second over last 20 seconds for all instances by target:

.. code-block::

   sum by(host) (rate(stratum_rejected_submits_counter[20s]))


Temperature
-----------

Every available temperature sensor will provide the data. There might be sensor at different locations (pcb or chip).

``temperature``

**Labels**

-  instance: IP address of the miner
-  chip_addr
-  chip_in_domain
-  voltage_domain
-  hashboard
-  location: chip|pcb

**Examples**

Average maximum temperature across all instances (miners):

.. code-block::

   avg(max by (instance) (temperature))

Average maximum temperature across all instances (miners) by miner type:

.. code-block::

   avg by (model) (
     (max by (instance) (temperature)) * on (instance)
     group_left(model) count by (instance, model) (miner_metadata)
   )

Tuner stage
-----------

Stage of the tuner:

-  2: testing performance profile
-  3: tuning individual chips
-  4: stable
-  6: manual configuration running

``tuner_stage``

**Labels**

-  instance: IP address of the miner
-  hashboard: rank of the hashboard

**Examples**

Number of instances by stage:

.. code-block::

   count_values ("Stage", max by (instance) (tuner_stage))

Other Examples
--------------

**Extracting parts of IP address**

If you are managing your farm by assigning different IP ranges to different parts of your farm, grouping metrics by octet of IP address might be useful. Example for maximum chip temperature by 3rd octet:

.. code-block::

   max by (segment) (label_replace(
     temperature{location="chip"}, "segment", "$1", "instance","\\d+\\.\\d+\\.(\\d+)\\.\\d+.*"
   ))

If you need to do this for many/all metrics, it is better to have parts of the IP address as custom labels. See the Configuration section with an example.

Getting Help
============

For more information about Prometheus and Grafana, please refer to the official documentation:

-  `Prometheus Documentation <https://prometheus.io/docs/introduction/overview/>`__
-  `Grafana Documentation <https://grafana.com/docs/>`__

In case you have questions that are specific to monitoring of Braiins OS+ miners with Prometheus and Grafana, please contact our support team on Telegram.
