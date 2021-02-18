#############
Guía Avanzada
#############

.. contents::
	:local:
	:depth: 1

************
Mapa de ruta
************

Hay muchas herramientas, paquetes y scripts que pueden usarse para gestionar Braiins OS+. Use el siguiente árbol para una mejor navegación:

 * Instalar Braiins OS+

  * Usando la Caja de herramientas BOS (:ref:`bosbox_install`)
  * Usando el paquete web (:ref:`web_package_install`)
  * Usando la tarjeta SD (:ref:`sd_install`)
  * Usando la tarjeta SD y la herramienta miner (:ref:`miner_nand_install`)
  * Usando scripts SSH (:ref:`ssh_package_install`)

 * Desbloquear SSH en Antminer S9

  * Usando la Caja de Herramientas BOS (:ref:`bosbox_unlock`)

 * Actualizar Braiins OS+

  * Usando la Caja de herramientas BOS (:ref:`bosbox_update`)
  * Usando OPKG (:ref:`opkg_update`)
  * Usando el paquete sysupgrade (:ref:`sysupgrade_switch_braiinsplus`)
  * Usando el script bos2bos (:ref:`bos2bos`)

 * Cambiar a Braiins OS (versión sin autoajuste)

  * Usando el paquete sysupgrade (:ref:`sysupgrade_switch_braiinsos`)
  * Usando el script bos2bos (:ref:`bos2bos`)

 * Cambiar a Braiins OS+ (versión con autoajuste)

  * Usando OPKG (:ref:`opkg_switch_to_braiinsplus`)
  * Usando el paquete sysupgrade (:ref:`sysupgrade_switch_braiinsplus`)
  * Usando el script bos2bos (:ref:`bos2bos`)

 * Restablecer a la versión Braiins OS inicial (la versión que instaló en su dispositivo por primera vez) - restablecimiento de fabrica

  * Usando OPKG (:ref:`opkg_factory_reset`)
  * Usando la tarjeta SD (:ref:`sd_factory_reset`)
  * Usando la herramienta "miner" (:ref:`miner_factory_reset`)
  * Usando el script bos2bos (:ref:`bos2bos`)

 * Desinstalar Braiins OS+

  * Usando la Caja de herramientas BOS (:ref:`bosbox_uninstall`)
  * Usando scripts SSH (:ref:`ssh_package_uninstall`)

 * Encender/apagar alimentaciones nocturnas

  * Usando la herramienta "miner" (:ref:`miner_nightly`)

 * Encender/apagar auto-actualizar

  * Usando la herramienta "miner" (:ref:`miner_autoupgrade`)

 * Correr comandos personalizados de consola en el minero

  * Usando la Caja de Herramientas BOS (:ref:`bosbox_command`)

.. _bosbox:

************************
Caja de herramientas BOS
************************

La Caja de herramientas BOS es una nueva herramienta que permite a los usuarios instalar, desinstalar, actualizar, detectar, configurar Braiins OS+ y correr comandos personalizados fácilmente. También permite que los comandos sean ejecutados en modo por lotes, lo que hace mas fácil la gestión de un gran número de dispositivos. La caja de herramientas BOS también descarga automáticamente el último firmware. Esta es la manera recomendada de gestionar sus máquinas.

===
Uso
===

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/>`_.
  * Cree un nuevo archivo de texto, cambie la extensión ".txt" a ".csv" e inserte las direcciones IP en las que desea ejecutar los comandos. Coloque el archivo en el directorio donde se encuentra la Caja de herramientas BOS. **¡Use solo una dirección IP por línea!**
  * Siga las instrucciones abajo

============================================
Características, PROs y CONs de este método:
============================================

  + instala Braiins OS+ remotamente y automáticamente desbloquea SSH en Antminer S9 durante la instalación
  + actualiza Braiins OS+ remotamente
  + desinstala Braiins OS+ remotamente
  + configura Braiins OS+ remotamente
  + busca las máquinas en la red
  + corre comandos personalizados en las máquinas
  + por defecto migra toda la configuración (puede ajustarse) al instalar Braiins OS+
  + por defecto migra la configuración de red (puede ajustarse) al desinstalar Braiins OS+
  + hay parámetros disponibles para personalizar el proceso
  + activa el autoajuste al límite de energía por defecto al instalar Braiins OS+
  + modo-por-lotes disponible para gestionar múltiples dispositivos a la vez
  + fácil de usar

  - no funciona en dispositivos X17 con SSH bloqueado

.. _bosbox_install:

=====================================================
Instalar Braiins OS+ con la Caja de herramientas BOS
=====================================================

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/plus/download/>`_.
  * Cree un nuevo archivo de texto, cambie la extensión ".txt" a ".csv" e inserte las direcciones IP en las que desea ejecutar los comandos. Coloque el archivo en el directorio donde se encuentra la Caja de herramientas BOS. **¡Use solo una dirección IP por línea!**
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.bat install listaDeMineros.csv

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox install listaDeMineros.csv

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los **argumentos** siguientes para ajustar el proceso:

**Nota importante:**
Al instalar Braiins OS+ en **un solo dispositivo**, reemplace el argumento *HOSTS* con una dirección IP.
Al instalar Braiins OS+ en **varios dispositivos**, reemplace el argumento *HOSTS* con la ruta al archivo que contiene las direcciones ip.

====================================  ==================================================================
Argumentos                            Descripción
====================================  ==================================================================
-h, --help                            muestra este mensaje de ayuda y sale
--open-source         		         use para instalar la versión de código abierto (excluyente con **nightly** y **feed-url**)
--nightly             		         use para instalar la versión nightly (excluyente con **open-source** y **feed-url**)
--feeds-url [URL_FEEDS]		         saltar URL del servidor predeterminado de feeds (excluyente con **open-source** y **nightly**)
--fw-version [VERSION_FW]	         seleccionar versión específica de firmware
--backup                              hacer el respaldo al minero antes de actualizar
--no-auto-upgrade                     apagar auto-actualizar del firmware instalado
--no-nand-backup                      saltar respaldo completo NAND (la configuración aun se respalda)
--pool-user [USUARIO_POOL]            establecer nombre de usuario y equipo al pool predeterminado
--psu-power-limit [LÍMITE_ENERGÍA]    fijar límite de energía (en vatios) para la fuente de poder
--no-keep-network                     no mantener la configuración de red (usar DHCP)
--no-keep-pools                       no mantener la configuración del pool del minero
--no-keep-hostname                    no mantener el nombre de host y generar uno nuevo basado en MAC
--keep-hostname                       forzar mantener cualquier nombre host del minero
--no-wait                             no esperar a que el sistema esté completamente actualizado
--dry-run                             hacer todos los pasos de actualización sin realmente actualizar
--post-upgrade [POST_ACTUALIZADO]     ruta al directorio con el script stage3.sh
--bos-mgmt-id [ID_GESTOR_BOS]         fijar identificador para el gestor BOS
-p CONTRASEÑA --password CONTRASEÑA   contraseña administrativa
====================================  ==================================================================

**Ejemplo:**

::

  bos-toolbox.bat install --psu-power-limit 1200 --password root listaDeMineros.csv

Este comando instalará Braiins OS+ en los mineros, que estén especificados en el archivo *listaDeMineros.csv* y fija el límite de energía a 1200 en todos ellos. El comando también desbloqueará automáticamente la Antminer S9.

.. _bosbox_update:

=======================================================
Actualizar Braiins OS+ con la Caja de herramientas BOS
=======================================================

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/plus/download/>`_.
  * Cree un nuevo archivo de texto, cambie la extensión ".txt" a ".csv" e inserte las direcciones IP en las que desea ejecutar los comandos. Coloque el archivo en el directorio donde se encuentra la Caja de herramientas BOS. **¡Use solo una dirección IP por línea!**
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.bat update ARGUMENTOS HOSTS PAQUETE

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox update ARGUMENTOS HOSTS PAQUETE

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los **argumentos** siguientes para ajustar el proceso:

**Nota importante:**
Al actualizar Braiins OS+ en **un solo dispositivo**, use el argumento *NOMBREHOST* (dirección IP).
Al actualizar Braiins OS+ en **varios dispositivos**, **NO** use el argumento NOMBREHOST, sino el argumento *--batch LOTE* en su lugar.

====================================  ==================================================================
Argumentos                            Descripción
====================================  ==================================================================
-h, --help                            muestra este mensaje de ayuda y sale
-p PASSWORD, --password PASSWORD      palabra clave administrativa
-i, --ignore                          no detener en errores
PAQUETE                               nombre del paquete para actualizar
====================================  ==================================================================


**Ejemplo:**

::

  bos-toolbox.bat update listaDeMineros.csv

Este comando buscará actualizaciones para los mineros, que están especificados en la *listaDeMineros.csv* y los actualizará si hay una nueva versión del firmware.

.. _bosbox_uninstall:

========================================================
Desinstalar Braiins OS+ con la Caja de herramientas BOS
========================================================

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/plus/download/>`_.
  * Cree un nuevo archivo de texto en su editor de texto e inserte las direcciones IP en las cuales desea ejecutar los comandos. **¡Use una sola dirección IP por línea!** (Note que puede encontrar la dirección IP en la interfaz web de Braiins OS+ yendo a *Status -> Overview*.) Luego guarde el archivo en el mismo directorio donde guardó la Caja de herramientas BOS y cambie la extensión ".txt" a ".csv".
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.bat uninstall ARGUMENTOS HOSTS RUTA_DE_RESPALDO

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox uninstall ARGUMENTOS HOSTS RUTA_DE_RESPALDO

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los **argumentos** siguientes para ajustar el proceso:

