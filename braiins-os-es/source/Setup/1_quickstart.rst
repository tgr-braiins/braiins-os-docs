#############
Inicio Rápido
#############

.. contents::
  :local:
  :depth: 2

*******************
Instalar Braiins OS
*******************

==================================================
Corriendo firmware de serie liberado antes de 2019
==================================================

**Instalar un solo dispositivo**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/PjCZIVkTuLI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Puede instalar fácilmente Braiins OS mediante el proceso de actualización en la interfaz web. Para hacerlo, siga los pasos a continuación:

  * Descargue el **Paquete Web** desde nuestro `sitio web <https://braiins-os.com/open-source/download/>`_.
  * Ingrese a su minero y vaya a la sección *System -> Upgrade*.
  * Suba el paquete de la imagen descargada y escriba la imagen.

Braiins OS se instalará en el minero. La configuración de red (ej: Dirección IP estática) y la configuración del pool y usuario será migrada automáticamente a Braiins OS.

**Instalar en múltiples-dispositivos**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/0RTKiqwJ4to" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

La instalación de Braiins OS puede hacerse fácilmente usando la caja de herramientas BOS. Para hacerlo, siga los pasos a continuación:

  * Descargue la **Caja de Herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/open-source/download/>`_.
  * Cree un nuevo archivo de texto, cambie la extensión ".txt" a ".csv" e inserte las direcciones IP en las que desea ejecutar los comandos. Coloque el archivo en el directorio donde se encuentra la Caja de herramientas BOS. ¡Use solo una dirección IP por línea!
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc).
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.exe install listaDeMineros.csv

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox install listaDeMineros.csv

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Braiins OS+ será instalado en los mineros. La configuración de red (ej: Dirección IP estática) y la configuración del pool y usuario será migrada automáticamente a Braiins OS+ y el autoajuste será activado.

Para mayor información acerca de este proceso, y para mas opciones visite las secciones :ref:`bosbox` y :ref:`bosbox_install`.

====================================================
Corriendo firmware de serie liberado en 2019 o luego
====================================================

**(Solo Antminer S9) Desbloquear SSH e instalar usando la Caja de Herramientas BOS**

En 2019, la conexión SSH fue bloqueada y una verificación de firma en la interfaz web previene el uso de firmware de terceros. Para instalar Braiins OS+ en máquinas con SSH bloqueado, siga los pasos siguientes:

  * Descargue la **Caja de Herramientas BOS** de nuestro `sitioweb <https://es.braiins.com/os/plus/download>`_.
  * Cree un nuevo archivo de texto, cambie la terminación ".txt" a ".csv" e inserte las direcciones IP en donde quiere ejecutar los comandos. Coloque ese archivo en la carpeta donde se encuentra la Caja de Herramientas BOS. **¡Use solo una dirección IP por línea!**
  * Una vez descargada la Caja de Herramientas BOS, abra su interprete de linea-de-comandos (ej. CMD en Windows, Terminal en Ubuntu, etc.)
  * Reemplace el marcador *RUTA_HACIA_LA_CAJA_DE_HERRAMIENTAS_BOS* en el comando de abajo por la ruta actual al archivo donde guardó la Caja de Herramientas BOS Toolbox. Luego cambie a esa ruta de archivo corriendo el comando: ::

      cd RUTA_HACIA_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con el nombre de archivo en el comando de abajo y corra el comando apropiado para su sistema operativo:

    Terminal de comandos en **Windows**: ::

      #desbloquear e instalar Braiins OS en las máquinas
      bos-toolbox.exe install --password root listaDeMineros.csv

    Terminal de comandos en **Linux**: ::

      #desbloquear e instalar Braiins OS en las máquinas
      ./bos-toolbox install --password root listaDeMineros.csv

    **Nota:** *al usar la caja de herramientas BOS para Linux, debe hacerla ejecutable con el siguiente comando (solo necesita hacerlo una vez):* ::

      chmod u+x ./bos-toolbox

Braiins OS se instalará en el minero. La configuración de red (ej. Dirección IP estática) y la configuración de usuario y pool será migrada automáticamente a Braiins OS.

**método SD**

Si está corriendo firmware de serie liberado en 2019 o luego, la única forma de instalar Braiins OS es insertando la tarjeta SD con Braiins OS escrito en ella. En 2019, la conexión SSH fue bloqueada y la verificación de firma en la interfaz web previene el uso de firmwares de terceros.

Para instalar Braiins OS por el método de tarjeta SD, siga los pasos a continuación:

 * Descargue la imagen para tarjeta SD desde nuestro `sitio web <https://braiins-os.com/open-source/download/>`_.
 * Escriba la imagen descargada a una tarjeta SD (ej: usando `Etcher <https://etcher.io/>`_). *Nota: Una simple copia a la tarjeta SD no funcionará. La tarjeta SD debe ser escrita!*
 * **(Solo Antminer S9)** Ajuste los jumpers para arrancar desde la tarjeta SD (en lugar de la memoria NAND), como se muestra a continuación.

  .. |pic1| image:: ../_static/s9-jumpers.png
      :width: 45%
      :alt: S9 Jumpers

  .. |pic2| image:: ../_static/s9-jumpers-board.png
      :width: 45%
      :alt: S9 Jumpers Board

  |pic1|  |pic2|

 * Inserte la tarjeta SD en el dispositivo, luego inicie el dispositivo.
 * Luego de un momento, podrá acceder la interfaz de Braiins OS a través de a dirección IP del dispositivo.
 * *[Opcional]:* Puede ahora instalar Braiins OS a la memoria interna (NAND) siguiendo la sección :ref:`sd_nand_install`.

