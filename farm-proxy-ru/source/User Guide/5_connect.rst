
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###########
Подключение
###########

.. contents::
  :local:
  :depth: 2

Braiins Farm Proxy поддерживает нисходящие соединения через незащищенный Stratum V1, защищенный Stratum V1 и защищенный Stratum V2 в режиме только заголовков. Для восходящего потока используются только соединения V1 (из-за ограничения задания для V2, состоящего только из заголовков). Защищенный V1 используется для devfee, незащищенный V1 — для хешрейта клиента. Поэтому Braiins Farm Proxy имеет защищенное хранилище сертификатов.
