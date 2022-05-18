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

Braiins Farm Proxy is a free, standalone application which is run locally onsite at the farm. The aim of the proxy is to conserve bandwidth while aggregating SHA256 hashrate from individual workers and routing it to the target destinations, which are usually mining pools. Workers are configured to connect to the proxy. Braiins Farm Proxy can be set up to connect to several target pool(s) with backup pool(s) as a failover. The proxy optimizes the pool IP addresses and selects the endpoints with lowest latency or packet loss. It is also able to encrypt the pool connection for better privacy and security in case Slush Pool is used as an endpoint.

********
特性
********

 * **Bandwidth conservation** due to the hashrate aggregation and therefore higher upstream `share difficulty <https://braiins.com/blog/bitcoin-mining-pools-luck-shares-estimated-hashrate>`_, resulting in fewer data transfers to submit the same amount of shares.

 * **Network optimization** based on a Braiins Farm Proxy component which constantly monitors connection quality and automatically directs hashrate to the optimal performing endpoint.

 * Braiins Farm Proxy can be used with **any pool** for bitcoin mining.

 * **Automatic switch** to the backup endpoint if the target pool stops responding.

 * **Regular monitoring** in Grafana dashboard bundled in the Braiins Farm Proxy, with the possibility to build your own custom monitoring solution via a **monitoring API**.
 * Braiins OS+ users can benefit from **dev fee hashrate aggregation** for more bandwidth savings. Braiins Farm Proxy can aggregate both the farm’s hashrate and dev fee hashrate.
 * If the target endpoint is SlushPool, an **encrypted connection** is supported to ensure data privacy and protection from hashrate hijacking.
 * Braiins Farm Proxy is completely **free** software.
