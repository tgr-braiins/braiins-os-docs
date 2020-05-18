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

Braiins OS es un sistema operativo completamente código abierto para mineros ASIC. Fue el primer firmware
en implementar overt AsicBoost en 2018, y ahora caracteriza la implementación de un nuevo protocolo de
minado, Stratum V2. Adicionalmente, BraiinsOS trabaja en conjunto con nuestro nuevo componente de
software, BOSminer, el cual hemos escrito desde cero en lenguaje Rust como reemplazo del desactualizado
CGminer.

Los dispositivos actualmente soportados son Antminer S9, S9i y S9j de Bitmain. Soporte para Antminer S17
está planificado en el futuro próximo.

***************
Características
***************

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
