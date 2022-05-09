
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###########
术语
###########

.. contents::
  :local:
  :depth: 2

Braiins矿场代理是一个软件应用程序，矿场代理通过多个监听端口**[服务器]**[#f1]_接受算力并将它转移到多个端点**[目标]**。代理按照定义的规则**[布线目标]**转移算力。目标指的是端点的列表**[布线目标级别]**。服务器、布线目标和布线目标级别的集合叫做**布线域**。

从矿机到Braiins矿场代理的连接叫做**下游**连接。从Braiins矿场代理到矿池的连接叫做**上游**连接。

使用的术语要跟以下图表参考。

**算力布线的图表**

  .. |pic1| image:: ../_static/routing_diagram.png
      :width: 100%
      :alt: Routing Diagram

  |pic1|

**图标解释**

  .. |pic2| image:: ../_static/diagram_interpretation.png
      :width: 100%
      :alt: Diagram Interpretation

  |pic2|


.. rubric:: 注脚

.. [#f1] 对于Braiins矿场代理来说，服务器指监听端口，跟一般的服务器不一样。
