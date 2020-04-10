#######################################
Actualizar, Desactualizar y Desinstalar
#######################################

.. contents::
	:local:
	:depth: 2

.. _upgrade_bos:

*******************
Actualizar Firmware
*******************

El proceso de actualización de firmware usa un mecanismo estandar para
instalar/actualizar paquetes de software dentro de cualquier sistema
basado en OpenWrt.
Siga los pasos abajo para realizar la actualización de firmware.

Actualizar vía interfaz web
===========================

El firmware revisa periódicamente la disponibilidad de una nueva versión y actualiza automáticamente el
sistema. En caso de que la característica de auto-actualizar esté deshabilitada, un botón azul **Upgrade**
aparecerá en el lado derecho de la barra superior. Proceda pulsando el botón y confirme para iniciar la
actualización.

Alternativamente, puede actualizar la información del repositorio
manualmente al pulsar el botón *Update lists* en el menú
System > Software. En caso de que falte el botón, intente refrescar
la página. Para activar el proceso de actualización, escriba
``firmware`` en el campo *Download and install package* y pulse
*OK*.

Actualizar vía SSH
==================

Luego de conectarse al minero vía SSH, la actualización al último firmware puede activarse usando los siguientes comandos:

::

  opkg update && opkg install firmware

Ya que la instalación de firmware resulta en un reinicio, la
siguiente salida es esperada:

::

  ...
  Collected errors:
  * opkg_conf_load: Could not lock /var/lock/opkg.lock: Resource temporarily unavailable.
    Saving config files...
    Connection to 10.10.10.1 closed by remote host.
    Connection to 10.10.10.1 closed.

.. _upgrade_community_bos_plus:

************************
Actualizar a Braiins OS+
************************

Para actualizar desde una versión previa o la Edición Comunitaria a Braiins OS+, conéctese al minero vía SSH
y use los siguientes comandos:

::

    opkg update && opkg install bos_plus

.. _downgrade_bos_plus_community:

*************************************************
Actualizar/Desactualizar a la Edición Comunitaria
*************************************************

Para actualizar desde una versión previa de Braiins OS o desactualizar desde Braiins OS+, conéctese al
minero vía SSH y use el siguiente comando (reemplace ``DIRECCIÓN_IP`` según corresponda):

::

  ssh root@DIRECCIÓN_IP 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-03-29-0-6ec1a631_arm_cortex-a9_neon.tar && sysupgrade -F /tmp/firmware.tar'

.. _downgrade_bos_stock:

********************************************
Reiniciar a la versión inicial de Braiins OS
********************************************

El paquete de firmware actual puede desactualizarse a la versión que fue inicialmente instalada
durante el remplazo del firmware de serie. Esto puede hacerse mediante el uso de

 -  *botón IP SET* - mantenga presionado por *10s* hasta que el LED rojo parpadee
 -  *tarjeta SD* - edite el archivo *uEnv.txt* para que contenga la línea **factory_reset=yes**
 -  *utilidad miner* - invoque ``miner factory_reset`` desde la línea de comandos del minero
    (al mantener conexión vía ssh)
 -  *paquete opkg* - invoque ``opkg remove firmware`` desde la línea de comandos del minero
    (al mantener conexión vía ssh)

****************************
Escribir firmware de fábrica
****************************

Usar respaldo creado previamente
================================

Por defecto, un respaldo del firmware original es creado durante la
migración a Braiins OS y puede ser restaurado usando los siguientes comandos (reemplace ``RESPALDO_ID_FECHA`` and ``DIRECCIÓN_IP``según corresponda):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-0-6ec1a631 && source .env/bin/activate
  python3 restore2factory.py backup/RESPALDO_ID_FECHA/ DIRECCIÓN_IP

Usando imagen firmware de fábrica
=================================

En un Antminer S9, puede alternativamente escribir una imágen firmware
de fábrica de la página web del fabricante, con ``IMAGEN_DE_FÁBRICA``
siendo la ruta al archivo o URL al archivo ``tar.gz`` (¡sin extraer!).
Las imágenes soportadas y sus correspondientes hashes md5 se listan en
el archivo `platform.py <https://github.com/braiins/braiins-os/blob/master/upgrade/am1/platform.py>`__.

Corra (reemplace ``IMAGEN_DE_FÁBRICA`` y ``DIRECCIÓN_IP`` según corresponda):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-0-6ec1a631 && source .env/bin/activate
  python3 restore2factory.py --factory-image IMAGEN_DE_FÁBRICA DIRECCIÓN_IP
