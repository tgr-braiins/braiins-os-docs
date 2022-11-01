
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
 2.  Minimum HW requirement is **Raspberry Pi 3** with 8GB memory,
 3.  Braiins Farm Proxy supports only **1 routing domain**. It can limit a farm with broad needs for several routing rules. This limitation can be mitigated by using several Braiins Farm Proxy instances. For example, if you have multiple locations or need to send different hashrate to different pools, you need to use Braiins Farm Proxy for each of the locations/destinations. Of course, you can combine both approaches,
 4.  Linux has usually **1,024** limit on the number of open files. You can check your limit with command ``ulimit -Hn``. If you plan to connect about 1,000+ miners it is needed to increase this limit, it can be done with command ``ulimit -n 10000`` (increase to 10,000 open files as an example),
 5.  There is no **GUI** for configuration of hashrate routing. Configuration has to be done in the config TOML file,
 6.  Farm Monitor dashboards, which are dedicated to monitor the whole farm in detail, support currently miners with Braiins OS+ firmware,
 7.  Tested **miners** were Antminers S9, X17, X19, Whatsminers M2x/M3x and Avalon miners with stock firmware and Braiins OS+,
 8.  It is recommended to use minimal *extranonce_size = 3* in the *server section*. From practice lower *extranonoce_size* can produce duplicate submits when hashing with **Whatsminers** ASICs running on the stock firmware. This fact is probably connected either to the version rolling or nTime rolling. In case that the farm is mining on the pool, which is supporting maximal *extranonce_size = 4* (it is defined in the *target* section), parameter *use_empty_extranonce1 = true* has to be used to be able to go achieve the server extranonce_size = 3,
 9.  There is a **tradeoff** between hashrate aggregation and monitoring of individual miners on the target pool’s dashboard. This is relevant only for cases that use  the attribute *identity_pass_through = true* in the TOML config file (within [target] definition). Higher level of aggregation means that individual miners are sending submits slower due to high upstream difficulty (proxy with the high aggregation factor gets a job with a high difficulty from the target pool). In such cases, individual miners can appear on the pool's dashboard as inactive. It is not a limitation of the Braiins Farm Proxy, but it’s a logical feature.
