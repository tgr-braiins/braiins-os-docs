.. toctree::
   :hidden:
   :maxdepth: 3
   :caption: Table of Contents:
   :glob:

   self
   Setup/index*
   Configuration/index*
   Basic User's Guide/index*

---------------

############
Introducción
############

Braiins OS+ es un sistema operativo para mineros ASIC. Está basado en el producto `Braiins OS <https://braiins-os.com/community-edition>`_ y provee algoritmos adicionales propietarios para el autoajuste de mineros. Cuando un usuario provee el consumo máximo de energía permitido en Vatios, el sistema optimizará automáticamente el proceso de minado para maximizar la tasa de hash. Este proceso funciona a través de un amplio espectro de entradas, permitiéndole optimizar la mejor eficiencia posible o la tasa de hash máxima basada en consideraciones económicas. Pruebas internas muestran que para el Antminer S9, es posible alcanzar una eficiencia de 70J/THs o incluso mejor para el ajuste de pocos Vatios. Para consumo alto de energía, la tasa de hash puede mejorar 20% (comparado al Antminer S9, de serie a 13.5 TH/s ~ 94J/TH).

Los dispositivos actualmente soportados son Antminer S9, S9i y S9j de Bitmain. Soporte para Antminer S17 está planificado en el futuro próximo.

***************
Características
***************

 * Lo último en optimización de autoajuste para maximizar la tasa de hash o la eficiencia
 * Sistema operativo de código abierto
 * Implementación Stratum V2 con eficiencia de datos mejorada y prevención al secuestro de tasa hash
 * Reemplazo de CGminer (BOSminer) escrito desde cero en lenguaje Rust
 * Inicio rápido (5-7 segundos)
 * Sin bloqueos aleatorios por comportamiento indefinido
 * Instalación en masa
 * Actualizaciones automáticas con el sistema de paquetes estándar opkg
 * Control de ventilador completamente personalizable (permite el enfriamiento por inmersión)
 * Monitoreo avanzado para prevenir el sobrecalentamiento y otros problemas
 * Mecanismo de Auto-actualización
 * Escalado Dinámico de Energía, lo cual baja el límite de energía en caso de altas temperaturas, para minado continuo

******************
Contacto y Soporte
******************

¿Tiene preguntas?
Nuestros equipos de desarrollo y soporte siempre están disponibles para ayudar.

Únase a nuestro grupo en Telegram:

  * `EN group <https://t.me/BraiinsOS>`_
  * `ES group <https://t.me/BraiinsOS_ES>`_
  * `FA group <https://t.me/BraiinsOS_SlushPool_FA>`_
  * `RU group <https://t.me/BraiinsOS_RU>`_
  * `ZH group <https://t.me/BraiinsOS_ZH>`_

  También puede `enviar petición VIP <https://slushpool.kayako.com/en-us/conversation/new/11>`_ a nuestro equipo de soporte.


*********
Changelog
*********

20.06
---------------------------

Esta liberación apunta a mejorar la usabilidad de Braiins OS+ y la caja de herramientas BOS+ al implementar nuevas características y arreglar los problemas mas críticos.

  * Todos los tipos de hardware de minado

    * [solución] Soporte a pools basados en yiimp (ej: prohashing) que incorrectamente envían una máscara de versión rodante que empieza con '0x', lo cual no cumple con la especificación BIP-310
    * [característica] Soporte a contraseñas stratum V1 ya que son usados por varios pool para cambiar de algoritmo u otros hacks
    * [característica] Implementación de mecanismo de auto-actualización. La maquina revisará periódicamente si hay una nueva versión de Braiins OS y actualizará a ella automáticamente cuando la encuentre. Esta característica se enciende por defecto al cambiar desde el firmware de fábrica, pero debe ser encendida manualmente al actualizar desde versiones anteriores de Braiins OS
    * [característica] Mejorado sistema de registro con la implementación de rotación de registro. Los registros del sistema ahora son comprimidos automáticamente y guardados en la NAND del dispositivo lo cual permite almacenar registros mas largos
    * [característica] Actualizada la Caja de herramientas BOS, que ahora puede correr comandos personalizados en lote
    * [fallo] La instalación NAND desde una tarjeta SD ahora migra propiamente la configuración de la tarjeta SD, en lugar de la del viejo sistema en la NAND
    * [fallo] Arreglado el problema con *bosminer.toml* al estar vacío cuando se apaga el minero antes de que el sistema limpie el búfer
    * [fallo] El botón IP report ahora funciona correctamente
    * [característica] El sub-sistema de autoajuste ahora almacena perfiles de rendimiento en /etc/bosminer-autotune.json. Los perfiles de rendimiento se graban para cada nivel de energía e índice de tarjeta
    * [característica] Escalamiento de Energía Dinámico ahora baja el limite de energía (powerlimit) de un minero una cantidad definida por el usuario si el dispositivo alcanza la *Hot Temperature* (temperatura caliente). Al alcanzar el límite de energía mínimo, el minero se apaga para enfriarse. El minero vuelve a trabajar al limite de energía original luego de un período de tiempo definido por el usuario

  * Antminer S9

    * [característica] Hemos vuelto a cambiar al núcleo Xilinx I2C para la comunicación con los controladores de voltaje y se extendió con un filtro de errores para los ambientes ruidosos
    * [característica] La línea UART Rx para comunicarse con los chips de hasheo fue extendida con filtro de errores


