
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###################################
Agregación de la Tasa de Desarrollo
###################################

.. contents::
  :local:
  :depth: 2

Braiins Farm Proxy establece automáticamente la agregación de la tasa de desarrollo en caso de que Braiins OS+ se apunte a Braiins Farm Proxy desde la versión de Braiins OS+ 22.02.01. **Por lo tanto se recomienda usar con la última versión de Braiins OS+**. Para lanzamientos anteriores a 22.02.01 debe darle un consejo al equipo para agregar la tasa de desarrollo. Este consejo significa:

 * Creación del grupo pool group llamado “bos-management”

  .. |pic3| image:: ../_static/bos_management.png
      :width: 100%
      :alt: BOS Management

  |pic3|

 * Introduzca la dirección de Braiins Farm Proxy y el puerto de su servidor configurado dentro de Braiins Farm Proxy (cualquiera de los servidores). Es necesario el reinicio de los BOS miners.

  .. |pic4| image:: ../_static/pool_groups.png
      :width: 100%
      :alt: Pool Groups

  |pic4|

También es posible usar Braiins Farm Proxy puramente para la agregación de la tasa de desarrollo (y no el resto de su tasa de hash). Puede ser útil para granjas con su propio proxy de agregación pero que están corriendo Braiins OS+ en sus dispositivos. En ese caso, la configuración de Braiins Farm Proxy para el enrutamiento de la tasa de desarrollo depende de la versión de Braiins OS+:

**Braiins OS+ 22.02.01 o mas nuevo:**

1. Vaya a la configuración de cada minero y en la primera fila llene la dirección del **farm proxy propio** ``stratum+tcp://<proxy-propio>:puerto`` y en la **segunda fila llene la dirección del Braiins Farm Proxy** ``stratum+tcp://<farm-proxy>:puerto``. Trabajará como respaldo para la tasa de hash de los clientes y al mismo tiempo **será usado para la agregación de la tasa de desarrollo**.
   
  .. |pic5| image:: ../_static/devfee_aggregation.png
      :width: 100%
      :alt: Devfee Aggregation

  |pic5|

2. En el archivo de configuración de Braiins Farm Proxy coloqué el **farm proxy propio** como un punto final objetivo.

.. code-block:: shell

      [[server]]
      name = "v1"
      port = 3333

      [[target]]
      name = "Farm's own proxy"
      url = "stratum+tcp://<own-proxy>:port"
      user_identity = "userName.workerName"

      [[routing]]
      from = ["v1"]

      [[routing.goal]]
      name = "Goal 1"

      [[routing.goal.level]]
      targets = ["Farm's own proxy"]

**Braiins OS+ anterior al 22.02.01:**

1. Vaya a la configuración de cada minero, cree un grupo “bos-management” si no existe ya y **llene el grupo bos-management group con la dirección de Braiins Farm Proxy** ``stratum+tcp://<farm-proxy>:puerto``. Será usado para la agregación de la tasa de desarrollo.

2. En el archivo de configuración de Braiins Farm Proxy, coloque el **farm proxy propio** como un punto final objetivo, vea el ejemplo previo.
