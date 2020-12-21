
.. _configuration:

#############
Configuración
#############

.. contents::
  :local:
  :depth: 2

**********************************************************
Configurar Braiins OS+ usando la Caja de Herramientas BOS+
**********************************************************

Puede configurar fácilmente Braiins OS+ en múltiples dispositivos usando la **Caja de herramientas BOS+**. Para hacerlo, siga los pasos en la sección :ref:`bosbox_configure`.

*****************************************************
Configurar Braiins OS+ usando el Paquete Remoto (SSH)
*****************************************************

El script de instalación usa dos tipos de argumentos:

   * argumentos posicionales - requerido para completar la instalación;
   * argumentos opcionales - opcional (ej: no requerido) para completar la instalación.

La sintaxis del script de instalación es la siguiente:

  ::

    uso: upgrade2bos.py [-h] [--no-backup] [--no-nand-backup] [--no-keep-network] [--keep-hostname] [--no-wait] hostname

**Argumentos posicionales:**

.. code-block:: none

    hostname [hostname ...] Nombre host o dirección IP del minero seleccionado

**Argumentos opcionales:**

.. code-block:: none

  -h, --help            muestra este mensaje de ayuda y sale
  --no-backup           salta hacer respaldo al minero antes de actualizar
  --no-nand-backup      salta respaldo completo NAND (la configuración sigue siendo respaldada)
  --no-keep-network     no conservar la configuración de red del minero
                        (usa DHCP)
  --keep-hostname       mantener nombre host del minero
  --no-wait             no esperar hasta que el sistema esté completamente
                        actualizado


****************
Ajustes del Pool
****************

Los usuarios pueden especificar múltiples pool. Todos los pool en un grupo usan la estrategia multi-pool con respaldo, lo que significa que BOSminer cambiará automáticamente al segundo pool si el primer pool muere.

La configuración está disponible a través de la web GUI (*Miner -> Configuration*) o en el archivo de configuración``/etc/bosminer.toml``.

La sintaxis es la siguiente:

  ::

     [[group]]
     name = 'Default'
     quota = 1

     [[group.pool]]
     enabled = true
     url = 'stratum2+tcp://v2.stratum.slushpool.com/u95GEReVMjK6k5YqiSFNqqTnKU4ypU2Wm8awa6tmbmDmk1bWt'
     user = 'username.workername'
     password = 'secret'

  * *name* - Nombre del grupo de pool (explicado en la sección *Grupos de Pool* abajo)
  * *quota* - Cuota fijada por el usuario para el grupo (explicado en la sección *Grupos de Pool* abajo)
  * *enabled* - Estado inicial del pool luego de la inicializar BOSminer (por defecto=true (verdadero))
  * *url* - Argumento obligatorio para el URL del servidor especificado en el formato ``scheme://HOSTNAME:PORT/POOL_PUBLIC_KEY``. No se necesita especificar un puerto explicito para *Stratum V2* en Slush Pool. La razón es que el protocolo está todavía en desarrollo y nosotros alternamos entre dos puertos por defecto (**3336** y **3337**) a través de actualizaciones al protocolo. Los mineros que no actualicen podrán seguir usando la versión previa del protocolo. Los mineros que actualicen no tendrán que preocuparse en actualizar su URL de minado con un puerto nuevo. Hay un *nuevo* elemento requerido en la ruta del URL que es la llave pública anunciada por el pool que el software de minado usa para verificar la autenticidad del punto final de minado al cual se conecta. Esto previene ataques de intermediarios que intenten robar tasa de hash. Cualquier intento de ello resultará en verificación fallida y el software rechazará usar el pool dado.
  * *user* - Argumento obligatorio para el nombre de usuario especificado en el formato ``USERNAME.WORKERNAME``
  * *password* - Ajustes opcionales en password

