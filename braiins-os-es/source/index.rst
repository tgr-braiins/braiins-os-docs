.. toctree::
   :hidden:
   :maxdepth: 3
   :caption: Table of Contents:
   :glob:

   self
   Setup/index*
   Configuration/index*
   Basic User's Guide/index*
   Development/index*

---------------

############
Introducción
############

Braiins OS es un sistema operativo completamente código abierto para mineros ASIC. Fue el primer firmware en implementar overt AsicBoost en 2018, y ahora caracteriza la implementación de un nuevo protocolo de minado, Stratum V2. Adicionalmente, BraiinsOS trabaja en conjunto con nuestro nuevo componente de software, BOSminer, el cual hemos escrito desde cero en lenguaje Rust como reemplazo del desactualizado CGminer.

Los dispositivos actualmente compatibles son Antminer S9, s9i, S9j, S17, S17 Pro, S17+, T17 y T17+ de Bitmain.  El soporte de Antminer S17e, T17e y Whatsminer M20S está previsto para un futuro próximo.

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
 * Mecanismo de Auto-actualización

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

*********
Changelog
*********

20.09.1
---------------------------

Esta es una versión de corrección de fallos.

* Todos los tipos de hardware

  * [característica] hemos deshabilitado la protección rebind en DNSmasq para recuperar el comportamiento original en la resolución de nombres. Lo que esto significa es que el servidor DNS de una granja de minado podrá servir respuestas que apunten a rangos de IP (locales). Esto mejora la experiencia de usuario si la granja tiene un proxy stratum local disponible por nombre..
  * [característica] soporte para minería opcional. Los mensajes stratum {ping/pong} que algunos pool utilizan para comprobar que el minero está vivo
  * [fallo] solución alternativa para otra-implementación-rota de stratum V1 ha sido desplegada. El problema es que algunas implementaciones de  stratum V1 no marcan el resultado 'nulo' como una respuesta que lleva error sinó que coloca varias cosas en ella (ej: falso). El cliente stratum aborta la conexión en ese caso. Hemos colocado esto como mensaje de advertencia en el registro y el cliente ignora semejantes anomalías y puede extraer la carga útil de ella
  * [fallo] versión de formato bosminer.toml es ahora correctamente migrada

* Antminer S17

  * [característica] limite de temperatura caliente ha sido reducido a 100 C
  * [característica de diagnóstico] el último error de una máquina ahora por defecto es enviado a nuestro servidor de registro. Esto es para simplificar cualquier problema de diagnostico en la S17. Si esta característica temporal no es deseable, puede deshabilitarse en /etc/init.d/bosminer reemplazando "PROG=/usr/bin/bosminer-panic-wrapper" con "PROG=/usr/bin/bosminer"

20.09
---------------------------

Esta liberación trae soporte para la Antminer S17 y S17 Pro e incluye una versión de mantenimiento para la familia Antminer S9.

* Todos los tipos de hardware de minado

  * [característica] la temperatura de chip es derivada de la temperatura de la tarjeta en caso de sensor fallido
  * [característica] DNSmasq integrado para cache DNS local para reducir el número de peticiones a nombres de dominio
  * [fallo] arreglado el problema con la Tasa Fija Compartida y la Cuota, donde la tasa de hash no era dividida correctamente
  * [característica] hemos mejorado el algoritmo de retroceso para pools inestables donde el pool se considera estable solo si corre sin errores por una hora

* Antminer S17

  * [nota] Braiins OS para Antminer S17 no diferencia entre una versión de S17 "clásico" y el Pro ya que ambos tipos de hardware son casi idénticos

20.06
---------------------------

Esta liberación apunta a mejorar la usabilidad de Braiins OS y la caja de herramientas BOS al implementar nuevas características y arreglar los problemas mas críticos.

  * Todos los tipos de hardware de minado

    * [solución] Soporte a pools basados en yiimp (ej: prohashing) que incorrectamente envían una máscara de versión rodante que empieza con '0x', lo cual no cumple con la especificación BIP-310
    * [característica] Soporte a contraseñas stratum V1 ya que son usados por varios pool para cambiar de algoritmo u otros hacks
    * [característica] Implementación de mecanismo de auto-actualización. La maquina revisará periódicamente si hay una nueva versión de Braiins OS y actualizará a ella automáticamente cuando la encuentre. Esta característica se enciende por defecto al cambiar desde el firmware de fábrica, pero debe ser encendida manualmente al actualizar desde versiones anteriores de Braiins OS
    * [característica] Mejorado sistema de registro con la implementación de rotación de registro. Los registros del sistema ahora son comprimidos automáticamente y guardados en la NAND del dispositivo lo cual permite almacenar registros mas largos
    * [característica] Actualizada la Caja de herramientas BOS, que ahora puede correr comandos personalizados en lote
    * [fallo] La instalación NAND desde una tarjeta SD ahora migra propiamente la configuración de la tarjeta SD, en lugar de la del viejo sistema en la NAND
    * [fallo] Arreglado el problema con *bosminer.toml* al estar vacío cuando se apaga el minero antes de que el sistema limpie el búfer
    * [fallo] El botón IP report ahora funciona correctamente

  * Antminer S9

    * [característica] Hemos vuelto a cambiar al núcleo Xilinx I2C para la comunicación con los controladores de voltaje y se extendió con un filtro de errores para los ambientes ruidosos
    * [característica] La línea UART Rx para comunicarse con los chips de hasheo fue extendida con filtro de errores


20.04
---------------------------

Esta liberación cubre mayoritariamente asuntos encontrados por los usuarios, dificultades de instalación/desinstalación y 1 problema mayor con la controladora I2C en las S9's. También, ahora tenemos versiones nocturnas que son fáciles de habilitar mediante la herramienta **bos**.

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

    * [fallo] algunos dispositivos estaban experimentando bloqueos aleatorios en el bus del controlador I2C y fallaban en comunicarse con los controladores de energía en la tarjeta de hash conectada al bus I2C compartido. Encontramos que la causa era el núcleo del controlador Xilinx I2C que hemos integrado al flujo de bits en la FPGA. Hemos cambiado al I2C presente en el SoC y el flujo de bits solo enruta la señal del periférico (IIC9) a los pins FPGA correspondientes.

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
