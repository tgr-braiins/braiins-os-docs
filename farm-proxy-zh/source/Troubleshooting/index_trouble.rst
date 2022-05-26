
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###############
疑难解答
###############

.. toctree::
   :maxdepth: 3
   :glob:

   *

**下游连接的问题**

如果连接到Braiins矿场代理时出现问题，BOSminer可能需要一段时间才能重新连接。问题持续的时间越长，重试的时间就越长。重试逐渐减速：立即重试，如果失败，1秒后重试，2秒后失败，4秒后失败...... 甚至到一个小时。在这种情况下，解决方案就是重启BOSminer，或者等待。


**Raspberry Pi**

说到Raspberry Pi资源，Prometheus和Grafana的要求较高。为了让代理顺利运行，建议从 *docker-compose.yml* 中删除Prometheus和Grafana，并在别的设备上运行。

**已拒绝算力**

如果从下游或上游检测到被拒绝的算力，请联系到Braiins客服团队。 为了解决问题，请给我么客服团队提供Grafana "Debug Dashboard "的消息和日志。