Grupos de Pool
==============

  Los usuarios pueden crear distintos grupos de pool múltiples. Todos los pool dentro de un grupo usarán la estrategia de respaldo multi-pool descrita arriba. Cuando se crean grupos de pool múltiples, el trabajo es distribuido para cada grupo con la estrategia de balanceo de carga, bien sea a base de Cuotas o por una Tasa Fija Compartida.

  Ejemplo:

  Grupo 1 tiene dos pool especificados y una Cuota asignada de "1". Grupo 2 tiene solo un pool especificado y una Cuota asignada de "2".

  - El trabajo es asignado a los grupos con una tasa 1:2
  - Grupo 2 recibirá el doble la cantidad de trabajo asignado que grupo 1.
  - Si el primer pool en Grupo 1 muere, BOSminer cambiará al segundo pool en Grupo 1.

  Es posible usar una Tasa Fija Compartida en lugar de una Cuota, lo que dividiría el trabajo en un porcentaje especificado. Una Cuota de 1:1 es equivalente a una Tasa Fija Compartida de 0.5 (50%) - ambos ajustes dividirán el trabajo por la mitad y lo enviarán a los dos grupos.

  La configuración está disponible a través de la web GUI (*Miner -> Configuration*) o en el archivo de configuración ``/etc/bosminer.toml``.

  Ejemplo de dos grupos y pools multiples:

  ::

     [[group]]
     name = 'MiGrupo1'
     quota = 1

     [[group.pool]]
     enabled = true
     url = 'stratum2+tcp://v2.stratum.slushpool.com/u95GEReVMjK6k5YqiSFNqqTnKU4ypU2Wm8awa6tmbmDmk1bWt'
     user = 'usuarioA.minero'

     [[group.pool]]
     enabled = true
     url = 'stratum+tcp://stratum.slushpool.com:3333'
     user = 'usuarioA.minero'

     [[group]]
     name = 'MiGrupo2'
     quota = 2

     [[group.pool]]
     url = 'stratum+tcp://stratum.slushpool.com:3333'
     user = 'usuarioB.minero'

Con esta disposición, el trabajo será dividido entre los dos grupos, a una tasa 1:2. Por defecto, el minero estará minando en el primer pool del grupo "MiGrupo1" y en un pool definido en el grupo "MiGrupo2". Si el primer pool en "MiGrupo1" muere, el minero estará minando en el segundo pool del grupo "MiGrupo1". Ya que un segundo pool url no está especificado para "MiGrupo2", nada se hará si el pool en "MiGrupo2" falla.

*******************
Ajustes Cadena Hash
*******************

Configuración opcional para anular los ajustes predeterminados de todas las cadenas hash. Esto permite a los usuarios controlar la frecuencia y voltaje de cada cadena hash y les permite activar o desactivar AsicBoost. Cuando el autoajuste está activado, estos ajustes son ignorados. Los ajustes de cadena hash globales pueden también ser anulados en ajustes por-cadena.

La configuración esta disponible también a través de la web GUI (*Miner -> Configuration*) o en el archivo de configuración ``/etc/bosminer.toml``.

La sintaxis es la siguiente:

  ::

     [hash_chain_global]
     asic_boost = true
     frequency = 650.0
     voltage = 8.8

  * *asic_boost* - Activa o desactiva soporte AsicBoost (por defecto=true (verdad))
  * *frequency* - Fija la frecuencia por defecto del chip en MHz para todas las cadenas hash (por defecto=650.0)
  * **(Solo Antminer S9)** *voltage* - Fija el voltaje por defecto en V para todas las cadenas hash (por defecto=8.8)

La sintaxis de ajuste por-cadena es la siguiente:

  ::

     [hash_chain.6]
     frequency = 650.0
     voltage = 8.8

  * *[hash_chain.6]* - Anula los ajustes globales para la cadena hash '6'
  * *frequency* - Anula la frecuencia de chip global en MHz para la cadena hash '6' (por defecto='hash_chain_global.frequency')
  * *voltage* - Anula el voltaje global en V para la cadena hash '6' (por defecto='hash_chain_global.voltage')

***********************************
Control de Temperatura y ventilador
***********************************

Modo Control de Temperatura
===========================

  Braiins OS+ soporta control automático de temperatura (utilizando el `controlador PID <https://es.wikipedia.org/wiki/Controlador_PID>`__).
  El controlador puede operar en uno de tres modos:

  -  **Automatic** - El software del minero intenta regular la velocidad del ventilador para que la temperatura sea aproximadamente la target temperature (que puede ser configurada). El rango de temperatura permitido es 0-200 grados Celsius.
  -  **Manual** - Los ventiladores se mantienen a una velocidad fija, definida por el usuario, sin importar la temperatura. Esto es útil si se tiene una forma propia de enfriar el minero o si los sensores de temperatura no funcionan. La velocidad permitida es entre 0%-100%. La unidad de control monitorea solo temperaturas hot (caliente) y dangerous (peligrosa).
  -  **Disabled** - **ADVERTENCIA**: ¡esto podría dañar el dispositivo porque no se hace ningún control!

  El modo control de temperatura puede cambiarse en la página *Miner -> Configuration* o en el archivo de configuración  ``/etc/bosminer.toml``.

  **Advertencia**: mal ajustar los ventiladores (bien sea por apagarlos o por usar un nivel muy lento, o colocar una target temperature muy alta) podría **DAÑAR** de forma irreversible su minero.

