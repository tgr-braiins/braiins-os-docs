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
   Braiins OS+ Manager/index*

---------------

.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>


############
Introducción
############

Braiins OS+ es un sistema operativo para mineros ASIC. Está basado en el producto `Braiins OS <https://braiins-os.com/community-edition>`_ y provee algoritmos adicionales propietarios para el autoajuste de mineros. Cuando un usuario provee el consumo máximo de potencia permitido en Vatios, el sistema optimizará automáticamente el proceso de minado para maximizar la tasa de hash. Este proceso funciona a través de un amplio espectro de entradas, permitiéndole optimizar la mejor eficiencia posible o la tasa de hash máxima basada en consideraciones económicas. Pruebas internas muestran que para el Antminer S9, es posible alcanzar una eficiencia de 70J/THs o incluso mejor para el ajuste de pocos Vatios. Para potencia de alto consumo, la tasa de hash puede mejorar 20% (comparado al Antminer S9, de serie a 13.5 TH/s ~ 94J/TH).

Los dispositivos actualmente compatibles son Antminer S9, s9i, S9j, S17, S17 Pro, S17+, T17, T17+, S17e y T17e de Bitmain. El soporte de Antminer x19 y Whatsminer M20S está previsto para un futuro próximo.

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
 * Escalado de Potencia Dinámico, lo cual baja el límite de potencia en caso de altas temperaturas, para minado continuo
 * **Soporte a BTC Tools**

  * Braiins OS+ tiene soporte en BTC Tools - la herramienta para mineros de gestión por lotes. Soporta las nuevas versiones de Braiins OS+ actualice con la caja de herramientas si utiliza versiones anteriores a 20.11. Los mineros S9 y también x17 con Braiins OS+ tienen soporte. BTC Tools para Windows/Linux puede descargarse `aquí <https://btccom.zendesk.com/hc/en-us/articles/360020105012>`_. En la misma página, documentación sobre como usar BTC Tools está disponible.

  * Con excepción de lo de abajo, Braiins OS+ soporta todas las características de BTC Tools.

  * Limitaciones de BTC Tools al usarlo con Braiins OS+:

    * La configuración en la sección Overclock/Underclock no afecta a Braiins OS+
    * Enhanced LPM enciende autotuning y fija el power limit del minero a 2/3 del power limit predeterminado para ese minero.
    * Deshabilitar Enhanced LPM mantiene el último estado del autotuning y fija el power limit al valor predeterminado del minero (según hardware)
    * Nota: tanto LPM como Enhanced LPM son opciones que solo se usan si se marca la casilla de "Power Control". De lo contrario, se mantiene la configuración específica según la máquina.
    * Actualmente no se puede desactivar Autotuning con BTC Tools y power limit no puede establecerse a un valor específico.
    * Hardware attribute no está rellenado.
    * En caso de tener varios grupos configurados en el minero, solo los pools asociados con el primero se mostrarán en la herramienta.

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

  También puede `enviar petición VIP <https://help.slushpool.com/en/support/tickets/new>`_ a nuestro equipo de soporte.


*******************
Registro de Cambios
*******************

21.12
---------------------------

Este es un lanzamiento mayor que provee soporte para la Antminer S19J Pro (beta)

* Todo el Hardware de minería

  * [característica] el perfil de autoajuste ahora se añade a los archivos de Obtener Ayuda, para un mejor soporte
  * [característica] añadido botón de cambio para modo inmersión a la interfaz web
  * [característica] los registros ahora son menos verbosos, los fastidiosos mensajes de temperatura han sido removidos
  * [característica] los registros ya no contienen códigos de color que confunden la cónsola web
  * [característica] registrar la razón del detenimiento del minero
  * [fallo] arreglado problema con la tasa de hash por hashboard mostrando el total de la tasa de hash en las gráficas
  * [fallo] la elevación de voltaje ha sido reelaborada y ahora es mas rápida
  * [fallo] Bosminer con el autoajuste apagado ahora inicia correctamente con la configuración definida
  * [fallo] Removida la información logrotate de syslog

