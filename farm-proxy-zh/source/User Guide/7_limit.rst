
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###########
限制
###########

.. contents::
  :local:
  :depth: 2

Braiins矿场代理的第一个版本有几个已知的限制，在未来的版本中将解决。

 1.  支持**Linux 64bit** 和 **ARM 64bit and 32bit**，
 2.  最低硬件要求是**Raspberry Pi 3**,
 3.  Braiins矿场代理仅支持**一个布线域**。它可以限制一个矿场对几个布线规则的广泛需求。这种限制可以通过使用几个Braiins矿场代理实例来减轻。例如，如果您有多个地点或需要发送不同的算力到不同的矿池，您需要为每个地点/目的地使用Braiins矿场代理。当然，你可以结合这两种方法。
 4.  Braiins矿场代理包含一个证书，用于进行开发商费用算力聚合。目前没有自动的**证书更新机制**，所以必须通过升级到Github公共资源库中的Braiins矿场代理的最新版本来手动更新新的证书。最大的证书有效期是3个月。
 5.  算力布线配置没有**图形用户界面**。 配置需要通过TOML文件配置。
 6.  测试的**矿机**是跑原厂固件和Braiins OS+固件的蚂蚁矿机S9、S17和T17系列、S19和T19系列、神马 M2x/M3x和阿瓦隆矿机。
 7.  在目标矿池的仪表盘上，算力聚合和监控单个矿工之间存在**有得必有失的情况**。这只适用于在TOML配置文件中使用*identity_pass_through = true*属性的情况（在[目标]定义中）。较高的聚合水平意味着单个矿工由于上游的高难度（具有高聚合系数的代理从目标矿池中获得高难度的作业），发送提交的速度较慢。在这种情况下，单个矿工会在矿池的仪表板上显示为不活动状态。这不是Braiins矿场代理的限制，而是其逻辑功能。