Limites de temperatura por defecto
==================================

  Los limites de temperatura por defecto están ajustados para prevenir que el minero se sobre-caliente y se dañe.

  * **Target temperature** es una temperatura que el minero intentará mantener (*por defecto es* **89°C**).
  * **Hot temperature** es un límite en la cual los ventiladores comenzarán a girar al 100% (*por defecto es* **100°C**).
  * **Dangerous temperature** es un límite en el cual BOSminer se apagará para prevenir sobre-calentar y dañar el minero (*por defecto es* **110°C**).

  Los límites por defecto de temperatura pueden ajustarse en la página *Miner -> Configuration* o en el archivo de configuración``/etc/bosminer.toml``.

Configuración de Control de Temperatura y ventilador en ``bosminer.toml``
=========================================================================

  Los valores por defecto pueden anularse al editar las líneas correspondientes en el archivo de configuración, ubicado en ``/etc/bosminer.toml``.

  La sintaxis es la siguiente:

  ::

     [temp_control]
     mode = 'auto'
     target_temp = 89
     hot_temp = 100
     dangerous_temp = 110

  * *mode* - Ajusta el modo de control (por defecto='auto')
  * *target_temp* - Ajusta la temperatura en Celsius (por defecto=89.0). ¡Esta opción SOLO se usa cuando 'temp_control.mode' está en 'auto'!
  * *hot_temp* - Ajusta la temperatura caliente en Celsius (por defecto=100.0). Cuando se alcanza esta temperatura, la velocidad del ventilador se pone a 100%.
  * *dangerous_temp* - Ajusta la temperatura peligrosa en Celsius (por defecto=110.0). Cuando se alcanza esta temperatura, ¡el minado se apaga! **ADVERTENCIA:** ¡fijar muy alto este valor puede dañar el dispositivo!


  ::

     [fan_control]
     speed = 100
     min_fans = 1

  * *speed* - Ajusta una velocidad de ventilador fija en % (por defecto=70). ¡Esta opción NO se usa cuando *temp_control.mode* está 'auto'!
  * *min_fans* - Ajusta el número mínimo de ventiladores requeridos para que corra BOSminer (por defecto=1).
  * Para **deshabilitar el control del ventilador** completamente, coloque 'speed' y 'min_fans' en 0.

Operación de ventilador
=======================

  1. Al iniciarse los sensores de temperatura, se activa el control de ventilador. Si los sensores de temperatura no están funcionando o leen una temperatura 0, los ventiladores se ponen automáticamente a máxima velocidad.
  2. Si el modo actual es "velocidad fija de ventilador", el ventilador se pone a la velocidad dada.
  3. Si el modo actual es "control de ventilador automático", la velocidad de ventilador es regulada por la temperatura.
  4. En caso de que la temperatura del minero esté por encima de *HOT temperature*, los ventiladores se ponen a 100% (incluso en el modo de velocidad fija de ventilador).
  5. En caso de que la temperatura del minero esté por encima de *DANGEROUS temperature*, BOSminer se apagará (incluso en el modo de velocidad fija de ventilador).

********************
Ajustes de Afinación
********************

La afinación se puede configurar tanto vía web GUI, usando la Caja de Herramientas BOS+ o en el archivo de configuración ``/etc/bosminer.toml``.

Para hacer un cambio a la configuración vía web GUI, entre al menú *Miner -> Configuration* y edite la sección *Autotuning*.

Para hacer un cambio de configuración en múltiples dispositivos usando la **Caja de Herramientas BOS+**, siga los pasos en la sección :ref:`bosbox_configure`.

Para hacer un cambio a la configuración en el archivo de configuración, conéctese al minero vía SSH y edite el archivo ``/etc/bosminer.toml``. La sintaxis es la siguiente:

  ::

     [autotuning]
     enabled = true
     psu_power_limit = 1200

La línea *enabled* puede contener los valores *true* (verdad) para activar el autoajuste, o *false* (falso) para desactivar el autoajuste. El *psu_power_limit* puede contener valores numéricos (min. 100 y max. 5000), representando el límite de energía (en Vatios) de la PSU (fuente de poder) para tres tarjetas hash y la tarjeta controladora.

