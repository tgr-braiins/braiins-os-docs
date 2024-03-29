
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#####################
Solución de Problemas
#####################

****************************************************************
Recuperar Dispositivos Bloqueados (no arranca) Usando tarjeta SD
****************************************************************

Si algo sale mal y su dispositivo parece no arrancar, puede usar la
imagen de la tarjeta SD creada previamente para recuperar el firmware
original del fabricante usando los siguientes comandos:

.. code:: bash

   cd braiins-os_am1-s9_ssh_VERSIÓN
   source .env/bin/activate
   python3 restore2factory.py backup/2ce9c4aab53c-2018-09-19/ ip-o-nombre-host-de-su-minero

Luego de que el script finalice, espere unos minutos y ajuste el
jumper para arrancar desde NAND (memoria interna).

*********************
Operación de BOSminer
*********************

Bosminer puede controlarse desde la línea de comandos o vía página web.

Para iniciar o detener BOSminer, use los siguientes comandos:

::

	/etc/init.d/bosminer stop
	/etc/init.d/bosminer start

Alternativamente, BOSminer puede controlarse en la página `System -> Startup` y se reinicia
cada vez que el usuario pulsa el botón `Save & Apply` en la página `Miner -> Configuration`.
