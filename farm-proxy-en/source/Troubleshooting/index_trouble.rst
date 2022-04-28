
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###############
Troubleshooting
###############

.. toctree::
   :maxdepth: 3
   :glob:

   *

**Problems With Downstream Connectivity**

If there is a problem connecting to Braiins Farm Proxy, the BOSminer can take a while to reconnect. The longer the problem persists, the longer it takes for the retries. It has graceful slowdown: retry immediately, if fail, retry after 1 second, if fail after 2 second, if fail after 4 seconds... It can go all the way to 1 hour between retries. In such cases the solution is either restart BOSminer or wait.

**Raspberry Pi**

Prometheus and Grafana are quite demanding when it comes to Raspberry Pi resources. For the sake of the proxy running smoothly, it is advised to remove Prometheus and Grafana from the *docker-compose.yml* and run it on a different device.

**Rejected Hashrate**

In case of detecting rejected hashrate from downstream or upstream, please contact Braiins support. To solve the issues, information from Grafana “Debug Dashboard” and logs will provide much help to our support team.