Alternativamente, es posible encender el autoajuste automáticamente luego de que termine la instalación especificando el argumento ``--power-limit POWER_LIMIT`` en el comando de instalación.

********************************
Escalamiento de Energía Dinámico
********************************

El Escalamiento de Energía Dinámico baja el limite de energía (powerlimit) de un minero una cantidad definida por el usuario si el dispositivo alcanza la *Hot Temperature* (temperatura caliente). Al alcanzar el límite de energía mínimo, el minero se apaga para enfriarse. El minero vuelve a trabajar al limite de energía original luego de un período de tiempo definido por el usuario.

El Escalamiento de Energía Dinámico puede configurarse vía web GUI, usando la Caja de Herramientas BOS+ o en el archivo de configuración ``/etc/bosminer.toml``.

Para hacer un cambio a la configuración vía web GUI, entre en el menú *Miner -> Configuration* y edite la sección *Dynamic Power Scaling*.

Para hacer un cambio a la configuración en múltiples dispositivos usando la **Caja de Herramientas BOS+**, siga los pasos en la sección :ref:`bosbox_configure`.

Para hacer un cambio en el archivo de configuración, conéctese el aminero vía SSH y edite el archivo ``/etc/bosminer.toml``. La sintaxis es la siguiente:

  ::

     [power_scaling]
     enabled = false
     power_step = 100
     min_psu_power_limit = 800
     shutdown_enabled = true
     shutdown_duration = 3.0

La línea *enabled* puede tener los valores *true* para activar el Escalamiento de Energía Dinámico, o *false* para desactivar el Escalamiento de Energía Dinámico.
El *power_step* puede tener valores numéricos (min. 100 y max. 1000), representando el escalón para bajar el límite de energía (powerlimit) en vatios (Watts), que ocurre cada vez que el minero alcance la *HOT* temperature (temperatura caliente).
El *min_psu_power_limit* puede tener valores numéricos (min. 100 y max. 5000), representando el límite mínimo de la fuente para el Escalamiento de Energía Dinámico. Si *psu_power_limit* está en el nivel *min_psu_power_limit* y el minero sigue *HOT* (caliente) y *shutdown_enabled* es true (verdadero), entonces el minero se apaga por un período de tiempo, definido en el valor *shutdown_duration* (duración de apagado) (en horas). Luego de eso, el minero es iniciado pero con el valor inicial de *psu_power_limit* (*PSU power limit* en la sección *Autotuning*) (autoajuste).

***************
Auto-actualizar
***************

Mientras auto-actualizar esté encendido, La máquina revisará periódicamente si hay una nueva versión de Braiins OS+ y actualizará a ella automáticamente cuando la encuentre. Esta característica se enciende por defecto al cambiar desde el firmware de fábrica, pero debe ser encendida manualmente al actualizar desde versiones anteriores de Braiins OS o Braiins OS+.

Auto-actualizar puede configurarse tanto vía web GUI o usando la Caja de Herramientas BOS+.

Para hacer un cambio a la configuración vía web GUI, entre en el menú *System -> Upgrade* y edite la sección *System Upgrade*.

Para hacer un cambio a la configuración en múltiples dispositivos usando la **Caja de Herramientas BOS+**, siga los pasos en la sección :ref:`bosbox_configure`.

Alternativamente, es posible **apagar** auto-actualizar durante la instalación especificando el argumento ``--no-auto-upgrade`` en el comando de instalación.

************
Password SSH
************

Puede poner el password del minero via SSH desde un host remoto al correr el comando de abajo y reemplazar *[passwordnuevo]* con su propio password.

  * Nota: Braiins OS+ **no*** mantiene el historial de los comandos ejecutados.

  .. code:: bash

     ssh root@[minero-hostname-o-ip] 'echo -e "[passwordnuevo]\n[passwordnuevo]" | passwd'

Para hacer eso en muchos hosts en paralelo podría usar `p-ssh <https://linux.die.net/man/1/pssh>`__.

******************
Dirección MAC e IP
******************

Por defecto, la dirección MAC del dispositivo se mantiene igual y es heredada del firmware (de serie o Braiins OS) almacenada en el dispositivo (NAND). De esta forma, una vez que el dispositivo inicie con Braiins OS+, tendrá la misma dirección IP que tenía con el firmware de fábrica.