**Nota importante:**
Al desinstalar Braiins OS+ en **un solo dispositivo**, reemplace el argumento *HOSTS* con una dirección IP.
Al desinstalar Braiins OS+ en **varios dispositivos**, reemplace el argumento *HOSTS* con la ruta al archivo que contiene las direcciones IP.
*RUTA_DE_RESPALDO* es opcional. Use solo en conjunto con el argumento *--nand-restore*.

====================================  ===================================================================
Argumentos                            Descripción
====================================  ===================================================================
-h, --help                            muestra este mensaje de ayuda y sale
-p PASSWORD, --password PASSWORD      palabra clave administrativa
--feeds-url [URL_FEEDS]               saltar URL del servidor predeterminado de feeds
--nand-restore                        usar restauración completa NAND desde un respaldo previo
RUTA_DE_RESPALDO                      ruta al directorio o archivo tgz con datos para restaurar el minero
====================================  ===================================================================

**Ejemplo:**

::

  bos-toolbox.bat uninstall listaDeMineros.csv

Este comando desinstalará Braiins OS+ de los mineros, que están especificados en el archivo *listaDeMineros.csv* e instala un firmware de serie.

**¡Advertencia!** El firmware de serie que se instala al desinstalar Braiins OS+ ¡no es adecuado para minar! Actualice a una versión mas nueva del firmware de serie para su modelo de hardware específico antes de comenzar a minar.

.. _bosbox_configure:

=======================================================
Configurar Braiins OS+ con la Caja de herramientas BOS
=======================================================

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/plus/download/>`_.
  * Cree un nuevo archivo de texto en su editor de texto e inserte las direcciones IP en las cuales desea ejecutar los comandos. **¡Use una sola dirección IP por línea!** (Note que puede encontrar la dirección IP en la interfaz web de Braiins OS+ yendo a *Status -> Overview*.) Luego guarde el archivo en el mismo directorio donde guardó la Caja de herramientas BOS y cambie la extensión ".txt" a ".csv".
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.bat config ARGUMENTOS ACCIÓN TABLA

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox config ARGUMENTOS ACCIÓN TABLA

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los **argumentos** siguientes para ajustar el proceso:

====================================  ==================================================================
Argumentos                            Descripción
====================================  ==================================================================
-h, --help                            muestra este mensaje de ayuda y sale
-u USER, --user USER                  nombre administrativo
-p PASSWORD, --password PASSWORD      palabra clave administrativa o "preguntar"
--change-password                     Permite cambiar contraseña (a una puesta en *listaDeMineros.csv*)
-c, --check                           ensayo sin escrituras
-i, --ignore                          no detener en errores
====================================  ==================================================================

**Debe usar una** de las siguientes **acciones** para ajustar el proceso:

====================================  ==================================================================
Acciones                              Descripción
====================================  ==================================================================
load                                  cargar la configuración actual de los mineros (especificados en 
                                      el archivo CSV) e insertarla al archivo CSV
save                                  guardar la configuración desde el archivo CSV a los mineros 
                                      (esto no la aplica)
apply                                 aplicar la configuración, que fue copiada desde el archivo CSV a 
                                      los mineros
save_apply                            guardar y aplicar la configuración del archivo CSV a los mineros
====================================  ==================================================================

**Ejemplo:**

::

  bos-toolbox.bat config --user root load listaDeMineros.csv

  #edite el archivo CSV con un editor de hojas de cálculo (ej: Office Excel, LibreOffice Calc, etc.)

  bos-toolbox.bat config --user root -p admin --change-password save_apply listaDeMineros.csv

El primer comando va a cargar la configuración de los mineros, que estén especificados en la *listaDeMineros.csv* (usando el usuario *root*) y la guardará en ese archivo CSV. Ahora puede abrir el archivo y editar lo que necesite. Luego de que el archivo esté editado, el segundo comando copiará la configuración de vuelta a los mineros, la aplicará y cambiará la contraseña a la colocada en la columna contraseña.

.. _bosbox_scan:

============================================================================
Explorar la red para identificar mineros usando la Caja de herramientas BOS
============================================================================

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/plus/download/>`_.
  * Cree un nuevo archivo de texto en su editor de texto e inserte las direcciones IP en las cuales desea ejecutar los comandos. **¡Use una sola dirección IP por línea!** (Note que puede encontrar la dirección IP en la interfaz web de Braiins OS+ yendo a *Status -> Overview*.) Luego guarde el archivo en el mismo directorio donde guardó la Caja de herramientas BOS y cambie la extensión ".txt" a ".csv".
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.bat scan ARGUMENTOS

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox scan ARGUMENTOS

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los **argumentos** siguientes para ajustar el proceso:

====================================  ==================================================================
Argumentos                            Descripción
====================================  ==================================================================
-h, --help                            muestra este mensaje de ayuda y sale
====================================  ==================================================================

**Debe usar una** de las siguientes **acciones** para ajustar el proceso:

====================================  ==================================================================
Acciones                              Descripción
====================================  ==================================================================
scan                                  explorar activamente el rango provisto de direcciones
listen                                escuchar transmisión entrante desde los dispositivos (al presionar
                                      el botón IP report)
====================================  ==================================================================

**Ejemplo:**

::

  #explorar la red, en el rango 10.10.10.0 - 10.10.10.255
  bos-toolbox.bat discover scan 10.10.10.0/24

  #explorar la red, en el rango 10.10.0.0 - 10.10.255.255
  bos-toolbox.bat discover scan 10.10.0.0/16

  #explorar la red, en el rango 10.0.0.0 - 10.255.255.255
  bos-toolbox.bat discover scan 10.0.0.0/8

.. _bosbox_command:

=============================================================================
Correr comandos personalizados en mineros usando la Caja de Herramientas BOS
=============================================================================

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/plus/download/>`_.
  * Cree un nuevo archivo de texto en su editor de texto e inserte las direcciones IP en las cuales desea ejecutar los comandos. **¡Use una sola dirección IP por línea!** (Note que puede encontrar la dirección IP en la interfaz web de Braiins OS+ yendo a *Status -> Overview*.) Luego guarde el archivo en el mismo directorio donde guardó la Caja de herramientas BOS y cambie la extensión ".txt" a ".csv".
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.bat command ARGUMENTOS

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox command ARGUMENTOS

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los **argumentos** siguientes para ajustar el proceso:

====================================  ==================================================================
Acciones                              Descripción
====================================  ==================================================================
-h, --help                            muestra este mensaje de ayuda y sale
-a, --auto                            Usar ssh si rpc no está disponible
-l, --legacy                          Usar ssh
-L, --no-legacy                       Usar rpc
-o, --output                          Capturar e imprimir la salida remota
-O, --output-hostname                 Capturar e imprimir la salida remota
-p PASSWORD, --password PASSWORD      palabra clave administrativa
-j JOBS, --jobs JOBS                  número de trabajos concurrentes
====================================  ==================================================================

**Debe usar una** de las siguientes **acciones** para ajustar el proceso:

====================================  ==================================================================
Acciones                              Descripción
====================================  ==================================================================
start                                 Iniciar BOSminer
stop                                  Detener BOSminer
*comando_personalizado*               Reemplace *comando_personalizado* con su propio comando de consola
                                      (ej: *cat /etc/bosminer.toml* para mostrar el contenido del
                                      archivo de configuración *bosminer.toml*)
====================================  ==================================================================

**Ejemplo:**

::

  #detiene BOSminer, deteniendo efectivamente el minado y reduciendo el consumo de energía al mínimo
  bos-toolbox.bat command -o list.csv stop

.. _bosbox_unlock:

===============================================================
Desbloquear SSH en Antminer S9 usando Caja de Herramientas BOS
===============================================================

  * Descargue la **Caja de Herramientas BOS** de nuestro `sitio web <https://es.braiins.com/os/plus/download>`_.
  * Cree un nuevo archivo de texto, cambie la terminación ".txt" a ".csv" e inserte las direcciones IP en donde quiere ejecutar los comandos. Coloque ese archivo en la carpeta donde se encuentra la Caja de Herramientas BOS. **¡Use solo una dirección IP por línea!**
  * Una vez descargada la Caja de Herramientas BOS, abra su interprete de linea-de-comandos (ej. CMD en Windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_HACIA_LA_CAJA_DE_HERRAMIENTAS_BOS* en el comando de abajo por la ruta actual al archivo donde guardó la Caja de Herramientas BOS Toolbox. Luego cambie a esa ruta de archivo corriendo el comando: ::

      cd RUTA_HACIA_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con el nombre de archivo en el comando de abajo y corra el comando apropiado para su sistema operativo:

    Terminal de comandos en **Windows**: ::

      bos-toolbox.bat unlock ARGUMENTOS HOSTS

    Terminal de comandos en **Linux**: ::

      ./bos-toolbox unlock ARGUMENTOS HOSTS

    **Nota:** *al usar la caja de herramientas BOS para Linux, debe hacerla ejecutable con el siguiente comando (solo necesita hacerlo una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los siguientes **argumentos** para ajustar el proceso:

**Nota importante:**
Al desinstalar Braiins OS+ en **un solo dispositivo**, reemplace el argumento *HOSTS* con una dirección IP.
Al desinstalar Braiins OS+ en **varios dispositivos**, reemplace el argumento *HOSTS* con la ruta al archivo que contiene las direcciones IP.

==========================================  ==========================================================
Argumentos                                  Descripción
==========================================  ==========================================================
--h, --help                                 muestra este mensaje de ayuda y sale
-u NOMBREUSUARIO, --username NOMBREUSUARIO  nombre de usuario para la interfaz web
-p CONTRASEÑA, --password CONTRASEÑA        contraseña para la interfaz web
--port PUERTO                               puerto de la interfaz web
--ssl                                       si va usar SSL
==========================================  ==========================================================


**Ejemplo:**

::

  bos-toolbox.bat unlock -u root -p root listaDeMineros.csv

Este comando va desbloquear SSH en los mineros, que están especificados en la *listaDeMineros.csv*.

.. _web_package:

***********
Paquete Web
***********

El paquete Web puede usarse para cambiar el firmware de serie, liberado antes de 2019. También debería funcionar con otros basados en firmware de serie. Este paquete no puede usarse con firmware de serie, liberado en 2019 o posterior, debido a la verificación de firma, que fue implementada. La verificación de firma previene el uso de otro firmware que no sea firmware de serie originales.

===
Uso
===

  * Descargue el **Paquete Web** desde nuestro `sitio web <https://braiins-os.com/>`_.
  * Siga las instrucciones abajo

============================================
Características, PROs y CONs de este método:
============================================

  + reemplaza el firmware de serie con Braiins OS+ sin herramientas adicionales
  + migra la configuración de red
  + migra las direcciones (URL) de los pool, usuarios y claves
  + activa el autoajuste al límite de energía por defecto

  - no puede usarse con firmware de serie liberado en 2019 o luego
  - no puede configurar la instalación (ej: siempre migrará la configuración de red)
  - no hay modo-por-lotes (a menos que se cree sus propios scripts)

.. _web_package_install:

==========================================
Instalar Braiins OS+ usando el Paquete web
==========================================

  * Descargue el **Paquete web** desde nuestro `sitio web <https://braiins-os.com/>`_.
  * Ingrese a su minero y vaya a la sección *System -> Upgrade*.
  * Suba la imagen descargada y escriba la imagen.

.. _sd:

**********************
Imagen para tarjeta SD
**********************

Si está corriendo firmware de serie, que fue liberado en 2019 o luego, la única forma de instalar Braiins OS+ es insertar una tarjeta SD con Braiins OS+ escrito en ella. En 2019, la conexión SSH fue bloqueada y la verificación de firma en la interfaz web impide el uso de otro firmware distinto al de serie.

===
Uso
===

  * Descargue la **Imagen para tarjeta SD** desde nuestro `sitio web <https://braiins-os.com/>`_.
  * Siga las instrucciones abajo

============================================
Características, PROs y CONs de este método:
============================================

  + reemplaza el firmware de serie con SSH bloqueado con Braiins OS+
  + usa la configuración de red almacenada en la NAND (esto puede apagarse, vea la sección *Network settings* abajo)
  + activa el autoajuste al límite de energía por defecto

  - no migra direcciones (URL) de pool, usuarios o claves
  - no hay modo-por-lotes

.. _sd_install:

==============================================
Instalar Braiins OS+ usando la tarjeta SD card
==============================================

 * Descargue la **Imagen para tarjeta SD** desde nuestro `sitio web <https://braiins-os.com/>`_.
 * Escriba la imagen descargada a una tarjeta SD (ej: usando `Etcher <https://etcher.io/>`_). *Nota: Una simple copia no funcionará. ¡La tarjeta SD debe ser escrita!*
 * **(Solo Antminer S9)** Ajuste los jumpers para arrancar desde la tarjeta SD (en lugar de la memoria NAND), como se muestra abajo.

  .. |pic1| image:: ../_static/s9-jumpers.png
      :width: 45%
      :alt: S9 Jumpers

  .. |pic2| image:: ../_static/s9-jumpers-board.png
      :width: 45%
      :alt: S9 Jumpers Board

  |pic1|  |pic2|

 * Inserte la tarjeta SD card en el dispositivo, luego inicie el dispositivo.
 * Tras un momento, debe poder acceder la interfaz de Braiins OS+ a través de la dirección IP del dispositivo.

.. _sd_network:

====================
Configuración de red
====================

 Por defecto, la configuración almacenada en la NAND se utilizará, mientras esté corriendo Braiins OS+ desde una tarjeta SD. Esta característica puede apagarse, siguiendo los pasos abajo:

  * Monte la primera partición FAT de la tarjeta SD
  * Abra el archivo uEnv.txt e inserte la frase siguiente (asegúrese de que solo hay una frase por línea)

  ::

    cfg_override=no

Deshabilitar el uso de la vieja configuración de red es beneficioso para los usuarios, que tienen problemas con el minero no estar visible en la red (ej: la dirección IP estática usada en la NAND está fuera del rango de la red). Al hacerlo, se usa DHCP.

.. _sd_nand_install:

===============
Instalar a NAND
===============

La tarjeta SD puede usarse para reemplazar el firmware corriendo en la NAND con Braiins OS+. Eso se hace:
  * Añada ``nand_install=yes`` al archivo ``uEnv.txt`` para la instalación automática a NAND.
  * usando la interfaz web - sección *System -> Install current system to device (NAND)*
  * usando la herramienta *miner*, via SSH - siga esta sección de la guía :ref:`miner_nand_install`

.. _sd_factory_reset:

=======================================================
Restablecer de fábrica Braiins OS+ usando la tarjeta SD
=======================================================

Puede hacer un restablecimiento de fábrica, siguiendo los pasos abajo:

  * Monte la primera partición FAT de la tarjeta SD
  * Abra el archivo uEnv.txt e inserte la frase siguiente (asegúrese de que solo hay una frase por línea)

  ::

    factory_reset=yes

.. _ssh_package:

***********************************
Paquete de instalación remota (SSH)
***********************************

Con el *paquete de instalación remota (SSH)* puede instalar o desinstalar Braiins OS+. Este método no es recomendado, ya que requiere una instalación Python. Use la caja de herramientas BOS en su lugar.

===
Uso
===

  * Descargue el **Paquete de instalación remota (SSH)** desde nuestro `sitio web <https://braiins-os.com/>`_.
  * Siga las instrucciones abajo

============================================
Características, PROs y CONs de este método:
============================================

  + instala Braiins OS+ remotamente
  + desinstala Braiins OS+ remotamente
  + migra toda la configuración por defecto (puede ajustarse) al instalar Braiins OS+
  + migra la configuración de red por defecto (puede ajustarse) al desinstalar Braiins OS+
  + parámetros disponibles para personalizar el proceso
  + enciende el autoajuste a un límite de energía por defecto al instalar Braiins OS+

  - no hay modo-por-lotes (a menos que cree sus propios scripts)
  - requiere una larga instalación
  - no funciona en un minero con SSH bloqueado

.. _ssh_package_environment:

======================
Preparando el ambiente
======================

Primero, necesita preparar el ambiente Python. Esto consiste en los siguientes pasos:

* *(Solo Windows)* Instalar *Ubuntu para Windows 10* disponible desde la Tienda Microsoft `aquí. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Corra los siguientes comandos en su terminal de línea de comandos:

*(Note que los comandos son compatibles con Ubuntu y Ubuntu para Windows 10. Si está usando una distribución diferente de Linux o un sistema operativo diferente, por favor verifique la documentación correspondiente y edite los comandos según sea necesario.)*

::

  #Actualizar los repositorios e instalar dependencias
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv

  #Descargar y extraer el paquete de firmware
  #Antminer S9
  wget -c https://feeds.braiins-os.com/20.10/braiins-os_am1-s9_ssh_2020-10-25-0-908ca41d-20.10-plus.tar.gz -O - | tar -xz

  #Antminer S17
  wget -c https://feeds.braiins-os.com/20.11/braiins-os_am2-s17_ssh_2020-11-27-0-5eb922d4-20.11-plus.tar.gz -O - | tar -xz

  #Cambiar el directorio a la carpeta donde desempacó el firmware
  #Antminer S9
  cd ./braiins-os_am1-s9_ssh_VERSION

  #Antminer S17
  cd ./braiins-os_am2-s17_ssh_VERSION

  #Crear un ambiente virtual y activarlo
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate

  #Instalar los paquetes Python requeridos
  python3 -m pip install -r requirements.txt

.. _ssh_package_install:

==========================================
Instalar Braiins OS+ usando el paquete SSH
==========================================

La instalación de Braiins OS+ usando el asi-llamado *Método SSH* consiste en los siguientes pasos:

* *(Firmware Personalizado)* Escribir firmware de serie. Este paso puede omitirse si el dispositivo está corriendo el firmware de serie o una versión previa de Braiins OS. *(Nota: Es posible, que Braiins OS+ pueda ser instalado directamente sobre un firmware personalizado, pero como difieren de la versión de serie, podría ser necesario escribir la versión de serie primero.)*
* *(Solo Windows)* Instalar *Ubuntu para Windows 10* disponible desde la Tienda Microsoft `aquí. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Prepare el ambiente Python, como se describe en la sección :ref:`ssh_package_environment`.
* Corra los siguientes comandos en su terminal de línea de comandos (reemplace ``DIRECCIÓN_IP`` por la correspondiente) :

*(Note que los comandos son compatibles con Ubuntu y Ubuntu para Windows 10. Si está usando una distribución diferente de Linux o un sistema operativo diferente, por favor verifique la documentación correspondiente y edite los comandos según sea necesario.)*

::

  #Cambiar al directorio de la carpeta con el firmware desempacado (si no está ya en la carpeta del firmware)
  #Antminer S9
  cd ./braiins-os_am1-s9_ssh_VERSION

  #Antminer S17
  cd ./braiins-os_am2-s17_ssh_VERSION

  #Activar el ambiente virtual (si no está ya activado)
  source .env/bin/activate

  #Correr el script para instalar Braiins OS+
  python3 upgrade2bos.py DIRECCIÓN_IP

**Nota:** *para mas información acerca de los argumentos que pueden usarse, use el argumento* **--help**.

.. _ssh_package_uninstall:

=============================================
Desinstalar Braiins OS+ usando el paquete SSH
=============================================

.. _ssh_package_uninstall_image:

Usando imagen de fábrica
========================

Primero, debe preparar el ambiente Python, que está descrito en la sección :ref:`ssh_package_environment`.

En un Antminer S9, puede escribir una imagen de fábrica del sitio web del fabricante, con la ``IMAGEN_DE_FÁBRICA`` siendo la ruta al archivo o la dirección (URL) al archivo ``tar.gz`` (¡sin extraer!). Las imágenes soportadas con sus correspondientes hashes MD5 están listadas en el archivo
`platform.py <https://github.com/braiins/braiins/blob/master/braiins-os/upgrade/am1/platform.py>`__.

Corra (reemplace los marcadores ``IMAGEN_DE_FÁBRICA`` y ``DIRECCIÓN_IP`` como corresponda):

::

  #Antminer S9
  cd ~/braiins-os_am1-s9_ssh_2020-09-07-1-463cb8d0-20.09-plus && source .env/bin/activate
  python3 restore2factory.py --factory-image IMAGEN_DE_FÁBRICA DIRECCIÓN_IP

  #Antminer S17
  cd ~/braiins-os_am2-s17_ssh_2020-11-27-0-5eb922d4-20.11-plus && source .env/bin/activate
  python3 restore2factory.py --factory-image IMAGEN_DE_FÁBRICA DIRECCIÓN_IP

**Nota:** *para mas información acerca de los argumentos que pueden usarse, use el argumento* **--help**.

.. _ssh_package_uninstall_backup:

Usando respaldo previamente creado
==================================

Primero, debe preparar el ambiente Python, que está descrito en la sección :ref:`ssh_package_environment`.

Si creo un respaldo del firmware original durante la instalación de Braiins OS+, puede restaurarlo mediante el uso de los siguientes comandos (reemplace los marcadores ``ID_RESPALDO_FECHA`` y ``DIRECCIÓN_IP`` como corresponda):

::

  #Antminer S9
  cd ~/braiins-os_am1-s9_ssh_2020-09-07-1-463cb8d0-20.09-plus && source .env/bin/activate
  python3 restore2factory.py backup/ID_RESPALDO_FECHA/ DIRECCIÓN_IP
  
  #Antminer S17
  cd ~/braiins-os_am2-s17_ssh_2020-11-27-0-5eb922d4-20.11-plus && source .env/bin/activate
  python3 restore2factory.py backup/ID_RESPALDO_FECHA/ DIRECCIÓN_IP

**Nota: Este método no es recomendado ya que la creación del respaldo es muy quisquillosa. El respaldo puede corromperse y no hay manera de comprobarlo. ¡Use a su propio riesgo y asegúrese, de tener acceso al minero e insertar una tarjeta SD al mismo en caso de que la restauración no finalice exitosamente!**

.. _opkg:

****
OPKG
****

Los comandos OPKG pueden usarse luego de conectarse al minero vía SSH. Hay muchos comandos OPKG, pero respecto a Braiins OS+, solo necesita usar los siguientes:

  * *opkg update* - actualiza la lista de paquetes. Se recomienda usar este comando antes de otros comandos OPKG.
  * *opkg install NOMBRE_DE_PAQUETE* instala el paquete definido. Se recomienda usar *opkg update* para actualizar la lista de paquetes antes de instalar paquetes.
  * *opkg remove NOMBRE_DE_PAQUETE*

Ya que los cambios de firmware resultan en un reinicio, se espera la siguiente salida:

::

  ...
  Collected errors:
  * opkg_conf_load: Could not lock /var/lock/opkg.lock: Resource temporarily unavailable.
    Saving config files...
    Connection to 10.10.10.1 closed by remote host.
    Connection to 10.10.10.1 closed.

============================================
Características, PROs y CONs de este método:
============================================

  + actualiza Braiins OS+ remotamente
  + cambia a Braiins OS+ desde otras versiones remotamente
  + revierte a la versión inicial de Braiins OS remotamente
  + migra la configuración y continua minando sin necesidad de configurar nada (al actualizar o cambiar a Braiins OS+)

  - no hay modo-por-lotes (a menos que cree sus propios scripts)

.. _opkg_update:

==================================
Actualizar Braiins OS+ usando OPKG
==================================

Con OPKG puede actualizar fácilmente su instalación actual de Braiins OS+, conectándose al minero vía SSH y usando los siguientes comandos:

::

  opkg update
  opkg install firmware

  #también se puede conectar al minero y correr los comandos al mismo tiempo
  ssh root@DIRECCIÓN_IP "opkg update && opkg install firmware"

Esto migrará la configuración y continuará minando sin necesidad de configurar nada.

.. _opkg_switch_to_braiinsplus:

=======================================================
Cambiar a Braiins OS+ desde otras versiones usando OPKG
=======================================================

Con OPKG puede fácilmente cambiar a Braiins OS+, conectándose al minero vía SSH y usando los siguientes comandos:

::

  opkg update
  opkg install bos_plus

  #también se puede conectar al minero y correr los comandos al mismo tiempo
  ssh root@DIRECCIÓN_IP "opkg update && opkg install bos_plus"

Esto migrará la configuración y continuará minando sin necesidad de configurar nada. El límite de energía por defecto se pone a.

.. _opkg_factory_reset:

==============================================
Restablecer de fábrica Braiins OS+ usando OPKG
==============================================

Con OPKG puede revertir fácilmente a la versión inicial de Braiins OS (la versión que fue instalada por primera vez en ese dispositivo), conectándose al minero vía SSH y usando los siguientes comandos:

::

  opkg update
  opkg remove firmware

  #también se puede conectar al minero y correr los comandos al mismo tiempo
  ssh root@DIRECCIÓN_IP "opkg update && opkg remove firmware"

Esto restablecerá la configuración al estado luego de la primera instalación de Braiins OS.

.. _sysupgrade:

******************
Paquete sysupgrade
******************

Sysupgrade se usa para actualizar el sistema corriendo en el dispositivo. Con este método, puede instalar varias versiones de Braiins OS o crear un respaldo del sistema. La instalación de un firmware usando la *interfaz web Braiins OS* o usar *opkg install firmware* usan este método. Es recomendado usar la *interfaz web Braiins OS* u *opkg install firmware* en lugar de este método.

===
Uso
===

Para poder usar sysupgrade, necesita conectarse al minero vía SSH. La sintaxis es la siguiente:

::

  sysupgrade [parámetros] <archivo imagen o dirección (URL)>

Los parámetros mas importantes son **--help** (para mostrar la ayuda) y **-F** para forzar la instalación. No es recomendado usar este método (a pesar de la forma, se describe abajo), a menos que realmente sepa, lo que está haciendo.

============================================
Características, PROs y CONs de este método:
============================================

  + instala varias versiones de Braiins OS, estando conectado al minero
  + migra la configuración
  + parámetros disponibles para personalizar el proceso

  - no hay modo-por-lotes (a menos que cree sus propios scripts)
  - no puede cambiar a una versión vieja de Braiins OS (liberada antes de 2020)

.. _sysupgrade_switch_braiinsos:

=============================================================================
Cambiar a Braiins OS (sin autoajuste) desde otras versiones usando Sysupgrade
=============================================================================

Para actualizar desde una versión anterior de Braiins OS o desactualizar desde Braiins OS+, use el siguiente comando (reemplace el marcador ``DIRECCIÓN_IP`` como corresponda):

::

  #Antminer S9
  ssh root@DIRECCIÓN_IP 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-09-07-0-e50f2a1b-20.09_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'
  
  #Antminer S17
  ssh root@DIRECCIÓN_IP 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am2-s17/firmware_2020-09-07-0-e50f2a1b-20.09_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

Este comando contiene los siguientes comandos:

  * **ssh** - para conectarse al minero
  * **wget** - usado para descargar archivos, en este caso el paquete firmware
  * **sysupgrade** - para propiamente escribir el paquete de firmware descargado

.. _sysupgrade_switch_braiinsplus:

=============================================================
Cambiar a Braiins OS+ desde otras versiones usando Sysupgrade
=============================================================

Para actualizar desde una versión anterior de Braiins OS, use el siguiente comando (reemplace el marcador ``DIRECCIÓN_IP`` como corresponda):

::

  #Antminer S9
  ssh root@DIRECCIÓN_IP 'wget -O /tmp/firmware.tar https://feeds.braiins-os.com/am1-s9/firmware_2020-09-07-1-463cb8d0-20.09-plus_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

  #Antminer S17
  ssh root@DIRECCIÓN_IP 'wget -O /tmp/firmware.tar https://feeds.braiins-os.com/am2-s17/firmware_2020-11-27-0-5eb922d4-20.11-plus_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

Este comando contiene los siguientes comandos:

  * **ssh** - para conectarse al minero
  * **wget** - usado para descargar archivos, en este caso el paquete firmware
  * **sysupgrade** - para propiamente escribir el paquete de firmware descargado

Nota: Se recomienda usar la *Caja de herramientas BOS*, *Interfaz web Braiins OS* u *opkg install bos_plus* en lugar de este método.

.. _bos2bos:

**************
Script Bos2Bos
**************

**Bos2Bos script is not recommended to use, unless you experience problems with the installation using the other methods.** This method works, only if Braiins OS is already running on the device.

============================================
Características, PROs y CONs de este método:
============================================

  + instala cualquier versión de Braiins OS remotamente
  + instala una versión limpia de Braiins OS
  + parámetros disponibles para personalizar el proceso

  - no hay modo-por-lotes (a menos que cree sus propios scripts)

===
Uso
===

Usage of the Bos2Bos script requires the following setup:

* *(Solo Windows)* Instalar *Ubuntu para Windows 10* disponible desde la Tienda Microsoft `aquí. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Corra los siguientes comandos en su terminal de línea de comandos:

*(Note que los comandos son compatibles con Ubuntu y Ubuntu para Windows 10. Si está usando una distribución diferente de Linux o un sistema operativo diferente, por favor verifique la documentación correspondiente y edite los comandos según sea necesario.)*

::

  #Actualizar los repositorios e instalar dependencias
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv

  # clonar repositorio
  git clone https://github.com/braiins/braiins-os.git

  #cambiar el directorio
  cd ./braiins-os/braiins-os/

  #Crear un ambiente virtual y activarlo
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate

  #Instalar los paquetes Python requeridos
  python3 -m pip install -r requirements.txt

Luego de finalizar la instalación exitosamente, puede usar los siguientes comandos:

::

  #activar el ambiente virtual
  source .env/bin/activate

  #su uso básico es el siguiente
  python3 bos2bos.py URL_FIRMWARE DIRECCIÓN_IP

  #la descripción de todos los parámetros disponibles puede verse usando el siguiente comando
  python3 bos2bos.py -h

*****************
Herramienta Miner
*****************

.. _miner_nand_install:

==============================================
Instalar SD a NAND usando la herramienta Miner
==============================================

La tarjeta SD puede usarse para reemplazar el firmware corriendo en la NAND con Braiins OS+. Esto puede hacerse conectándose al minero via SSH y usando el siguiente comando:

  ::

    miner nand_install


.. _miner_factory_reset:

==============================================================
Restablecer de fábrica Braiins OS+ usando la herramienta Miner
==============================================================

Restablecer de fábrica también puede hacerse con la *herramienta Miner*. Use el siguiente comando para hacerlo:

  ::

    miner factory_reset

.. _miner_detect:

=================================================================
Detectar el dispositivo mediante LEDs usando la herramienta Miner
=================================================================

Puede conseguir un dispositivo encendiendo el parpadeo LED, usando la *herramienta Miner*. Use el siguiente comando para hacerlo:

  ::

    #encender parpadeo LED
    miner fault_light on

    #apagar parpadeo LED
    miner fault_light off

.. _miner_nightly:

====================================================================
Encender/apagar alimentaciones nocturnas usando la herramienta Miner
====================================================================

Puede encender las alimentaciones Nocturnas para poder actualizar a las últimas versiones nocturnas. Estas versiones son para arreglar problemas cruciales tan rápido como sea posible y por ello, no se hacen pruebas a fondo en estas versiones. Use estas versiones con precaución y solo si resuelve sus problemas. Para poder encender/apagar las alimentaciones nocturnas, use el siguiente comando:

  ::

    #encender alimentaciones nocturnas
    miner nightly_feeds on

    #apagar alimentaciones nocturnas
    miner nightly_feeds off

.. _miner_autoupgrade:

===========================================================
Encender/apagar auto-actualizar usando la herramienta Miner
===========================================================

Puede encender la característica de auto-actualizar, que actualizará el sistema automáticamente a la última versión. Esta característica está **encendida** por defecto, en transición desde un firmware **de serie** y **apagada** al actualizar desde versiones anteriores de **Braiins OS** o **Braiins OS+**. Para poder encender/apagar auto-actualizar, use el siguiente comando:

  ::

    #encender auto-actualizar
    miner auto_upgrade on

    #apagar auto-actualizar
    miner auto_upgrade off
  * Usando el paquete sysupgrade (:ref:`sysupgrade_switch_braiinsplus`)
  * Usando el script bos2bos (:ref:`bos2bos`)

 * Restablecer a la versión Braiins OS inicial (la versión que instaló en su dispositivo por primera vez) - restablecimiento de fabrica

  * Usando OPKG (:ref:`opkg_factory_reset`)
  * Usando la tarjeta SD (:ref:`sd_factory_reset`)
  * Usando la herramienta "miner" (:ref:`miner_factory_reset`)
  * Usando el script bos2bos (:ref:`bos2bos`)

 * Desinstalar Braiins OS+

  * Usando la Caja de herramientas BOS (:ref:`bosbox_uninstall`)
  * Usando scripts SSH (:ref:`ssh_package_uninstall`)

 * Encender/apagar alimentaciones nocturnas

  * Usando la herramienta "miner" (:ref:`miner_nightly`)

 * Encender/apagar auto-actualizar

  * Usando la herramienta "miner" (:ref:`miner_autoupgrade`)

.. _bosbox:

*************************
Caja de herramientas BOS
*************************

La Caja de herramientas BOS es una nueva herramienta, que permite al usuario instalar, desinstalar, actualizar, detectar y configurar fácilmente Braiins OS+. También permite hacerlo en modo por lotes, lo que hace la gestión de un gran número de dispositivos mas fácil. Esta es la manera recomendada de gestionar sus máquinas.

===
Uso
===

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/>`_.
  * Cree un nuevo archivo de texto, cambie la extensión ".txt" a ".csv" e inserte las direcciones IP en las que desea ejecutar los comandos. Coloque el archivo en el directorio donde se encuentra la Caja de herramientas BOS. **¡Use solo una dirección IP por línea!**
  * Siga las instrucciones abajo

============================================
Características, PROs y CONs de este método:
============================================

  + instala Braiins OS+ remotamente
  + actualiza Braiins OS+ remotamente
  + desinstala Braiins OS+ remotamente
  + configura Braiins OS+ remotamente
  + busca las máquinas en la red
  + por defecto migra toda la configuración (puede ajustarse) al instalar Braiins OS+
  + por defecto migra la configuración de red (puede ajustarse) al desinstalar Braiins OS+
  + hay parámetros disponibles para personalizar el proceso
  + activa el autoajuste al límite de energía por defecto al instalar Braiins OS+
  + modo-por-lotes disponible para gestionar múltiples dispositivos a la vez
  + fácil de usar

  - no funciona en un minero con SSH bloqueado

.. _bosbox_install:

=====================================================
Instalar Braiins OS+ con la Caja de herramientas BOS
=====================================================

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/plus/download/>`_.
  * Cree un nuevo archivo de texto, cambie la extensión ".txt" a ".csv" e inserte las direcciones IP en las que desea ejecutar los comandos. Coloque el archivo en el directorio donde se encuentra la Caja de herramientas BOS. ¡Use solo una dirección IP por línea!
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.bat install ARGUMENTOS NOMBREHOST

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox install ARGUMENTOS NOMBREHOST

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los **argumentos** siguientes para ajustar el proceso:

**Nota importante:**
Al instalar Braiins OS+ en **un solo dispositivo**, use el argumento *NOMBREHOST* (dirección IP).
Al instalar Braiins OS+ en **varios dispositivos**, **NO** use el argumento NOMBREHOST, sino el argumento *--batch LOTE* en su lugar.

====================================  ==================================================================
Argumentos                            Descripción
====================================  ==================================================================
-h, --help                            muestra este mensaje de ayuda y sale
--batch LOTE                          ruta al archivo con la lista de hosts (direcciones IPs) a instalar
--backup                              hacer el respaldo al minero antes de actualizar
--no-nand-backup                      saltar respaldo completo NAND (la configuración aun se respalda)
--pool-user [USUARIO_POOL]            fijar nombre de usuario y minero al pool por defecto
--psu-power-limit [LÍMITE_ENERGÍA]    fijar límite de energía (en vatios) para la fuente de poder
--no-keep-network                     no mantener la configuración de red (usar DHCP)
--no-keep-pools                       no mantener la configuración del pool del minero
--no-keep-hostname                    no mantener el nombre de host y generar uno nuevo basado en MAC
--keep-hostname                       forzar mantener cualquier nombre host del minero
--no-wait                             no esperar a que el sistema esté completamente actualizado
--dry-run                             hacer todos los pasos de actualización sin realmente actualizar
--post-upgrade [POST_ACTUALIZADO]     ruta al directorio con el script stage3.sh
-p, --password CLAVE_INSTALACIÓN      palabra clave ssh para la instalación
====================================  ==================================================================

**Ejemplo:**

::

  ./bos-toolbox.bat install --psu-power-limit 1200 --password clave listaDeMineros.csv

Este comando instalará Braiins OS+ en los mineros, que estén especificados en el archivo *listaDeMineros.csv* y fija el límite de energía a 1200 en todos ellos. El comando también usará automáticamente la palabra clave SSH *clave*, cuando el minero la pida.

.. _bosbox_update:

=======================================================
Actualizar Braiins OS+ con la Caja de herramientas BOS
=======================================================

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/plus/download/>`_.
  * Cree un nuevo archivo de texto, cambie la extensión ".txt" a ".csv" e inserte las direcciones IP en las que desea ejecutar los comandos. Coloque el archivo en el directorio donde se encuentra la Caja de herramientas BOS.
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.bat update ARGUMENTOS NOMBREHOST

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox update ARGUMENTOS NOMBREHOST

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los **argumentos** siguientes para ajustar el proceso:

**Nota importante:**
Al actualizar Braiins OS+ en **un solo dispositivo**, use el argumento *NOMBREHOST* (dirección IP).
Al actualizar Braiins OS+ en **varios dispositivos**, **NO** use el argumento NOMBREHOST, sino el argumento *--batch LOTE* en su lugar.

====================================  ==================================================================
Argumentos                            Descripción
====================================  ==================================================================
-h, --help                            muestra este mensaje de ayuda y sale
-p PASSWORD, --password PASSWORD      palabra clave administrativa
-i, --ignore                          no detener en errores
====================================  ==================================================================


**Ejemplo:**

::

  ./bos-toolbox.bat update listaDeMineros.csv

Este comando buscará actualizaciones para los mineros, que están especificados en la *listaDeMineros.csv* y los actualizará si hay una nueva versión del firmware.

.. _bosbox_uninstall:

========================================================
Desinstalar Braiins OS+ con la Caja de herramientas BOS
========================================================

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/plus/download/>`_.
  * Cree un nuevo archivo de texto en su editor de texto e inserte las direcciones IP en las cuales desea ejecutar los comandos. Cada dirección IP debe ser separada por una coma. (Note que puede encontrar la dirección IP en la interfaz web de Braiins OS+ yendo a *Status -> Overview*.) Luego guarde el archivo en el mismo directorio donde guardó la Caja de herramientas BOS y cambie la extensión ".txt" a ".csv".
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.bat uninstall ARGUMENTOS NOMBREHOST

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox uninstall ARGUMENTOS NOMBREHOST

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los **argumentos** siguientes para ajustar el proceso:

**Nota importante:**
Al desinstalar Braiins OS+ en **un solo dispositivo**, use el argumento *NOMBREHOST* (dirección IP).
Al desinstalar Braiins OS+ en **varios dispositivos**, **NO** use el argumento NOMBREHOST, sino el argumento *--batch LOTE* en su lugar.

====================================  ==================================================================
Argumentos                            Descripción
====================================  ==================================================================
-h, --help                            muestra este mensaje de ayuda y sale
--factory-image IMAGEN_DE_FÁBRICA     ruta/url a imagen de actualización de firmware original (defecto:
                                      Antminer-S9-all-201812051512-autofreq-user-Update2UBI-NF.tar.gz)
====================================  ==================================================================

**Ejemplo:**

::

  ./bos-toolbox.bat uninstall listaDeMineros.csv

Este comando desinstalará Braiins OS+ de los mineros, que están especificados en el archivo *listaDeMineros.csv* e instala un firmware de serie (Antminer-S9-all-201812051512-autofreq-user-Update2UBI-NF.tar.gz).

.. _bosbox_configure:

=======================================================
Configurar Braiins OS+ con la Caja de herramientas BOS
=======================================================

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/plus/download/>`_.
  * Cree un nuevo archivo de texto en su editor de texto e inserte las direcciones IP en las cuales desea ejecutar los comandos. Cada dirección IP debe ser separada por una coma. (Note que puede encontrar la dirección IP en la interfaz web de Braiins OS+ yendo a *Status -> Overview*.) Luego guarde el archivo en el mismo directorio donde guardó la Caja de herramientas BOS y cambie la extensión ".txt" a ".csv".
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.bat config ARGUMENTOS ACCIÓN TABLA

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox config ARGUMENTOS ACCIÓN TABLA

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los **argumentos** siguientes para ajustar el proceso:

====================================  ==================================================================
Argumentos                            Descripción
====================================  ==================================================================
-h, --help                            muestra este mensaje de ayuda y sale
-u USER, --user USER                  nombre administrativo
-p PASSWORD, --password PASSWORD      palabra clave administrativa o "preguntarla"
-c, --check                           ensayo sin escrituras
-i, --ignore                          no detener en errores
====================================  ==================================================================

**Debe usar una** de las siguientes **acciones** para ajustar el proceso:

====================================  ==================================================================
Acciones                              Descripción
====================================  ==================================================================
load                                  cargar la configuración actual de los mineros (especificados en 
                                      el archivo CSV) e insertarla al archivo CSV
save                                  guardar la configuración desde el archivo CSV a los mineros 
                                      (esto no la aplica)
apply                                 aplicar la configuración, que fue copiada desde el archivo CSV a 
                                      los mineros
save_apply                            guardar y aplicar la configuración del archivo CSV a los mineros
====================================  ==================================================================

**Ejemplo:**

::

  ./bos-toolbox.bat config --user root load listaDeMineros.csv

  #edite el archivo CSV con un editor de hojas de cálculo (ej: Office Excel, LibreOffice Calc, etc.)

  ./bos-toolbox.bat config --user root save_apply listaDeMineros.csv

El primer comando va a cargar la configuración de los mineros, que estén especificados en la *listaDeMineros.csv* (usando el usuario *root*) y la guardará en ese archivo CSV. Ahora puede abrir el archivo y editar lo que necesite. Luego de que el archivo esté editado, el segundo comando copiará la configuración de vuelta a los mineros y la aplicará.

.. _bosbox_scan:

============================================================================
Explorar la red para identificar mineros usando la Caja de herramientas BOS
============================================================================

  * Descargue la **Caja de herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/plus/download/>`_.
  * Cree un nuevo archivo de texto en su editor de texto e inserte las direcciones IP en las cuales desea ejecutar los comandos. Cada dirección IP debe ser separada por una coma. (Note que puede encontrar la dirección IP en la interfaz web de Braiins OS+ yendo a *Status -> Overview*.) Luego guarde el archivo en el mismo directorio donde guardó la Caja de herramientas BOS y cambie la extensión ".txt" a ".csv".
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.bat scan ARGUMENTOS

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox scan ARGUMENTOS

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Puede usar los **argumentos** siguientes para ajustar el proceso:

====================================  ==================================================================
Argumentos                            Descripción
====================================  ==================================================================
-h, --help                            muestra este mensaje de ayuda y sale
====================================  ==================================================================

**Debe usar una** de las siguientes **acciones** para ajustar el proceso:

====================================  ==================================================================
Acciones                              Descripción
====================================  ==================================================================
scan                                  explorar activamente el rango provisto de direcciones
listen                                escuchar transmisión entrande desde los dispositivos (al presionar
                                      el botón IP report)
====================================  ==================================================================

**Ejemplo:**

::

  ./bos-toolbox.bat discover scan 10.10.10.0/24

Este comando va explorar la red, en el rango 10.10.10.0 - 10.10.10.255 y mostrará los mineros que encuentre con sus direcciones IP.

.. _web_package:

***********
Paquete Web
***********

El paquete Web puede usarse para cambiar el firmware de serie, liberado antes de 2019. También debería funcionar con otros basados en firmware de serie. Este paquete no puede usarse con firmware de serie, liberado en 2019 o posterior, debido a la verificación de firma, que fue implementada. La verificación de firma previene el uso de otro firmware que no sea firmware de serie originales.

===
Uso
===

  * Descargue el **Paquete Web** desde nuestro `sitio web <https://braiins-os.com/>`_.
  * Siga las instrucciones abajo

============================================
Características, PROs y CONs de este método:
============================================

  + reemplaza el firmware de serie con Braiins OS+ sin herramientas adicionales
  + migra la configuración de red
  + migra las direcciones (URL) de los pool, usuarios y claves
  + activa el autoajuste al límite de energía por defecto

  - no puede usarse con firmware de serie liberado en 2019 o luego
  - no puede configurar la instalación (ej: siempre migrará la configuración de red)
  - no hay modo-por-lotes (a menos que se cree sus propios scripts)

.. _web_package_install:

==========================================
Instalar Braiins OS+ usando el Paquete web
==========================================

  * Descargue el **Paquete web** desde nuestro `sitio web <https://braiins-os.com/>`_.
  * Ingrese a su minero y vaya a la sección *System -> Upgrade*.
  * Suba la imagen descargada y escriba la imagen.

.. _sd:

**********************
Imagen para tarjeta SD
**********************

Si está corriendo firmware de serie, que fue liberado en 2019 o luego, la única forma de instalar Braiins OS+ es insertar una tarjeta SD con Braiins OS+ escrito en ella. En 2019, la conexión SSH fue bloqueada y la verificación de firma en la interfaz web impide el uso de otro firmware distinto al de serie.

===
Uso
===

  * Descargue la **Imagen para tarjeta SD** desde nuestro `sitio web <https://braiins-os.com/>`_.
  * Siga las instrucciones abajo

============================================
Características, PROs y CONs de este método:
============================================

  + reemplaza el firmware de serie con SSH bloqueado con Braiins OS+
  + usa la configuración de red almacenada en la NAND (esto puede apagarse, vea la sección *Network settings* abajo)
  + activa el autoajuste al límite de energía por defecto

  - no migra direcciones (URL) de pool, usuarios o claves
  - no hay modo-por-lotes

.. _sd_install:

==============================================
Instalar Braiins OS+ usando la tarjeta SD card
==============================================

 * Descargue la **Imagen para tarjeta SD** desde nuestro `sitio web <https://braiins-os.com/>`_.
 * Escriba la imagen descargada a una tarjeta SD (ej: usando `Etcher <https://etcher.io/>`_). *Nota: Una simple copia no funcionará. ¡La tarjeta SD debe ser escrita!*
 * Ajuste los jumpers para arrancar desde la tarjeta SD (en lugar de la memoria NAND), como se muestra abajo.

  .. |pic1| image:: ../_static/s9-jumpers.png
      :width: 45%
      :alt: S9 Jumpers

  .. |pic2| image:: ../_static/s9-jumpers-board.png
      :width: 45%
      :alt: S9 Jumpers Board

  |pic1|  |pic2|

 * Inserte la tarjeta SD card en el dispositivo, luego inicie el dispositivo.
 * Tras un momento, debe poder acceder la interfaz de Braiins OS+ a través de la dirección IP del dispositivo.
 * *[Opcional]:* Ahora puede instalar Braiins OS+ a la NAND (ver la sección :ref:`sd_nand_install`)

.. _sd_network:

====================
Configuración de red
====================

 Por defecto, la configuración almacenada en la NAND se utilizará, mientras esté corriendo Braiins OS+ desde una tarjeta SD. Esta característica puede apagarse, siguiendo los pasos abajo:

  * Monte la primera partición FAT de la tarjeta SD
  * Abra el archivo uEnv.txt e inserte la frase siguiente (asegúrese de que solo hay una frase por línea)

  ::

    cfg_override=no

Deshabilitar el uso de la vieja configuración de red es beneficioso para los usuarios, que tienen problemas con el minero no estar visible en la red (ej: la dirección IP estática usada en la NAND está fuera del rango de la red). Al hacerlo, se usa DHCP.

.. _sd_nand_install:

===============
Instalar a NAND
===============

La tarjeta SD puede usarse para reemplazar el firmware corriendo en la NAND con Braiins OS+. Eso se hace:
  * usando la interfaz web - sección *System -> Install current system to device (NAND)*
  * usando la herramienta *miner*, via SSH - siga esta sección de la guía :ref:`miner_nand_install`

.. _sd_factory_reset:

=======================================================
Restablecer de fábrica Braiins OS+ usando la tarjeta SD
=======================================================

Puede hacer un restablecimiento de fábrica, siguiendo los pasos abajo:

  * Monte la primera partición FAT de la tarjeta SD
  * Abra el archivo uEnv.txt e inserte la frase siguiente (asegúrese de que solo hay una frase por línea)

  ::

    factory_reset=yes

.. _ssh_package:

***********************************
Paquete de instalación remota (SSH)
***********************************

Con el *paquete de instalación remota (SSH)* puede instalar o desinstalar Braiins OS+. Este método no es recomendado, ya que requiere una instalación Python. Use la caja de herramientas BOS en su lugar.

===
Uso
===

  * Descargue el **Paquete de instalación remota (SSH)** desde nuestro `sitio web <https://braiins-os.com/>`_.
  * Siga las instrucciones abajo

============================================
Características, PROs y CONs de este método:
============================================

  + instala Braiins OS+ remotamente
  + desinstala Braiins OS+ remotamente
  + migra toda la configuración por defecto (puede ajustarse) al instalar Braiins OS+
  + migra la configuración de red por defecto (puede ajustarse) al desinstalar Braiins OS+
  + parámetros disponibles para personalizar el proceso
  + enciende el autoajuste a un límite de energía por defecto al instalar Braiins OS+

  - no hay modo-por-lotes (a menos que cree sus propios scripts)
  - requiere una larga instalación
  - no funciona en un minero con SSH bloqueado

.. _ssh_package_environment:

======================
Preparando el ambiente
======================

Primero, necesita preparar el ambiente Python. Esto consiste en los siguientes pasos:

* *(Solo Windows)* Instalar *Ubuntu para Windows 10* disponible desde la Tienda Microsoft `aquí. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Corra los siguientes comandos en su terminal de línea de comandos:

*(Note que los comandos son compatibles con Ubuntu y Ubuntu para Windows 10. Si está usando una distribución diferente de Linux o un sistema operativo diferente, por favor verifique la documentación correspondiente y edite los comandos según sea necesario.)*

::

  #Actualizar los repositorios e instalar dependencias
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv

  #Descargar y extraer el paquete de firmware
  wget -c http://feeds.braiins-os.com/20.04/braiins-os_am1-s9_ssh_2020-04-30-1-cbf99510-plus.tar.gz -O - | tar -xz

  #Cambiar el directorio a la carpeta donde desempacó el firmware
  cd ./braiins-os_am1-s9_ssh_2020-04-30-1-cbf99510-plus

  #Crear un ambiente virtual y activarlo
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate

  #Instalar los paquetes Python requeridos
  python3 -m pip install -r requirements.txt

.. _ssh_package_install:

==========================================
Instalar Braiins OS+ usando el paquete SSH
==========================================

La instalación de Braiins OS+ usando el asi-llamado *Método SSH* consiste en los siguientes pasos:

* *(Firmware Personalizado)* Escribir firmware de serie. Este paso puede omitirse si el dispositivo está corriendo el firmware de serie o una versión previa de Braiins OS. *(Nota: Es posible, que Braiins OS+ pueda ser instalado directamente sobre un firmware personalizado, pero como difieren de la versión de serie, podría ser necesario escribir la versión de serie primero.)*
* *(Solo Windows)* Instalar *Ubuntu para Windows 10* disponible desde la Tienda Microsoft `aquí. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Prepare el ambiente Python, como se describe en la sección :ref:`ssh_package_environment`.
* Corra los siguientes comandos en su terminal de línea de comandos (reemplace ``DIRECCIÓN_IP`` por la correspondiente) :

*(Note que los comandos son compatibles con Ubuntu y Ubuntu para Windows 10. Si está usando una distribución diferente de Linux o un sistema operativo diferente, por favor verifique la documentación correspondiente y edite los comandos según sea necesario.)*

::

  #Cambiar al directorio de la carpeta con el firmware desempacado (si no está ya en la carpeta del firmware)
  cd ./braiins-os_am1-s9_ssh_2020-04-30-1-cbf99510-plus

  #Activar el ambiente virtual (si no está ya activado)
  source .env/bin/activate

  #Correr el script para instalar Braiins OS+
  python3 upgrade2bos.py DIRECCIÓN_IP

**Nota:** *para mas información acerca de los argumentos que pueden usarse, use el argumento* **--help**.

.. _ssh_package_uninstall:

=============================================
Desinstalar Braiins OS+ usando el paquete SSH
=============================================

.. _ssh_package_uninstall_image:

Usando imagen de fábrica
========================

Primero, debe preparar el ambiente Python, que está descrito en la sección :ref:`ssh_package_environment`.

En un Antminer S9, puede escribir una imagen de fábrica del sitio web del fabricante, con la ``IMAGEN_DE_FÁBRICA`` siendo la ruta al archivo o la dirección (URL) al archivo ``tar.gz`` (¡sin extraer!). Las imágenes soportadas con sus correspondientes hashes MD5 están listadas en el archivo
`platform.py <https://github.com/braiins/braiins/blob/master/braiins-os/upgrade/am1/platform.py>`__.

Corra (reemplace los marcadores ``IMAGEN_DE_FÁBRICA`` y ``DIRECCIÓN_IP`` como corresponda):

::

  cd ~/braiins-os_am1-s9_ssh_2020-04-30-1-cbf99510-plus && source .env/bin/activate
  python3 restore2factory.py --factory-image IMAGEN_DE_FÁBRICA DIRECCIÓN_IP

**Nota:** *para mas información acerca de los argumentos que pueden usarse, use el argumento* **--help**.

.. _ssh_package_uninstall_backup:

Usando respaldo previamente creado
==================================

Primero, debe preparar el ambiente Python, que está descrito en la sección :ref:`ssh_package_environment`.

Si creo un respaldo del firmware original durante la instalación de Braiins OS+, puede restaurarlo mediante el uso de los siguientes comandos (reemplace los marcadores ``ID_RESPALDO_FECHA`` y ``DIRECCIÓN_IP`` como corresponda):

::

  cd ~/braiins-os_am1-s9_ssh_2020-04-30-1-cbf99510-plus && source .env/bin/activate
  python3 restore2factory.py backup/ID_RESPALDO_FECHA/ DIRECCIÓN_IP

**Nota: Este método no es recomendado ya que la creación del respaldo es muy quisquillosa. El respaldo puede corromperse y no hay manera de comprobarlo. ¡Use a su propio riesgo y asegúrese, de tener acceso al minero e insertar una tarjeta SD al mismo en caso de que la restauración no finalice exitosamente!**

.. _opkg:

****
OPKG
****

Los comandos OPKG pueden usarse luego de conectarse al minero vía SSH. Hay muchos comandos OPKG, pero respecto a Braiins OS+, solo necesita usar los siguientes:

  * *opkg update* - actualiza la lista de paquetes. Se recomienda usar este comando antes de otros comandos OPKG.
  * *opkg install NOMBRE_DE_PAQUETE* instala el paquete definido. Se recomienda usar *opkg update* para actualizar la lista de paquetes antes de instalar paquetes.
  * *opkg remove NOMBRE_DE_PAQUETE*

Ya que los cambios de firmware resultan en un reinicio, se espera la siguiente salida:

::

  ...
  Collected errors:
  * opkg_conf_load: Could not lock /var/lock/opkg.lock: Resource temporarily unavailable.
    Saving config files...
    Connection to 10.10.10.1 closed by remote host.
    Connection to 10.10.10.1 closed.

============================================
Características, PROs y CONs de este método:
============================================

  + actualiza Braiins OS+ remotamente
  + cambia a Braiins OS+ desde otras versiones remotamente
  + revierte a la versión inicial de Braiins OS remotamente
  + migra la configuración y continua minando sin necesidad de configurar nada (al actualizar o cambiar a Braiins OS+)

  - no hay modo-por-lotes (a menos que cree sus propios scripts)

.. _opkg_update:

==================================
Actualizar Braiins OS+ usando OPKG
==================================

Con OPKG puede actualizar fácilmente su instalación actual de Braiins OS+, conectándose al minero vía SSH y usando los siguientes comandos:

::

  opkg update
  opkg install firmware

  #también se puede conectar al minero y correr los comandos al mismo tiempo
  ssh root@DIRECCIÓN_IP "opkg update && opkg install firmware"

Esto migrará la configuración y continuará minando sin necesidad de configurar nada.

.. _opkg_switch_to_braiinsplus:

=======================================================
Cambiar a Braiins OS+ desde otras versiones usando OPKG
=======================================================

Con OPKG puede fácilmente cambiar a Braiins OS+, conectándose al minero vía SSH y usando los siguientes comandos:

::

  opkg update
  opkg install bos_plus

  #también se puede conectar al minero y correr los comandos al mismo tiempo
  ssh root@DIRECCIÓN_IP "opkg update && opkg install bos_plus"

Esto migrará la configuración y continuará minando sin necesidad de configurar nada. El límite de energía por defecto se pone a.

.. _opkg_factory_reset:

==============================================
Restablecer de fábrica Braiins OS+ usando OPKG
==============================================

Con OPKG puede revertir fácilmente a la versión inicial de Braiins OS (la versión que fue instalada por primera vez en ese dispositivo), conectándose al minero vía SSH y usando los siguientes comandos:

::

  opkg update
  opkg remove firmware

  #también se puede conectar al minero y correr los comandos al mismo tiempo
  ssh root@DIRECCIÓN_IP "opkg update && opkg remove firmware"

Esto restablecerá la configuración al estado luego de la primera instalación de Braiins OS.

.. _sysupgrade:

******************
Paquete sysupgrade
******************

Sysupgrade se usa para actualizar el sistema corriendo en el dispositivo. Con este método, puede instalar varias versiones de Braiins OS o crear un respaldo del sistema. La instalación de un firmware usando la *interfaz web Braiins OS* o usar *opkg install firmware* usan este método. Es recomendado usar la *interfaz web Braiins OS* u *opkg install firmware* en lugar de este método.

===
Uso
===

Para poder usar sysupgrade, necesita conectarse al minero vía SSH. La sintaxis es la siguiente:

::

  sysupgrade [parámetros] <archivo imagen o dirección (URL)>

Los parámetros mas importantes son **--help** (para mostrar la ayuda) y **-F** para forzar la instalación. No es recomendado usar este método (a pesar de la forma, se describe abajo), a menos que realmente sepa, lo que está haciendo.

============================================
Características, PROs y CONs de este método:
============================================

  + instala varias versiones de Braiins OS, estando conectado al minero
  + migra la configuración
  + parámetros disponibles para personalizar el proceso

  - no hay modo-por-lotes (a menos que cree sus propios scripts)
  - no puede cambiar a una versión vieja de Braiins OS (liberada antes de 2020)

.. _sysupgrade_switch_braiinsos:

=============================================================================
Cambiar a Braiins OS (sin autoajuste) desde otras versiones usando Sysupgrade
=============================================================================

Para actualizar desde una versión anterior de Braiins OS o desactualizar desde Braiins OS+, use el siguiente comando (reemplace el marcador ``DIRECCIÓN_IP`` como corresponda):

::

  ssh root@DIRECCIÓN_IP 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-04-30-0-259943b5_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

Este comando contiene los siguientes comandos:

  * **ssh** - para conectarse al minero
  * **wget** - usado para descargar archivos, en este caso el paquete firmware
  * **sysupgrade** - para propiamente escribir el paquete de firmware descargado

.. _sysupgrade_switch_braiinsplus:

=============================================================
Cambiar a Braiins OS+ desde otras versiones usando Sysupgrade
=============================================================

Para actualizar desde una versión anterior de Braiins OS, use el siguiente comando (reemplace el marcador ``DIRECCIÓN_IP`` como corresponda):

::

  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar http://feeds.braiins-os.com/am1-s9/firmware_2020-04-30-1-cbf99510-plus_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

Este comando contiene los siguientes comandos:

  * **ssh** - para conectarse al minero
  * **wget** - usado para descargar archivos, en este caso el paquete firmware
  * **sysupgrade** - para propiamente escribir el paquete de firmware descargado

Nota: Se recomienda usar la *Caja de herramientas BOS*, *Interfaz web Braiins OS* u *opkg install bos_plus* en lugar de este método.

.. _bos2bos:

**************
Script Bos2Bos
**************

**Bos2Bos script is not recommended to use, unless you experience problems with the installation using the other methods.** This method works, only if Braiins OS is already running on the device.

============================================
Características, PROs y CONs de este método:
============================================

  + instala cualquier versión de Braiins OS remotamente
  + instala una versión limpia de Braiins OS
  + parámetros disponibles para personalizar el proceso

  - no hay modo-por-lotes (a menos que cree sus propios scripts)

===
Uso
===

Usage of the Bos2Bos script requires the following setup:

* *(Solo Windows)* Instalar *Ubuntu para Windows 10* disponible desde la Tienda Microsoft `aquí. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Corra los siguientes comandos en su terminal de línea de comandos:

*(Note que los comandos son compatibles con Ubuntu y Ubuntu para Windows 10. Si está usando una distribución diferente de Linux o un sistema operativo diferente, por favor verifique la documentación correspondiente y edite los comandos según sea necesario.)*

::

  #Actualizar los repositorios e instalar dependencias
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv

  # clonar repositorio
  git clone https://github.com/braiins/braiins-os.git

  #cambiar el directorio
  cd ./braiins-os/braiins-os/

  #Crear un ambiente virtual y activarlo
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate

  #Instalar los paquetes Python requeridos
  python3 -m pip install -r requirements.txt

Luego de finalizar la instalación exitosamente, puede usar los siguientes comandos:

::

  #activar el ambiente virtual
  source .env/bin/activate

  #su uso básico es el siguiente
  python3 bos2bos.py URL_FIRMWARE DIRECCIÓN_IP

  #la descripción de todos los parámetros disponibles puede verse usando el siguiente comando
  python3 bos2bos.py -h

*****************
Herramienta Miner
*****************

.. _miner_nand_install:

==============================================
Instalar SD a NAND usando la herramienta Miner
==============================================

La tarjeta SD puede usarse para reemplazar el firmware corriendo en la NAND con Braiins OS+. Esto puede hacerse conectándose al minero via SSH y usando el siguiente comando:

  ::

    miner nand_install


.. _miner_factory_reset:

==============================================================
Restablecer de fábrica Braiins OS+ usando la herramienta Miner
==============================================================

Restablecer de fábrica también puede hacerse con la *herramienta Miner*. Use el siguiente comando para hacerlo:

  ::

    miner factory_reset

.. _miner_detect:

=================================================================
Detectar el dispositivo mediante LEDs usando la herramienta Miner
=================================================================

Puede conseguir un dispositivo encendiendo el parpadeo LED, usando la *herramienta Miner*. Use el siguiente comando para hacerlo:

  ::

    #encender parpadeo LED
    miner fault_light on

    #apagar parpadeo LED
    miner fault_light off

.. _miner_nightly:

====================================================================
Encender/apagar alimentaciones nocturnas usando la herramienta Miner
====================================================================

Puede encender las alimentaciones Nocturnas para poder actualizar a las últimas versiones nocturnas. Estas versiones son para arreglar problemas cruciales tan rápido como sea posible y por ello, no se hacen pruebas a fondo en estas versiones. Use estas versiones con precaución y solo si resuelve sus problemas. Para poder encender/apagar las alimentaciones nocturnas, use el siguiente comando:

  ::

    #encender alimentaciones nocturnas
    miner nightly_feeds on

    #apagar alimentaciones nocturnas
    miner nightly_feeds off

.. _miner_autoupgrade:

===========================================================
Encender/apagar auto-actualizar usando la herramienta Miner
===========================================================

Puede encender la característica de auto-actualizar, que actualizará el sistema automáticamente a la última versión. Esta característica está **encendida** por defecto, en transición desde un firmware **de serie** y **apagada** al actualizar desde versiones anteriores de **Braiins OS** o **Braiins OS+**. Para poder encender/apagar auto-actualizar, use el siguiente comando:

  ::

    #encender auto-actualizar
    miner auto_upgrade on

    #apagar auto-actualizar
    miner auto_upgrade off
