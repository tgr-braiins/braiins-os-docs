
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

################
Агрегация DevFee
################

.. contents::
  :local:
  :depth: 2

Braiins Farm Proxy автоматически рассчитывает агрегирование DevFee, если Braiins OS+ нацелена на Braiins Farm Proxy, начиная с Braiins OS+ версии 22.02.01. **Поэтому рекомендуется использовать последнюю версию Braiins OS+**. Для релизов старше 22.02.01 вы должны подсказать воркеру, как агрегировать DevFee. Этот означает:

 * Создание группы пулов с именем “bos-management”

  .. |pic3| image:: ../_static/bos_management.png
      :width: 100%
      :alt: BOS Management

  |pic3|

 * Введите URL-адрес прокси-сервера Braiins Farm и порт настроенного сервера в прокси-сервере Braiins Farm (любой из серверов). Требуется перезапуск майнеров BOS.

  .. |pic4| image:: ../_static/pool_groups.png
      :width: 100%
      :alt: Pool Groups

  |pic4|

Также можно использовать Braiins Farm Proxy исключительно для агрегации DevFee (а не остальной части вашего хешрейта). Это может быть полезно для ферм с собственным прокси-агрегатором, но на устройствах с Braiins OS+. В таком случае настройка Braiins Farm Proxy только для маршрутизации DevFee зависит от версии Braiins OS+.:

**Braiins OS+ 22.02.01 и новее:**

1. Перейдите к конфигурации каждого майнера и в первой строке введите URL-адрес **собственного прокси фермы** ```stratum+tcp://<own-proxy>:port``, а во **второй строке введите URL-адрес прокси фермы Braiins** ``stratum+tcp://<farm-proxy>:port``. Он будет работать как резервный для хешрейта клиентов и в то же время **будет использоваться для агрегации devfee**.
   
  .. |pic5| image:: ../_static/devfee_aggregation.png
      :width: 100%
      :alt: Devfee Aggregation

  |pic5|

2. В файле конфигурации Braiins Farm Proxy настройте **собственный прокси фермы** в качестве целевой конечной точки.

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

**Braiins OS+ старше 22.02.01:**

1. Перейдите к конфигурации каждого майнера, создайте группу «bos-management», если она еще не существует, и **заполните группу bos-management URL-адресом Braiins Farm Proxy** ``stratum+tcp://<farm-proxy>:port``. Он будет использоваться для агрегации devfee.

2. В файле конфигурации Braiins Farm Proxy настройте **собственный прокси-сервер фермы** в качестве целевой конечной точки, см. предыдущий пример.