Alternativamente, puede especificar una dirección MAC de su selección al modificar el parametro ``ethaddr=`` en el archivo ``uEnv.txt`` (ubicado en la primera partición FAT de la tarjeta SD).


**Argumentos opcionales:**

.. code-block:: none

  -h, --help            muestra este mensaje de ayuda y sale
  --no-backup           salta hacer respaldo al minero antes de actualizar
  --no-nand-backup      salta respaldo completo NAND (la configuración sigue siendo respaldada)
  --no-keep-network     no conservar la configuración de red del minero
                        (usa DHCP)
  --keep-hostname       mantener nombre host del minero
  --no-wait             no esperar hasta que el sistema esté completamente
                        actualizado


****************
Ajustes del Pool
****************

Los usuarios pueden especificar múltiples pool. Todos los pool en un grupo usan la estrategia multi-pool con respaldo, lo que significa que BOSminer cambiará automáticamente al segundo pool si el primer pool muere.

La configuración está disponible a través de la web GUI (*Miner -> Configuration*) o en el archivo de configuración``/etc/bosminer.toml``.

La sintaxis es la siguiente:

  ::

     [[group]]
     name = 'Default'
     quota = 1

     [[group.pool]]
     enabled = true
     url = 'stratum2+tcp://v2.stratum.slushpool.com/u95GEReVMjK6k5YqiSFNqqTnKU4ypU2Wm8awa6tmbmDmk1bWt'
     user = 'username.workername'
     password = 'secret'

  * *name* - Nombre del grupo de pool (explicado en la sección *Grupos de Pool* abajo)
  * *quota* - Cuota fijada por el usuario para el grupo (explicado en la sección *Grupos de Pool* abajo)
  * *enabled* - Estado inicial del pool luego de la inicializar BOSminer (por defecto=true (verdadero))
  * *url* - Argumento obligatorio para el URL del servidor especificado en el formato ``scheme://HOSTNAME:PORT/POOL_PUBLIC_KEY``. No se necesita especificar un puerto explicito para *Stratum V2* en Slush Pool. La razón es que el protocolo está todavía en desarrollo y nosotros alternamos entre dos puertos por defecto (**3336** y **3337**) a través de actualizaciones al protocolo. Los mineros que no actualicen podrán seguir usando la versión previa del protocolo. Los mineros que actualicen no tendrán que preocuparse en actualizar su URL de minado con un puerto nuevo. Hay un *nuevo* elemento requerido en la ruta del URL que es la llave pública anunciada por el pool que el software de minado usa para verificar la autenticidad del punto final de minado al cual se conecta. Esto previene ataques de intermediarios que intenten robar tasa de hash. Cualquier intento de ello resultará en verificación fallida y el software rechazará usar el pool dado.
  * *user* - Argumento obligatorio para el nombre de usuario especificado en el formato ``USERNAME.WORKERNAME``
  * *password* - Ajustes opcionales en password

Grupos de Pool
==============

  Los usuarios pueden crear distintos grupos de pool múltiples. Todos los pool dentro de un grupo usarán la estrategia de respaldo multi-pool descrita arriba. Cuando se crean grupos de pool múltiples, el trabajo es distribuido para cada grupo con la estrategia de balanceo de carga, bien sea a base de Cuotas o por una Tasa Fija Compartida.

  Ejemplo:

  Grupo 1 tiene dos pool especificados y una Cuota asignada de "1". Grupo 2 tiene solo un pool especificado y una Cuota asignada de "2".

  - El trabajo es asignado a los grupos con una tasa 1:2
  - Grupo 2 recibirá el doble la cantidad de trabajo asignado que grupo 1.
  - Si el primer pool en Grupo 1 muere, BOSminer cambiará al segundo pool en Grupo 1.

  Es posible usar una Tasa Fija Compartida en lugar de una Cuota, lo que dividiría el trabajo en un porcentaje especificado. Una Cuota de 1:1 es equivalente a una Tasa Fija Compartida de 0.5 (50%) - ambos ajustes dividirán el trabajo por la mitad y lo enviarán a los dos grupos.

  La configuración está disponible a través de la web GUI (*Miner -> Configuration*) o en el archivo de configuración ``/etc/bosminer.toml``.

  Ejemplo de dos grupos y pools multiples:

  ::

     [[group]]
     name = 'MiGrupo1'
     quota = 1

     [[group.pool]]
     enabled = true
     url = 'stratum2+tcp://v2.stratum.slushpool.com/u95GEReVMjK6k5YqiSFNqqTnKU4ypU2Wm8awa6tmbmDmk1bWt'
     user = 'usuarioA.minero'

     [[group.pool]]
     enabled = true
     url = 'stratum+tcp://stratum.slushpool.com:3333'
     user = 'usuarioA.minero'

     [[group]]
     name = 'MiGrupo2'
     quota = 2

     [[group.pool]]
     url = 'stratum+tcp://stratum.slushpool.com:3333'
     user = 'usuarioB.minero'

