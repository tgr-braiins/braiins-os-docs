
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
 3.  Braiins矿场代理仅支持**一个布线域**. It can limit a farm with broad needs for several routing rules. This limitation can be mitigated by using several Braiins Farm Proxy instances. For example, if you have multiple locations or need to send different hashrate to different pools, you need to use Braiins Farm Proxy for each of the locations/destinations. Of course, you can combine both approaches,
 4.  Braiins Farm Proxy contains a certificate which is used for the dev fee hashrate aggregation. Currently there is no automatic **certificate renewal** mechanism so that a new certificate has to be renewed manually by upgrading to the latest version of Braiins Farm Proxy from Github public repository. Maximal certificate validity is 3 months,
 5.  算力布线配置没有**图形用户界面**。 配置需要通过TOML文件配置。
 6.  测试的**矿机**是跑原厂固件和Braiins OS+固件的蚂蚁矿机S9、S17和T17系列、S19和T19系列、神马 M2x/M3x和阿瓦隆矿机。
 7.  There is a **tradeoff** between hashrate aggregation and monitoring of individual miners on target pool’s dashboard. This is relevant only for the case that there is used attribute *identity_pass_through = true* in the TOML config file (within [target] definition). Higher level of aggregation means that individual miners are sending submits slower due to high upstream difficulty (proxy with the high aggregation factor gets a job with a high difficulty from the target pool). In such cases individual miners can appear on the pool's dashboard as inactive. It is not a limitation of the Braiins Farm Proxy but its logical feature.
