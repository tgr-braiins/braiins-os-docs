
.. _manager:

###################
Braiins OS+ Manager
###################

.. contents::
  :local:
  :depth: 1

Braiins OS+ Manager (Gestor Braiins OS+) es una plataforma basada en la nube que le permite configurar sus dispositivos de minería corriendo el firmware Braiins OS+ de forma remota y también recibe continuamente datos sobre su rendimiento.

Los datos se están enviando a través del protocolo Stratum V2 y usan el mismo canal utilizado para recaudar la comisión de desarrollo, para no agobiar su red con otra conexión.

El principal objeto dentro de Braiins OS+ Manager es un grupo de dispositivos denominado Farm (Granja). Cada Granja tiene su Farm ID. Esta es una frase que debe colocar a sus dispositivos Braiins OS+ si desea conectarlos a la Granja. Una vez conectados, los dispositivos enviarán sus datos de rendimiento al Braiins OS+ Manager cada 120 segundos.

Cada Granja tiene su configuración. Cuando actualiza y guarda la nueva configuración, esta será propagada al dispositivo en lo que Manager reciba la siguiente carga con los datos de rendimiento desde el dispositivo. Ya que la misma configuración es aplicada a todos los dispositivos dentro de una Granja, **recomendamos encarecidamente que cree granjas separadas por tipo de dispositivo** o simplemente agrupar los dispositivos (incluso del mismo tipo) si desea configurarlos de manera diferente.

***********
Inscribirse
***********

Para usar Braiins OS+ Manager, simplemente `inscríbase aquí <https://manager.braiins.com/#/register>`_.

Luego de introducir su dirección de correo, le estaremos enviando un correo de confirmación. Después de seguir el enlace del correo, se le pedirá que escoja una contraseña y configurar su autenticación de dos pasos (2FA).

****************
Crear una Granja
****************

Cuando haya ingresado, empiece creando una Granja:

1. Abra el diálogo de creación de Granja haciendo clic al símbolo '+'.
2. Escoja un nombre que desee usar para su granja. Puede cambiar luego el nombre.
3. Ingrese las credenciales de minería. Podrá cambiar luego las credenciales y también añadir otros pools.
4. La Farm ID para su granja ha sido creada.

La Farm ID es una frase que debe poner en sus dispositivos Braiins OS+ que desee conectar a la Granja.

**********************************
Connectar Dispositivos a la Granja
**********************************

Para conectar un dispositivo a su Granja Braiins OS+ Manager, debe:

  - correr Braiins OS+ 21.04 o posterior en los dispositivos seleccionados, 
  - poner la Farm ID (bos_mgmt_id) en los dispositivos seleccionados.

Esto puede realizarse usando la BOS Toolbox (Caja de Herramientas) siguiendo los pasos a continuación.
**Nota importante:** Descargue la última versión de la BOS Toolbox `desde aquí <https://braiins.com/os/plus/download>`_, antes de usar los comandos abajo.

**Instalar/Actualizar Braiins OS+ y poner el Farm ID**

Si sus dispositivos no están corriendo Braiins OS+ 21.04 (o mayor) aun, puede instalar el último Braiins OS+ y poner el Farm ID en un solo paso usando el comando de instalación de la BOS Toolbox con el argumento `--bos-mgmt-id`.
Reemplace el marcador “HOSTS” con la dirección IP o un archivo de texto que contenga IPs múltiples (uno por línea para instalación por lote). Reemplace “FARM_ID” con su Farm ID.

::

    #Windows
    bos-toolbox.bat install --bos-mgmt-id FARM_ID HOSTS

    #Linux
    ./bos-toolbox install --bos-mgmt-id FARM_ID HOSTS
    
**Poner el Farm ID a una instalación existente de Braiins OS+**

Si sus dispositivos ya están corriendo Braiins OS+ 21.04 (o mayor), puede poner el Farm ID en un solo paso usando el comando de actualización de la BOS Toolbox con el argumento `--bos-mgmt-id`.
Reemplace el marcador “HOSTS” con la dirección IP o un archivo de texto que contenga IPs múltiples (uno por línea para instalación por lote). Reemplace “FARM_ID” con su Farm ID.

::

    #Windows
    bos-toolbox.bat update --bos-mgmt-id FARM_ID HOSTS

    #Linux
    ./bos-toolbox update --bos-mgmt-id FARM_ID HOSTS

********************
Configurar la Granja
********************

**Configuración del Nombre de Equipo**