20.04
---------------------------

Esta liberación cubre mayoritariamente asuntos enfrentados por los usuarios, dificultades de instalación/desinstalación y 1 problema mayor con la controladora I2C en las S9's. También, ahora tenemos versiones nocturnas que son fáciles de habilitar mediante la herramienta **bos**.

  * Todos los tipos de hardware de minado

    * [característica] soporte a reconectar - hemos implementado soporte para `client.reconnect` (stratum V1) y mensaje de re-conexión para V2
    * [característica] el proceso de instalación/des-instalación (alias **upgrade2bos** y **restore2factory**) (transición desde firmware de fábrica a Braiins OS o vice versa) ha sido mejorado
    * [característica] un usuario de pool personalizado (`--pool-user`) puede ponerse en la línea de comandos
    * [característica] la configuración del pool en el firmware de fábrica ahora se migra automáticamente a la configuración de BOSminer. La migración puede deshabilitarse especificando (`--no-keep-pools`)
    * [característica] ahora proveemos **upgrade2bos** (basado en pyinstaller) en forma binaria que contiene la última imagen de instalación de Braiins OS
    * [característica] similarmente, **restore2factory** (basado en pyinstaller) está ahora disponible en forma binaria y ya no requiere descargar/ubicar el firmware de fábrica correcto
    * [característica] el respaldo que consume espacio de disco y tiempo del firmware original ahora está deshabilitado por defecto (puede habilitarse con `--backup`)
    * [característica] mantener el nombre del host al realizar la instalación por primera vez ahora se maneja por 2 opciones `--keep-hostname` y `--no-keep-hostname` permitiendo forzar la invalidación y generación automática de un nombre host basado en la dirección MAC
    * [característica] soporte para la activación/des-activación de versiones nocturnas ha sido integrado a la utilidad **bos** (y su contraparte heredada **miner**)
    * [característica] el sistema ahora provee **registros** abarcando **mayor tiempo** la operación de **BOSminer** debido a la activación de la **rotación de registro** y la compresión de '/var/log/syslog.old' cuando es mayor a 32 KiB
    * [fallo] la imagen de la tarjeta SD ahora contiene la llave pública oficial de slushpool que estaba faltando
    * [fallo] la tasa de rechazo ahora se muestra correctamente
    * [fallo] los mensajes stratum V1 desconocidos recibidos desde el servidor quedan ahora registrados para su diagnóstico

  * Antminer S9

    * [característica] Se muestra el estado del Ajuste en la GUI. Añadido comando API TUNERSTATUS API.
    * [fallo] algunos dispositivos estaban experimentando bloqueos aleatorios en el bus del controlador I2C y fallaban en comunicarse con los controladores de energía en la tarjeta de hash conectada al bus I2C compartido. Encontramos que la causa era el nucleo del controlador Xilinx I2C que hemos integrado al flujo de bits en la FPGA. Hemos cambiado al I2C presente en el SoC y el flujo de bits solo enruta la señal del periférico (IIC9) a los pins FPGA correspondientes.

20.03
---------------------------

  * Todos los tipos de hardware de minado

    * [característica] el archivo de configuración permite especificar un límite de energía para la PSU en el algoritmo de autoajuste que tomará en cuenta para maximizar los TH/W producidos por el dispositivo de minado.

  * Antminer S9

    * [característica] Autoajuste basado en limite de energía especificado por el usuario

*******************
Problemas Conocidos
*******************

A continuación se listan problemas que se sabe existen en la versión liberada.

20.03 (Actualizado 3/30/2020)
-----------------------------

  * GUI

   * La linea de referencia en el gráfico tasa de hash tiene un valor incorrecto para el promedio de tasa hash nominal. El problema solo aparece cuando hay menos de 3 cadenas
     hash funcionando.
   * La tasa de rechazo está multiplicada por 100. Por ejemplo cuando la tasa de rechazo es 0.1%, muestra entonces 10%.

  * Configuración

    * La instalación por tarjeta SD reportará una llave de autenticación Stratum V2 faltante en la sección Miner/Configuration (Error: missing upstream authority key for securing stratum2+tcp connection in pool"). El usuario puede configurar la conexión (incluyendo la llave) en la configuración, o directamente en el archivo ``/etc/bosminer.toml``.
