
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

############
连接性
############

.. contents::
  :local:
  :depth: 2

Braiins矿场代理支持通过不安全的阶层Stratum V1协议、安全的阶层Stratum V1协议和安全的阶层Stratum V2协议在仅有头端模式下的下游连接。对于上游，只使用V1连接（由于只用头端的V2的工作限制）。安全的V1用于开发费用，不安全的V1用于客户端算力。因此，Braiins矿场代理有一个安全的证书存储。