Hay tres opciones diferentes en como los dispositivos incluidos en una Granja pueden identificarse a si mismos en la lista de dispositivos del Gestor y del lado del pool:

  - Por Dispositivo (NOMBREGRANJAxIP4) - el nombre de equipo consiste en el nombre de la Granja y el cuarto segmento de la dirección IP de un dispositivo
  - Por Dispositivo (NOMBREGRANJAxIP3xIP4) - predeterminado - además, el tercer segmento de la dirección IP también se incluye
  - Por Dispositivo (NOMBREGRANJAxIP2xIP3xIP4) - además, el segundo segmento de la dirección IP también se incluye
  - Por Dispositivo (ID-DISPOSITIVO / INVALIDACIÓN MANUAL) - Puede escojer su nombre de equipo deseado para cada dispositivo haciendo clic en la rueda dentada a su lado. Por defecto, la ID de Dispositivo se usa como nombre de equipo en este modo.
  También puede cambiar el delimitador y usar `_` en lugar de `x`. Sin embargo, tenga en cuenta que algunos pool no aceptan subrayas en el nombre de equipo, y escojer `_` como un delimitador podría ocasionar problemas para conectarse con dicho pool.
  - Único (NOMBREGRANJA) - Todos los dispositivos usan el mismo nombre de equipo (nombre de la Granja). Esto significa que la tasa de hash se agregará a un solo equipo del lado del pool..

El nombre de equipo también puede cambiarse en cualquier momento.

**Configuración de Minería**

La configuración de minería disponible en la pestaña "Configuración" incluye un sub conjunto de la `configuración general de Braiins OS\+ <https://docs.braiins.com/os/plus-es/Configuration/index_configuration.html>`_ disponible en los dispositivos individualmente. Por ejemplo, las opciones de las cadenas individuales de hash no están disponibles aquí ya que solo tiene sentido desde una perspectiva individual. Aparte de eso, todas las opciones importantes para configurar ajuste, target temperature, o escalamiento de energía dinámico están presentes.

La configuración requiere que introduzca credenciales para al menos un pool (se hace durante el proceso de creación de la granja). Los otros campos de configuración son opcional. Si no provee ningún valor, cada Dispositivo en una Granja simplemente usará su predeterminado. Su comportamiento equivale a dejar la configuración de un solo dispositivo Braiins OS+ vacío.

Al hacer clic en el botón de Guardar, la nueva configuración  es propagada a los dispositivos incluidos en la Granja casi inmediatamente - típicamente dentro de un segundo.

**Cambios Locales**

Los cambios locales (en el minero) son siempre sobre-escritos por el Gestor. Si desea tomar control del dispositivo, des-conéctelo de la granja primero.

**************************************
Desconectar Dispositivos de una Granja
**************************************

Si desea desconectar los dispositivos de la Granja y configurar individualmente, puede hacerlo simplemente eliminando el archivo bos_mgmt_id de los dispositivos elegidos. Para múltiples dispositivos, esto puede hacerse con la BOS Toolbox (Caja de Herramientas BOS) de la siguiente forma:

::

    #Windows
    bos-toolbox.bat command -o HOSTS "rm /etc/bos_mgmt_id && /etc/init.d/bosminer restart"

    #Linux
    ./bos-toolbox command -o HOSTS "rm /etc/bos_mgmt_id && /etc/init.d/bosminer restart"

*********************
Solución de problemas
*********************

**1. Revise si el dispositivo corre Braiins OS+ 21.04 o posterior**

  - Usando GUI: la versión se muestra al pie de página
  - Usando CLI: la versión se muestra en la pantalla de bienvenida de SSH

**Arreglo**: si sus dispositivos corren una versión anterior de Braiins OS+, actualice primero sus dispositivos

**2. Revise si la Farm ID ha sido correctamente configurada**

Usando GUI:

  - vaya a Status -> Overview -> Miner
  - Revise si está la Farm ID correcta, en el campo *BOS Management ID*.
  - Si el campo no aparece, no hay configurado Farm ID en el dispositivo.

Usando CLI:

  - `cat /etc/bos_mgmt_id`
  - el comando debe devolver la Farm ID

**Arreglo**: si la ID no está o es incorrecta, intente ponerla de nuevo

**3. Reinicie su dispositivo**

¿Aun no funciona? Reinicie su dispositivo.

  - Usando GUI: System -> Reboot -> Perform Reboot
  - Usando CLI: `reboot`

**4. Contacte al equipo de soporte**

Si nada de lo mencionado arriba ayuda, `envíe un ticket de soporte <https://help.slushpool.com/es/support/tickets/new>`_. 

Para una solución efectiva de problemas, incluya la siguiente información:

  - **Hardware ID** (Status -> Overview)
  - **System Log** (Status -> System Log)