Con esta disposición, el trabajo será dividido entre los dos grupos, a una tasa 1:2. Por defecto, el minero estará minando en el primer pool del grupo "MiGrupo1" y en un pool definido en el grupo "MiGrupo2". Si el primer pool en "MiGrupo1" muere, el minero estará minando en el segundo pool del grupo "MiGrupo1". Ya que un segundo pool url no está especificado para "MiGrupo2", nada se hará si el pool en "MiGrupo2" falla.

*******************
Ajustes Cadena Hash
*******************

Configuración opcional para anular los ajustes predeterminados de todas las cadenas hash. Esto permite a los usuarios controlar la frecuencia y voltaje de cada cadena hash y les permite activar o desactivar AsicBoost. Cuando el autoajuste está activado, estos ajustes son ignorados. Los ajustes de cadena hash globales pueden también ser anulados en ajustes por-cadena.

La configuración esta disponible también a través de la web GUI (*Miner -> Configuration*) o en el archivo de configuración ``/etc/bosminer.toml``.

La sintaxis es la siguiente:

  ::

     [hash_chain_global]
     asic_boost = true
     frequency = 650.0
     voltage = 8.8

  * *asic_boost* - Activa o desactiva soporte AsicBoost (por defecto=true (verdad))
  * *frequency* - Fija la frecuencia por defecto del chip en MHz para todas las cadenas hash (por defecto=650.0)
  * *voltage* - Fija el voltaje por defecto en V para todas las cadenas hash (por defecto=8.8)

La sintaxis de ajuste por-cadena es la siguiente:

  ::

     [hash_chain.6]
     frequency = 650.0
     voltage = 8.8

  * *[hash_chain.6]* - Anula los ajustes globales para la cadena hash '6'
  * *frequency* - Anula la frecuencia de chip global en MHz para la cadena hash '6' (por defecto='hash_chain_global.frequency')
  * *voltage* - Anula el voltaje global en V para la cadena hash '6' (por defecto='hash_chain_global.voltage')

***********************************
Control de Temperatura y ventilador
***********************************

Modo Control de Temperatura
===========================

  Braiins OS+ soporta control automático de temperatura (utilizando el `controlador PID <https://es.wikipedia.org/wiki/Controlador_PID>`__).
  El controlador puede operar en uno de tres modos:

  -  **Automatic** - El software del minero intenta regular la velocidad del ventilador para que la temperatura sea aproximadamente la target temperature (que puede ser configurada). El rango de temperatura permitido es 0-200 grados Celsius.
  -  **Manual** - Los ventiladores se mantienen a una velocidad fija, definida por el usuario, sin importar la temperatura. Esto es útil si se tiene una forma propia de enfriar el minero o si los sensores de temperatura no funcionan. La velocidad permitida es entre 0%-100%. La unidad de control monitorea solo temperaturas hot (caliente) y dangerous (peligrosa).
  -  **Disabled** - **ADVERTENCIA**: ¡esto podría dañar el dispositivo porque no se hace ningún control!

  El modo control de temperatura puede cambiarse en la página *Miner -> Configuration* o en el archivo de configuración  ``/etc/bosminer.toml``.

  **Advertencia**: mal ajustar los ventiladores (bien sea por apagarlos o por usar un nivel muy lento, o colocar una target temperature muy alta) podría **DAÑAR** de forma irreversible su minero.

Limites de temperatura por defecto
==================================

  Los limites de temperatura por defecto están ajustados para prevenir que el minero se sobre-caliente y se dañe.

  * **Target temperature** es una temperatura que el minero intentará mantener (*por defecto es* **89°C**).
  * **Hot temperature** es un límite en la cual los ventiladores comenzarán a girar al 100% (*por defecto es* **100°C**).
  * **Dangerous temperature** es un límite en el cual BOSminer se apagará para prevenir sobre-calentar y dañar el minero (*por defecto es* **110°C**).

  Los límites por defecto de temperatura pueden ajustarse en la página *Miner -> Configuration* o en el archivo de configuración``/etc/bosminer.toml``.

