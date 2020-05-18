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

Braiins OS+ es un sistema operativo para mineros ASIC. Está basado en el producto `Braiins OS <https://braiins-os.com/community-edition>`_ y provee algoritmos adicionales propietarios para el autoajuste de
mineros. Cuando un usuario provee el consumo máximo de energía permitido en Vatios, el sistema optimizará automáticamente el proceso de minado para maximisar la tasa de hash. Este proceso funciona a través de un amplio espectro de entradas, permitiendole optimizar la mejor eficiencia posible o la tasa de hash máxima basada en consideraciones económicas. Pruebas internas muestran que para el Antminer S9, es posible alcanzar una eficiencia de 70J/THs o incluso mejor para el ajuste de pocos Vatios. Para consumo alto de energía, la tasa de hash puede mejorar 20% (comparado al Antminer S9, de serie a 13.5 TH/s ~ 94J/TH).

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

******************
Contacto y Soporte
******************

¿Tiene preguntas?
Nuestros equipos de desarrollo y soporte siempre están disponibles para ayudar.

Únase a nuestro grupo en Telegram:

  * `EN group <https://t.me/BraiinsOS>`_
  * `ES group <https://t.me/BraiinsOS_ES>`_
  * `RU group <https://t.me/BraiinsOS_RU>`_
  * `ZH group <https://t.me/BraiinsOS_ZH>`_

  También puede `enviar petición VIP <https://slushpool.kayako.com/en-us/conversation/new/11>`_ a nuestro equipo de soporte.


*********
Changelog
*********

20.04
---------------------------

Este lanzamiento mas que todo cubre asuntos encontrados por los usuarios, dificultades en la instalación/des-instalación y un problema mayor con el controlador I2C en las S9's. También tenemos versiones nightly que son fáciles de activar mediante la herramienta **bos**

  * Todos los tipos de hardware de minado

    * [característica] soporte a re-conexión - hemos implementado soporte a `client.reconnect` (stratum V1) y mensaje de re-conexión para V2
    * [característica] proceso de instalación/des-instalación (o **upgrade2bos** y **restore2factory**) (transición desde firmware de fábrica a Braiins OS o viceversa mejorado
    * [característica] un usuario de pool personalizado (`--pool-user`) se puede poner en la línea de comandos
    * [característica] los ajustes de pool ahora se migran automáticamente desde firmware de fábrica a la configuración de BOSminer. Puede desactivar la migración especificando (`--no-keep-pools`)
    * [característica] ahora proveemos una forma binaria de **upgrade2bos** (basada en pyinstaller) que contiene la última imagen de instalación Braiins OS
    * [característica] similarmente, **restore2factory** (basado en pyinstaller) está ahora disponible en forma binaria y ya no requiere descargar/encontrar el firmware correcto de fábrica.
    * [característica] el respaldo del firmware original que consume tiempo y espacio en disco ahora está deshabilitado por defecto (se puede activar con `--backup`)
    * [característica] mantener el nombre de host al hacer la instalación por primera vez ahora se maneja con 2 opciones `--keep-hostname` y `--no-keep-hostname` permitiendo la generación del nombre de host basado en una dirección MAC
    * [característica] soporte para activar/desactivar versiones nightly a sido integrado en la utilidad **bos** (y su contraparte **miner** heredada).
    * [característica] el sistema ahora provee **registros** (logs) cubriendo **un mayor tiempo** de operación de **BOSminer** por la activación de **rotación de registro** y compresión de '/var/log/syslog.old' cuando es mayor a 32 KiB
    * [fallo] la imagen de la tarjeta SD ahora contiene la autoridad de la llave pública que faltaba
    * [fallo] la tasa de rechazo ahora se muestra correctamente
    * [fallo] los mensajes stratum V1 desconocidos recibidos desde el servidor quedan ahora registrados para su diagnóstico

  * Antminer S9

    * [característica] ahora se muestra el estado del ajuste en la GUI. Se añadió el comando API TUNERSTATUS.
    * [fallo] algunos dispositivos estaban experimentando bloqueos aleatorios de bus con el controlador I2C y fallaban en comunicarse a los controladores de energía conectados al bus I2C compartido. Hemos encontrado que la causa era el núcleo del controlador Xilinx I2C que integramos al flujo de bits de la FPGA. De momento hemos cambiado al I2C presente en el SoC y el flujo de bits solo envía la señal del periférico (IIC0) a los pines FPGA correspondientes.

20.03
---------------------------

  * Todos los tipos de hardware de minado

    * [característica] el archivo de configuración permite especificar un límite de energía para la PSU en
      el algoritmo de autoajuste que tomará en cuenta para maximizar los TH/W producidos por el dispositivo
      de minado.

  * Antminer S9

    * [característica] Autoajuste basado en limite de energía especificado por el usuario

*******************
Problemas Conocidos
*******************

A continuación se listan problemas que se sabe existen en la versión liberada.

20.03 (Actualizado 3/30/2020)
-----------------------------

  * GUI

   * La linea de referencia en el gráfico tasa de hash tiene un valor incorrecto para el
     promedio de tasa hash nominal. El problema solo aparece cuando hay menos de 3 cadenas
     hash funcionando.
   * La tasa de rechazo está multiplicada por 100. Por ejemplo cuando la tasa de rechazo es
     0.1%, muestra entonces 10%.

  * Configuración

    * La instalación por tarjeta SD reportará una llave de autenticación Stratum V2 faltante en la
      sección Miner/Configuration (Error: missing upstream authority key for securing stratum2+tcp
      connection in pool"). El usuario puede configurar la conexión (incluyendo la llave) en la
      configuración, o directamente en el archivo ``/etc/bosminer.toml``.
      
