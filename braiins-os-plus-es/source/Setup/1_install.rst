###########
Instalación
###########

.. contents::
	:local:
	:depth: 1

*********
Empezando
*********

Este documento es una guía de inicio-rápido sobre como instalar Braiins OS+ en su dispositivo minero. Hay dos formas de probar y usar Braiins OS+:

  1. **Arranque por tarjeta SD** con la imagen de Braiins OS+, manteniendo el firmware de serie efectivamente en la memoria interna flash. En
     caso de encontrar algún problema, puede simplemente arrancar con el firmware de serie desde la memoria interna. Este es un método seguro
     que sugerimos para empezar.

  2. **Escritura permanente del firmware de serie** efectivamente reemplaza el firmware del fabricante completamente con Braiins OS+. En este
     método, la única manera de volver a la disposición anterior es restaurado el firmware del fabricante desde un respaldo que se crea durante
     la instalación o escribiendo un firmware de fábrica.

Debido a las razones mencionadas, es altamente recomendable instalar el firmware Braiins OS+ **solo en dispositivos con ranura para tarjeta SD**.

Para comenzar a minar usando Braiins OS+ y BOSminer+ necesitará:

 * tener un minero ASIC soportado
 * obtener la última versión de Braiins OS+
 * Instalar Braiins OS+
 * configurar Braiins OS+ y comenzar a minar

*Nota: los comandos usados en este manual son con propósito instructivo. Podría necesitar ajustar las rutas y nombres de archivo apropiadamente.*

*********************************
Guía de Instalación/Actualización
*********************************

Para una mejor navegación entre las diferentes rutas de instalación/actualización, use la siguiente guía:

 * **De serie -> Braiins OS+ (última versión)** - Siga la guía en las secciones :ref:`sd_card_method` o :ref:`remote_ssh_method` abajo
 * **Braiins OS (versión anterior) -> Braiins OS+ (última versión)** - Siga esta sección de la guía de actualización :ref:`upgrade_community_bos_plus`
 * **Braiins OS (versión anterior) -> Braiins OS Edición Comunitaria (última versión)** Siga esta sección de la guía de actualización :ref:`downgrade_bos_plus_community`
 * **Braiins OS Edición Comunitaria (última versión) -> Braiins OS+ (última versión)** Siga esta sección de la guía de actualización :ref:`upgrade_community_bos_plus`
 * **Braiins OS+ (última versión) -> Braiins OS Edición Comunitaria (última versión)** Siga esta sección de la guía de actualización :ref:`downgrade_bos_plus_community`
 * **Braiins OS+ -> De serie** - Siga esta sección de la guía de actualización :ref:`downgrade_bos_stock`

.. _sd_card_method:

*****************
Método tarjeta SD
*****************

 * Descargue la última imagen de firmware transicional liberada desde nuestra `página web <https://braiins-os.com/>`_.
   Puede verificar las firmas usando la llave pública
   `disponibles aquí. <https://slushpool.com/media/download/braiins-os.gpg.pub>`_
 * Escriba la imagen descargada a una tarjeta SD (ej: usando `Etcher <https://etcher.io/>`_).
 * Ajuste los jumpers para arrancar desde la tarjeta SD (en lugar de la memoria NAND), como se muestra abajo.

	.. |pic1| image:: ../_static/s9-jumpers.png
	    :width: 45%
	    :alt: Jumpers S9

	.. |pic2| image:: ../_static/s9-jumpers-board.png
	    :width: 45%
	    :alt: Tarjeta de Jumpers S9

	|pic1|  |pic2|

 * Inserte la tarjeta SD en el dispositivo, luego inicie el dispositivo.
 * Un momento después, podrá acceder la interfaz de Braiins OS+ a través de la dirección IP del dispositivo.

**Utilizar una sola tarjeta SD en múltiples dispositivos**

La dirección MAC de uso mas reciente es almacenada en la partición capa de
la tarjeta SD para verificar si la SD se ha insertado en el mismo
dispositivo. Si la dirección MAC difiere de la anterior, entonces la red y
configuración del sistema son reiniciadas a la por defecto y se borra
``/etc/miner_hwid``.

HW_ID se determina desde la NAND si almacena firnware Braiins OS. Si la NAND está
corrompida o contiene firmware de serie, entonces el archivo ``/etc/miner_hwid`` se
usa si existe, de lo contrario un nuevo HW_ID es generado y almacenado en
``/etc/miner_hwid`` para preservar el HW_ID hasta el siguiente arranque.

Escribir Braiins OS+ desde la tarjeta sd a la memoria interna (NAND)
====================================================================

También es posible instalar Braiins OS+ en la memoria interna (NAND) mientras está ejecutando el firmware desde la
tarjeta SD. Para escribir Braiins OS+ de manera permanente a la NAND, conéctese al minero vía SSH y use el
comando siguiente:

::

  miner nand_install

.. _remote_ssh_method:

*******************
Método (SSH) Remoto
*******************

La instalación de Braiins OS+ usando el llamado *Método SSH* consiste en los siguientes pasos:

 * *(Firmware Personalizado)* Escribir firmware de serie (este paso puede omitirse si el dispositivo está corriendo el firmware de serie o una versión previa de Braiins OS).
 * *(Solo Windows)* Instalar *Ubuntu para Windows 10* disponible desde la Tienda Microsoft `aquí. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
 * Corra los siguientes comandos en su terminal de línea de comandos (reemplace ``DIRECCIÓN_IP`` por la correspondiente) :

*(Note que los comandos son compatibles con Ubuntu y Ubuntu para Windows 10. Si está usando una distribución diferente de Linux o un sistema operativo distinto, por favor verifique la documentación correspondiente y edite los comandos según sea necesario.)*

::

  # Preparar el ambiente y descargar el firmware (este paso puede omitirse si ya se ha hecho antes)
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv
  wget -c https://feeds.braiins-os.com/20.03/braiins-os-plus_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46.tar.gz -O - | tar -xz && cd ./braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate && python3 -m pip install -r requirements.txt && deactivate
  
  # Instalar Braiins OS+ en el dispositivo
  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 upgrade2bos.py DIRECCIÓN_IP

******************************************
Instalar/Actualizar dispositivos múltiples
******************************************

En caso que necesita realizar la instalación o actualización a múltiples dispositivos,
puede usar nuestra hoja de cálculo de configuración que generará comandos para casos
distintos.

La hoja de cálculo está disponible `aquí <https://docs.google.com/spreadsheets/d/1H3Zn1zSm6-6atWTzcU0aO63zvFzANgc8mcOFtRaw42E>`_