Configuración de Control de Temperatura y ventilador en ``bosminer.toml``
=========================================================================

  Los valores por defecto pueden anularse al editar las líneas correspondientes en el archivo de configuración, ubicado en ``/etc/bosminer.toml``.

  La sintaxis es la siguiente:

  ::

     [temp_control]
     mode = 'auto'
     target_temp = 89
     hot_temp = 100
     dangerous_temp = 110

  * *mode* - Ajusta el modo de control (por defecto='auto')
  * *target_temp* - Ajusta la temperatura en Celsius (por defecto=89.0). ¡Esta opción SOLO se usa cuando 'temp_control.mode' está en 'auto'!
  * *hot_temp* - Ajusta la temperatura caliente en Celsius (por defecto=100.0). Cuando se alcanza esta temperatura, la velocidad del ventilador se pone a 100%.
  * *dangerous_temp* - Ajusta la temperatura peligrosa en Celsius (por defecto=110.0). Cuando se alcanza esta temperatura, ¡el minado se apaga! **ADVERTENCIA:** ¡fijar muy alto este valor puede dañar el dispositivo!


  ::

     [fan_control]
     speed = 100
     min_fans = 1

  * *speed* - Ajusta una velocidad de ventilador fija en % (por defecto=70). ¡Esta opción NO se usa cuando *temp_control.mode* está 'auto'!
  * *min_fans* - Ajusta el número mínimo de ventiladores requeridos para que corra BOSminer (por defecto=1).
  * Para **deshabilitar el control del ventilador** completamente, coloque 'speed' y 'min_fans' en 0.

Operación de ventilador
=======================

  1. Al iniciarse los sensores de temperatura, se activa el control de ventilador. Si los sensores de temperatura no están funcionando o leen una temperatura 0, los ventiladores se ponen automáticamente a máxima velocidad.
  2. Si el modo actual es "velocidad fija de ventilador", el ventilador se pone a la velocidad dada.
  3. Si el modo actual es "control de ventilador automático", la velocidad de ventilador es regulada por la temperatura.
  4. En caso de que la temperatura del minero esté por encima de *HOT temperature*, los ventiladores se ponen a 100% (incluso en el modo de velocidad fija de ventilador).
  5. En caso de que la temperatura del minero esté por encima de *DANGEROUS temperature*, BOSminer se apagará (incluso en el modo de velocidad fija de ventilador).

********************
Ajustes de Afinación
********************

La afinación se puede configurar tanto vía web GUI, usando la Caja de Herramientas BOS+ o en el archivo de configuración ``/etc/bosminer.toml``.

Para hacer un cambio a la configuración vía web GUI, entre al menú *Miner -> Configuration* y edite la sección *Autotuning*.

Para hacer un cambio de configuración en múltiples dispositivos usando la **Caja de Herramientas BOS+**, siga los pasos en la sección :ref:`bosbox_configure`.

Para hacer un cambio a la configuración en el archivo de configuración, conéctese al minero vía SSH y edite el archivo ``/etc/bosminer.toml``. La sintaxis es la siguiente:

  ::

     [autotuning]
     enabled = true
     psu_power_limit = 1200

La línea *enabled* puede contener los valores *true* (verdad) para activar el autoajuste, o *false* (falso) para desactivar el autoajuste. El *psu_power_limit* puede contener valores numéricos (min. 100 y max. 5000), representando el límite de energía (en Vatios) de la PSU (fuente de poder) para tres tarjetas hash y la tarjeta controladora.

Alternativamente, es posible encender el autoajuste automáticamente luego de que termine la instalación especificando el argumento ``--power-limit POWER_LIMIT`` en el comando de instalación.

********************************
Escalamiento de Energía Dinámico
********************************

El Escalamiento de Energía Dinámico baja el limite de energía (powerlimit) de un minero una cantidad definida por el usuario si el dispositivo alcanza la *Hot Temperature* (temperatura caliente). Al alcanzar el límite de energía mínimo, el minero se apaga para enfriarse. El minero vuelve a trabajar al limite de energía original luego de un período de tiempo definido por el usuario.

El Escalamiento de Energía Dinámico puede configurarse vía web GUI, usando la Caja de Herramientas BOS+ o en el archivo de configuración ``/etc/bosminer.toml``.

