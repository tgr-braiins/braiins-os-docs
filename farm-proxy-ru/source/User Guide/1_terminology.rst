
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

############
Терминология
############

.. contents::
  :local:
  :depth: 2

Braiins Farm Proxy — это приложение, которое принимает хешрейт через несколько портов прослушивания. **[Сервера]** [#f1]_ передают данные на несколько конечных точек **[Целей]** следуя определенным правилам **[Целям маршрутизации]**. Цели относятся к списку конечных точек **[Целевым уровням маршрутизации]**. Набор серверов, целей маршрутизации и уровней целей маршрутизации называем **Доменами маршрутизации**.

Соединения майнеров с Braiins Farm Proxy называем **нисходящими** соединениями. Соединения от Braiins Farm Proxy с майнинговым пулом называем **восходящими** соединениями.

Используемая терминология помещена в контекст с использованием следующих схем.

**Схема маршрутизации хэшрейта**

  .. |pic1| image:: ../_static/routing_diagram.png
      :width: 100%
      :alt: Routing Diagram

  |pic1|

**Интерпретация схемы**

  .. |pic2| image:: ../_static/diagram_interpretation.png
      :width: 100%
      :alt: Diagram Interpretation

  |pic2|


.. rubric:: Footnotes

.. [#f1] Servers are listening ports in terms of Braiins Farm Proxy, don’t confuse it with classical server.
