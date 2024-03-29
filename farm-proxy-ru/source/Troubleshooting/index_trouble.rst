
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

##############
Поиск проблемы
##############

.. toctree::
   :maxdepth: 3
   :glob:

   *

**Проблемы с подключением**

Если есть проблема с подключением к Braiins Farm Proxy, BOSminer может занять некоторое время для повторного подключения. Чем дольше сохраняется проблема, тем больше времени требуется для повторных попыток. Замедление выглядит так: повтор попытки немедленно, если сбой, повтор попытки через 1 секунду, если сбой - через 2 секунды, если сбой - через 4 секунды ... Между повторами может пройти аж до 1 часа. В таких случаях решение либо перезапустить BOSminer, либо подождать.

**Raspberry Pi**

Prometheus и Grafana довольно требовательны к ресурсам Raspberry Pi. Для бесперебойной работы прокси рекомендуется удалить Prometheus и Grafana из *docker-compose.yml* и запустить это на другом устройстве.

**Отклоненный хэшрейт**

В случае обнаружения отклонения хэшрейта от нижестоящего или восходящего потока, обратитесь в службу поддержки Braiins. Для решения проблем приложите к запросу информацию из Grafana «Debug Dashboard» - логи окажут большую помощь нашей команде поддержки.