* Antminer X17, X19

  * [característica] soporte para la Antminer S19J Pro (beta)
  * [característica] mejorado la predicción de consumo de potencia para la Antminer S19J Pro
  * [característica] temperatura de chip para los modelos X19 está siendo estimada basado en la temperatura PCB
  * [característica] eliminada la invalidación de ventilador del autoajuste, por defecto es 100%
  * [fallo] arreglado un problema de chips inalcanzables en modelos X19

* Problemas conocidos:

  * Las tarjetas controladoras del mercado postventa a veces se congelan completamente

21.09.3
---------------------------

Este es un lanzamiento que arregla fallos menores para la familia Antminer X19

* Familia Antminer

  * [fallo] invalidación de máquina en bosminer.toml ya no ocasiona que el web frontend bloquee la configuración de pool
  * [característica] el contenido del EEPROM se escribe en el registro del sistema cuando falla la autodetección por razones de solución de problemas

* Antminer X19

  * [fallo] arreglado problema de autodetección que estaba confundiendo algunas S19Pro por máquinas S19
  * [característica] mas mejoras en la autodetección de máquinas S19

21.09.2
---------------------------

This is a miner bug fix release for Antminer X17/X19 family

* Antminer X17, X19

  * [fallo] activar la configuración del ajuste para la máquina S17Pro
  * [fallo] arreglado bloqueos del controlador de potencia

21.09.1
---------------------------

Este es un lanzamiento menor que extiende el suministro de potencia límite para instalaciones de inmersión.

* Antminer X19

  * [característica] Extendido el límite de potencia hasta 6500 W en la APW12. ¡Esto es para PSU's modificados que puedan manejar este límite de potencia!

21.09
---------------------------

Este es un lanzamiento mayor que presenta una revisión total de la interfaz web y un método de instalación por tarjeta SD mejorado.

* Todo el Hardware de minería

  * [característica] sistema corriendo desde tarjeta SD ahora soporta actualizar y auto-actualizar como el caso de un sistema corriendo desde la memoria interna (NAND)
  * [característica] BOSminer ahora pausa automáticamente la minería, si no hay ningún pool vivo, reduciendo consumo de potencia al mínimo
  * [característica] nueva interfaz web, con modo oscuro y soporte a traducciones (disponible previamente en versión nightly)

* Antminer X17

  * [característica] mejorada invalidación de modelo, para cubrir la situación donde las 3 hashboards tienen EEPROM válido pero el contenido pertenece a hashboards para un modelo diferente. Escenario típico: Tiene una máquina S17 de segunda mano y el dueño anterior reescribió las EEPROM con perfiles T17e.

21.06.1
---------------------------

Este es un lanzamiento con arreglo a fallo menor que mejora el soporte Antminer T17e.

* Antminer X17

  * [fallo] usa el voltaje de chip apropiado para inicialización de T17e

21.06
---------------------------

Este es un lanzamiento mayor que provee soporte mejorado para la familia Antminer X17(incluyendo e).

* Antminer X17

  * [característica] ajuste mejorado asegura rendimiento óptimo del minero en los niveles de potencia configurados por el usuario
  * [característica] soporte para S17e y T17e
  * [característica] mejorado soporte para T17, T17+, S17, S17+
  * [característica] soporte Braiins OS+ Manager activado para toda la familia x17
  * [característica] mejorado EPD, el Escalado de Potencia Dinámico ahora también sube la escala del límite de potencia, cuando la temperatura del minero esté al menos 5 grados debajo de la temperatura HOT (caliente) y los ventiladores estén por debajo de 80%.
  * [característica] BOSminer va correr e ignorar configuraciones incorrectas solo cuando sea usado Braiins OS+ Manager de manera que la configuración pueda ser arreglada. Si Braiins OS+ Manager no es usado, BOSminer se apagará cuando exista una configuración incorrecta.

21.04
---------------------------

Este es un lanzamiento mayor para Antminer S9 que añade soporte a Braiins OS+ Manager - una solución en la nube para la administración y vigilancia de mineros.