Para hacer un cambio a la configuración vía web GUI, entre en el menú *Miner -> Configuration* y edite la sección *Dynamic Power Scaling*.

Para hacer un cambio a la configuración en múltiples dispositivos usando la **Caja de Herramientas BOS+**, siga los pasos en la sección :ref:`bosbox_configure`.

Para hacer un cambio en el archivo de configuración, conéctese el aminero vía SSH y edite el archivo ``/etc/bosminer.toml``. La sintaxis es la siguiente:

  ::

     [power_scaling]
     enabled = false
     power_step = 100
     min_psu_power_limit = 800
     shutdown_enabled = true
     shutdown_duration = 3.0

La línea *enabled* puede tener los valores *true* para activar el Escalamiento de Energía Dinámico, o *false* para desactivar el Escalamiento de Energía Dinámico.
El *power_step* puede tener valores numéricos (min. 100 y max. 1000), representando el escalón para bajar el límite de energía (powerlimit) en vatios (Watts), que ocurre cada vez que el minero alcance la *HOT* temperature (temperatura caliente).
El *min_psu_power_limit* puede tener valores numéricos (min. 100 y max. 5000), representando el límite mínimo de la fuente para el Escalamiento de Energía Dinámico. Si *psu_power_limit* está en el nivel *min_psu_power_limit* y el minero sigue *HOT* (caliente) y *shutdown_enabled* es true (verdadero), entonces el minero se apaga por un período de tiempo, definido en el valor *shutdown_duration* (duración de apagado) (en horas). Luego de eso, el minero es iniciado pero con el valor inicial de *psu_power_limit* (*PSU power limit* en la sección *Autotuning*) (autoajuste).

***************
Auto-actualizar
***************

Mientras auto-actualizar esté encendido, La máquina revisará periódicamente si hay una nueva versión de Braiins OS+ y actualizará a ella automáticamente cuando la encuentre. Esta característica se enciende por defecto al cambiar desde el firmware de fábrica, pero debe ser encendida manualmente al actualizar desde versiones anteriores de Braiins OS o Braiins OS+.

Auto-actualizar puede configurarse tanto vía web GUI o usando la Caja de Herramientas BOS+.

Para hacer un cambio a la configuración vía web GUI, entre en el menú *System -> Upgrade* y edite la sección *System Upgrade*.

Para hacer un cambio ala configuración en múltiples dispositivos  usando la **Caja de Herramientas BOS+**, siga los pasos en la sección :ref:`bosbox_configure`.

Alternativamente, es posible **apagar** auto-actualizar durante la instalación especificando el argumento ``--no-auto-upgrade`` en el comando de instalación.

************
Password SSH
************

Puede poner el password del minero via SSH desde un host remoto al correr el comando de abajo y reemplazar *[passwordnuevo]* con su propio password.

  * Nota: Braiins OS+ **no*** mantiene el historial de los comandos ejecutados.

  .. code:: bash

     ssh root@[minero-hostname-o-ip] 'echo -e "[passwordnuevo]\n[passwordnuevo]" | passwd'

Para hacer eso en muchos hosts en paralelo podría usar `p-ssh <https://linux.die.net/man/1/pssh>`__.

******************
Dirección MAC e IP
******************

Por defecto, la dirección MAC del dispositivo se mantiene igual y es heredada del firmware (de serie o Braiins OS) almacenada en el dispositivo (NAND). De esta forma, una vez que el dispositivo inicie con Braiins OS+, tendrá la misma dirección IP que tenía con el firmware de fábrica.

Alternativamente, puede especificar una dirección MAC de su selección al modificar el parametro ``ethaddr=`` en el archivo ``uEnv.txt`` (ubicado en la primera partición FAT de la tarjeta SD).

*******************
Detección de Modelo
*******************

Esta opción de configuración permite saltar el resultado de la auto-detección de hardware y honrar el tipo preseleccionado en la configuración. Esto es para cubrir la situación en donde las 3 tarjetas de hash tienen EEPROM corrompido. Si es activado, el modelo se toma de la opción **[format] - model**.

Para activar esta funcionalidad, añada las siguientes líneas al archivo ``/etc/bosminer.toml``.

  ::

     [model_detection]
     use_config_fallback = true

Aleternativamente, añada las líneas usando el comando siguiente:

  .. code:: bash

     ssh root@IP_ADDRESS 'echo -e "\n[model_detection] \nuse_config_fallback = true" >> /etc/bosminer.toml'