Para mas información acerca de este proceso, y para mas opciones visite las secciones :ref:`sd` e :ref:`sd_install`.

*********************
Actualizar Braiins OS
*********************

**Actualizar un solo dispositivo**

El firmware periódicamente revisa la disponibilidad de una nueva versión. en caso de que una nueva versión esté disponible, aparecerá un botón azul **Upgrade** en la interfaz web al lado derecho de la barra superior. Proceda a presionar sobre el botón y confirme para iniciar la actualización.

Alternativamente, puede actualizar la información del repositorio manualmente presionando el botón *Update lists* en el menú System > Software. En caso de que falte el botón, intente refrescar la página. Para activar el proceso de actualización, escriba ``firmware`` en el campo *Download and install package* y presione *OK*.

**Actualizar múltiples dispositivos**

Actualizar Braiins OS en múltiples dispositivos a la vez puede hacerse fácilmente usando la **Caja de Herramientas BOS**. Para hacerlo, siga los pasos a continuación:

  * Descargue la **Caja de Herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/open-source/download/>`_.
  * Cree un nuevo archivo de texto, cambie la extensión ".txt" a ".csv" e inserte las direcciones IP en las que desea ejecutar los comandos. ¡Use solo una dirección IP por línea! Coloque el archivo en el directorio donde se encuentra la Caja de herramientas BOS.
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc).
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.exe update listaDeMineros.csv

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox update listaDeMineros.csv

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Este comando buscará una actualización para los mineros especificados en *listaDeMineros.csv* y los actualizará si hay una nueva versión de firmware.

Para mas información acerca de este proceso, y para mas opciones visite las secciones :ref:`bosbox` y :ref:`bosbox_update`.

**********************
Desinstalar Braiins OS
**********************

**Desinstalar un solo dispositivo**

Puede desinstalar fácilmente Braiins OS de un solo dispositivo usando la **Caja de Herramientas BOS**. Para hacerlo, siga los pasos a continuación:

  * Descargue la **Caja de Herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/open-source/download/>`_.
  * Una vez descargada la Caja de herramientas BOS, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc).
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *DIRECCIÓN_IP* con la dirección IP (o nombre anfitrión) de su minero en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.exe uninstall DIRECCIÓN_IP

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox uninstall DIRECCIÓN_IP

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Esto le regresará al firmware de serie. Instalará automáticamente una versión mas vieja donde SSH no está bloqueado, para que pueda acceder a su minero remotamente.

**Desinstalar en múltiples-dispositivos**

Puede desinstalar Braiins OS fácilmente en múltiples dispositivos usando la **Caja de Herramientas BOS**. Para hacerlo, siga los pasos a continuación:

  * Descargue la **Caja de Herramientas BOS** desde nuestro `sitio web <https://braiins-os.com/open-source/download/>`_.
  * Cree un nuevo archivo de texto en su editor de texto e inserte las direcciones IP en donde desea ejecutar los comandos. ¡Use solo una dirección IP por línea! (Nota puede encontrar la dirección IP en la interfaz web de Braiins OS yendo a *Status -> Overview*.) Luego guarde el archivo en el mismo directorio donde guardó la Caja de herramientas BOS y cambie la extensión ".txt" a ".csv".
  * Una vez descargada la Caja de herramientas BOS y guardado el archivo .csv, abra su interprete de línea de comandos (ej: CMD en windows, Terminal en Ubuntu, etc).
  * Reemplace el marcador *RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS* del comando siguiente con la verdadera ruta de archivo donde guardó la Caja de Herramientas BOS. Luego cámbiese a esa ruta ejecutando el comando: ::

      cd RUTA_A_LA_CAJA_DE_HERRAMIENTAS_BOS

  * Ahora reemplace el marcador *listaDeMineros.csv* con su nombre de archivo en el comando siguiente y ejecute el comando apropiado para su sistema operativo:

    Para terminal de comandos en **Windows**: ::

      bos-toolbox.exe uninstall listaDeMineros.csv

    Para terminal de comandos en **Linux**: ::

      ./bos-toolbox uninstall listaDeMineros.csv

    **Nota:** *al usar la la Caja de herramientas BOS en Linux, necesitará hacerla ejecutable mediante el comando siguiente (esto solo debe hacerse una vez):* ::

      chmod u+x ./bos-toolbox

Esto le regresará al firmware de serie. Instalará automáticamente una versión mas vieja donde SSH no está bloqueado, para que pueda acceder a su minero remotamente.

Para mayor información acerca de este proceso, y para mas opciones visite las secciones :ref:`bosbox` y :ref:`bosbox_uninstall`.

*********************
Configurar Braiins OS
*********************

**Configurar un solo dispositivo**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/PjCZIVkTuLI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Puede configurar Braiins OS en un solo dispositivo usando la **interfaz web** del minero o directamente en el archivo de configuración ubicado en **/etc/bosminer.toml** (para mas información, visite la sección :ref:`configuration`).

**Configurar múltiples-dispositivos**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/4jQCu6yuXUA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Puede configurar fácilmente Braiins OS+ en múltiples dispositivos usando la **Caja de herramientas BOS**. Para hacerlo, siga los pasos en la sección :ref:`bosbox_configure`.