* Todos los tipos de hardware de minado

  * [característica] soporte a Braiins OS+ Manager - una solución en la nube para la administración y vigilancia de mineros, creada en colaboración con FarmGod
  * [característica] BOSminer ha reducido el tráfico adicional de red al mínimo absoluto al sondear los servidores stratum vivos
  * [fallo] el autoajuste ahora está activado automáticamente al usar el método de arranque SD
  * [fallo] BOSminer correrá incluso cuando la configuración sea incorrecta para evitar la pérdida de conexión por estár BOSminer detenido
  * [fallo] se ha solucionado un problema de reconexión prolongada a los pools cuando se cambia la IP pública
  

21.02
---------------------------

* Todos los tipos de hardware de minado

  * [característica] la interfaz web ahora tiene una Herramienta de Soporte que puede generar el archivo con los registros que pueden ser enviados a nosotros
  * [característica] el nuevo tablero GUI permite una mejor vista general de la salud del minero y su rendimiento en una sola página condensada
  * [característica] mejoras en la caja de herramientas incluye listar los mineros desde el script discover y command para un solo IP
  * [característica] la imagen para tarjeta sd tiene una característica de auto-instalación a NAND que elimina completamente la necesidad de usar una máquina de escritorio para iniciar la instalación desde SD


* Antminer X17

  * [característica] minar en la familia X17 se puede pausar/seguir rápidamente lo cual es útil para granjas que participan en programas de la red eléctrica. Ej: el comando "pause" sería así: `echo '{"command":"pause"}' | nc IP_ADDRESS 4028`

20.12
---------------------------

* Todos los tipos de hardware de minado

  * [fallo] las versiones nocturnas a partir de ahora apuntarán a la fuente nocturna como se esperaba
  * [fallo] El servidor DHCP en la interfaz de red ha sido deshabilitado. Pedimos disculpas por esta equivocación


* Antminer X17

  * [característica] para mineros con la máquina bloqueada, ahora proveemos un mecanismo para configurar la imagen SD para que instale BOS+ en la NAND de forma completamente automática
  * [característica] todos los modelos X17 con la memoria flash NAND Macronix ahora están soportados
  * [característica] nueva sección de configuración [model_detection] se añadió para permitir ignorar el resultado de la auto-detección de hardware y honrar el tipo preseleccionado en la configuración. Esto es para cubrir la situación donde todas las 3 tarjetas tienen EEPROM corrompido. Vea la opción de configuración use_config_fallback
  * [característica] nueva FPGA permite overclocking hasta 950 MHz (NOTA: ¡estas frecuencias son realistas solo para instalaciones con super enfriamiento por inmersión!)
  * [característica] la configuración de voltaje se ha hecho mas robusta para soportar máquinas que han tenido problemas de fijación de voltaje dentro de un tiempo límite especificado

20.11
---------------------------

Este es una publicación importante que mejora el rendimiento del ajuste a la familia X17 y su operatividad general.

* Todos los tipos de hardware

  * [característica] - ahora hay una sola descarga para la Caja de herramientas BOS que puede ser utilizada en todos los tipos de hardware. También permite mezclar modelos S9 y X17 en un archivo lista.csv, para que los usuarios puedan hacer todo por lotes (instalar, configurar, desinstalar, etc.) incluso con múltiples tipos de hardware.

* Antminer X17

  * [característica] limitada la frecuencia para toda la familia a 750 MHz
  * [característica] mejorado el ajuste para toda la familia
  * [característica] implementada soluciones alternas para tarjetas hash con fallas
  * [característica] soporte para la T17, T17+
  * [fallo] mejorado el rendimiento para el hardware S17+
  * [fallo] el bloqueo del API cuando el ajuste está corriendo ha sido resuelto, las gráficas en la interfaz web no se siguen pegando un par de segundos durante el reinicio del ajuste

* Antminer S9

  * [característica] - la Caja de Herramientas BOS es la misma para Braiins OS y Braiins OS+ ahora con Braiins OS+ por defecto. Los usuarios que deseen instalar la versión de código abierto pueden hacerlo con el parámetro --open-source

20.10
---------------------------

Esta es una versión importante que agrega soporte beta para Antminer S17 +

