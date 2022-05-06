
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

Braiins Farm proxy is a software application that accepts hashrate through multiple listening ports **[Servers]** [#f1]_ transferring it to the multiple endpoints **[Targets]** by following defined rules **[Routing goals]**. Targets are referred to a list of endpoints **[Routing goal levels]**. A collection of Servers, Routing goals and Routing goal levels is referred to as the **Routing domain**.

Connections from miners to the Braiins Farm Proxy are referred to as **Downstream** connections. Connections from Braiins Farm Proxy to a mining pool are referred to as **Upstream** connections.

Used Terminology is put into context with use of following diagrams.

**Diagram Of Hashrate Routing**

  .. |pic1| image:: ../_static/routing_diagram.png
      :width: 100%
      :alt: Routing Diagram

  |pic1|

**Diagram Interpretation**

  .. |pic2| image:: ../_static/diagram_interpretation.png
      :width: 100%
      :alt: Diagram Interpretation

  |pic2|


.. rubric:: Footnotes

.. [#f1] Servers are listening ports in terms of Braiins Farm Proxy, don’t confuse it with classical server.
