.. toctree::
   :hidden:
   :maxdepth: 3
   :caption: Table of Contents:
   :glob:

   self
   Setup/index*
   User Guide/index*
   Troubleshooting/index*
   Support/index*

---------------

.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>


############
介绍
############

Braiins矿场代理是一个免费的、独立的工具，在矿场本地运行。代理的目的是节约带宽，同时在每个矿工上聚合SHA256算力，并将其路由到目标目的，就是矿池。矿工被配置为连接到代理。Braiins矿场代理可以被设置为连接到几个目标矿池，并有备份矿池作为故障转移。代理优化矿池的IP地址，并选择具有最低延迟或数据包丢失的端点。如果使用Slush Pool矿池作为端点，代理还能对矿池的连接进行加密，以提高隐私和安全。

********
特性
********

 * **带宽节约** 由于算力聚合，因此上游的 `份额难度 <https://braiins.com/blog/bitcoin-mining-pools-luck-shares-estimated-hashrate>`_ 较高，导致提交相同数量的份额的数据传输较少。

 * **网络优化** 基于Braiins矿场代理的一个组件，该组件不断监测连接质量，并自动将算力引导到最佳性能的终端。

 * Braiins矿场代理跟任何 **比特币矿池** 兼容。

 * 如果目标矿池停止响应，代理 **自动切换** 到备份终端。

 * Braiins矿场代理包含包含Grafana仪表盘中的 **常规监控**，让您通过 **监控API** 建立您自己的定制监控解决方案。

 * Braiins OS+固件用户可以享受 **开发商费用算力聚合**，以节省更多的带宽。Braiins矿场代理可以同时聚合矿场的算力和开发商费用的算力。
 * 如果目标终端是Slush Pool矿池， 那就支持 **加密连接** 来保证数据隐私并防止算力劫持。
 * Braiins矿场代理是完全 **免费的** 软件。