* Todos los tipos de hardware de minería

  * [feature] procd ahora espera hasta 20 segundos para permitir el apagado adecuado de BOSminer
  * [función] El monitor BOSminer ahora solo hace girar los ventiladores cuando BOSminer se ha detenido para enfriar la máquina
  * [error] el cliente Stratum ya no se queja de 'Stratum: solución aceptada inesperada # 0'
  * [error] error de estado incorrecto del cliente de stratum se ha corregido (es decir, ya no debería ver "ERRO BUG: 'finish_shutdown_or_recover': estado inesperado 'Iniciando'")
  * [función] La compatibilidad con el programa de referencia se ha hecho más sólida para admitir varios tipos de hardware en una única configuración de referencia
  * [característica] El protocolo de administración BOS ahora se transmite entre las conexiones devfee stratum V2 en caso de conmutación por error a una conexión de respaldo

* Antminer S9

  * no hubo cambios específicos de hardware

* Antminer S17

  * [función] Se ha añadido compatibilidad para S17 +
  * [característica] Los límites de temperatura predeterminados se han reducido aún más a la temperatura objetivo: 72 C, temperatura caliente: 85 C, temperatura peligrosa: 92 C ya que la familia S17 es muy sensible al sobrecalentamiento debido a la calidad del material de soldadura utilizado en el tablero PCB
  * [característica] Hemos agregado la detección automática de la variante de la placa de control (C49 vs C52) para manejar los ventiladores correctamente
  * [característica] Braiins OS se negaría a instalar en máquinas X17 que tengan el flash NAND 'Macronix'. Actualmente, solo se admite el flash NAND 'Micron'
  * [función] Se ha implementado la detección automática de S17, S17Pro, S17 + y hay una sola imagen para todos estos tipos de máquinas
  * [función] Los límites de potencia ahora se calculan dinámicamente en función de la máquina detectada

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
  
  * [característica] implementado programa de referidos - los vendedores de Braiins OS+ ahora pueden adquirir un programa paquete de referidos (con ID de referido y archivo de configuración) que al aplicarse enviará una porción de la comisión de desarrollo recogida a sus referentes.

* Problemas Conocidos
  * [problema] el consumo de potencia mostrado para la S17 y S17 Pro es menor al consumo actual de potencia, esto será mejorado en las próximas actualizaciones.
  * [problema] BOSminer es lento para re-conectarse al pool cuando el proveedor de internet cambia la dirección IP del usuario

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
    * [característica] El sub-sistema de autoajuste ahora almacena perfiles de rendimiento en /etc/bosminer-autotune.json. Los perfiles de rendimiento se graban para cada nivel de potencia e índice de tarjeta
    * [característica] Escalado de Potencia Dinámico ahora baja el limite de potencia (powerlimit) de un minero una cantidad definida por el usuario si el dispositivo alcanza la *Hot Temperature* (temperatura caliente). Al alcanzar el límite mínimo de potencia, el minero se apaga para enfriarse. El minero vuelve a trabajar al limite de potencia original luego de un período de tiempo definido por el usuario

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

    * [característica] Se muestra el estado del Ajuste en la GUI. Añadido comando API TUNERSTATUS API.
    * [fallo] algunos dispositivos estaban experimentando bloqueos aleatorios en el bus del controlador I2C y fallaban en comunicarse con los controladores de potencia en la tarjeta de hash conectada al bus I2C compartido. Encontramos que la causa era el núcleo del controlador Xilinx I2C que hemos integrado al flujo de bits en la FPGA. Hemos cambiado al I2C presente en el SoC y el flujo de bits solo enruta la señal del periférico (IIC9) a los pins FPGA correspondientes.

20.03
---------------------------

  * Todos los tipos de hardware de minado

    * [característica] el archivo de configuración permite especificar un límite de potencia para la PSU en el algoritmo de autoajuste que tomará en cuenta para maximizar los TH/W producidos por el dispositivo de minado.

  * Antminer S9

    * [característica] Autoajuste basado en limite de potencia especificado por el usuario

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
 * Instalación en masa
 * Actualizaciones automáticas con el sistema de paquetes estándar opkg
 * Control de ventilador completamente personalizable (permite el enfriamiento por inmersión)
 * Monitoreo avanzado para prevenir el sobrecalentamiento y otros problemas
 * Mecanismo de Auto-actualización
 * Escalado de Potencia Dinámico, lo cual baja el límite de potencia en caso de altas temperaturas, para minado continuo
