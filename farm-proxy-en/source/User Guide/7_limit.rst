
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###########
Limitations
###########

.. contents::
  :local:
  :depth: 2

The first version of Braiins Farm Proxy has a couple of known limitations which will be resolved in the future versions.

 1.  **Linux 64bit** and **ARM 64bit and 32bit** are supported,
 2.  Minimum HW requirement is **Raspberry Pi 3**,
 3.  Braiins Farm Proxy supports only **1 routing domain**. It can limit a farm with broad needs for several routing rules. This limitation can be mitigated by using several Braiins Farm Proxy instances. For example, if you have multiple locations or need to send different hashrate to different pools, you need to use Braiins Farm Proxy for each of the locations/destinations. Of course, you can combine both approaches,
 4.  Braiins Farm Proxy contains a certificate which is used for the dev fee hashrate aggregation. Currently there is no automatic **certificate renewal** mechanism so that a new certificate has to be renewed manually by upgrading to the latest version of Braiins Farm Proxy from Github public repository. Maximal certificate validity is 3 months,
 5.  There is no **GUI** for configuration of hashrate routing. Configuration has to be done in the config TOML file,
 6.  Tested **miners** were Antminers S9, X17, X19, Whatsminers M2x/M3x and Avalon miners with stock firmware and Braiins OS+,
 7.  There is a **tradeoff** between hashrate aggregation and monitoring of individual miners on target pool’s dashboard. This is relevant only for the case that there is used attribute *identity_pass_through = true* in the TOML config file (within [target] definition). Higher level of aggregation means that individual miners are sending submits slower due to high upstream difficulty (proxy with the high aggregation factor gets a job with a high difficulty from the target pool). In such cases individual miners can appear on the pool's dashboard as inactive. It is not a limitation of the Braiins Farm Proxy but its logical feature.