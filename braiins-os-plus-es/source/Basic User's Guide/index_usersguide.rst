
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#######################
Guía Básica del Usuario
#######################

.. contents::
	:local:
	:depth: 2

****************
Estadísticas Web
****************

Las estadísticas que se muestran en la página principal de la Web GUI se explican abajo.

Hash Chains
===========

   * **ID**                    - ID de Cadena de Hash, corresponde con el rotulado en la tarjeta controladora
   * **REAL HASH RATE**        - Tasa de hash actual en la cadena de hash
   * **NOMINAL HASH RATE**     - Tasa de hash teórica en la cadena de hash, basada en la frecuencia de los chips
   * **VOLTAGE**               - Voltaje usado en la cadena de hash
   * **FREQUENCY**             - Frecuencia de los chips (promedio)
   * **BOARD TEMP**            - Temperatura reportada por los sensores en la tarjeta de hash
   * **CHIP TEMP**             - Temperatura reportada por los sensores en el chip
   * **ASIC#**                 - Número de chips ASIC funcionando
   * **CORE#**                 - Summary of active cores on all functioning chips
   * **HARDWARE ERRORS**       - Número de fallos de hardware - trabajo inválido debido a cálculo erróneo de hardware
   * **HW ERROR HASH RATE**    - "Pérdida" en la tasa de hash debido a errores de HW; sumar la tasa de Errores de HW junto a la Tasa Real de Hash debería igualar la Tasa de Hash Nominal

Pools
=====

   * **ID**                    - El orden de los Pool, según especificó el usuario
   * **URL**                   - Dirección URL del pool de minado
   * **USER**                  - Nombre de usuario y nombre de worker, según especificó el usuario
   * **STATUS**                - Estado del pool - "Alive" cuando el pool está disponible para el minero, "Dead" cuando el pool no está disponible en ese URL
   * **ACTIVE**                - Estado activo: "Yes" - se está usando el pool para servir trabajos de minado; "No" - el pool no se está usando
   * **ACCEPTED**              - Número de shares enviados que fueron aceptados por el pool
   * **REJECTED**              - Número de shares enviados que fueron rechazados por el pool
   * **STALE**                 - Número de shares enviados para un trabajo que ya no es válido
   * **LAST DIFFICULTY**       - Dificultad del último share
   * **GENERATED WORK**        - Cantidad de trabajo generado a resolver para los chips
   * **ASICBOOST**             - Estado AsicBoost - "Yes" para habilitado, "No" para deshabilitado

Summary
=======

   * **HASH RATE 1M**          - Tasa promedio de hash durante el último minuto
   * **HASH RATE 15M**         - Tasa promedio de hash durante los últimos 15 minutos
   * **HASH RATE 24H**         - Tasa promedio de hash durante las últimas 24 horas
   * **FOUND BLOCKS**          - Número de bloques encontrados
   * **ACCEPTED**              - Número de shares enviados que fueron aceptados por el pool
   * **DIFFICULTY ACCEPTED**   - Dificultad del último share aceptado
   * **REJECTED**              - Número de shares enviados que fueron rechazados por el pool
   * **DIFFICULTY REJECTED**   - Dificultad del último share rechazado
   * **REJECTION RATIO**       - Tasa de shares rechazados y número total de shares (incluyendo aceptados)
   * **ELAPSED TIME**          - Tiempo transcurrido desde que BOSminer inició
   * **HARDWARE ERRORS**       - Número de fallos de hardware - trabajo rechazado debido a cálculo erróneo de hardware
   * **SHARES/1M**             - Cantidad promedio de shares aceptados por minuto

Fan Monitor
===========

   * **ID**                    - El orden de los ventiladores
   * **SPEED**                 - Velocidad del ventilador (% PWM)
   * **RPM**                   - Revoluciones por minuto del ventilador

*****************************
Señalización (LED) del Minero
*****************************

La señalización LED del minero depende de su modo operacional. Hay dos
modos (*recuperar* y *normal*) que son señalizados por los **LEDs verde**
y **rojo** en el panel frontal. El LED de la tarjeta controladora (adentro)
siempre muestra el *latido* (ej: parpadea a una tasa basada en el promedio
de carga).

Modo Recuperar
==============

El modo recuperar es señalizado por el **LED verde parpadeante** (50 ms
encendido, 950 ms apagado) en el panel frontal. El **LED rojo** representa
acceso a un disco NAND y parpadea durante el reinicio de fábrica cuando
los datos se escriben a NAND.

Modo Normal
===========

El estado de modo normal es señalizado mediante la combinación del panel
frontal **LEDs rojo** y **verde** como se especifica en la tabla abajo:

   +--------------------+---------------------------+--------------------+
   | LED rojo           | LED verde                 | significa          |
   +====================+===========================+====================+
   | encendido          | apagado                   | *bosminer* o       |
   |                    |                           | *bosminer_monitor* |
   |                    |                           | no está corriendo  |
   +--------------------+---------------------------+--------------------+
   | parpadeo lento     | apagado                   | tasa de hash 80%   |
   |                    |                           | por debajo de la   |
   |                    |                           | tasa de hash       |
   |                    |                           | esperada o el      |
   |                    |                           | minero no puede    |
   |                    |                           | conectarse con     |
   |                    |                           | ninguno de los     |
   |                    |                           | pool (todos los    |
   |                    |                           | pool muertos)      |
   +--------------------+---------------------------+--------------------+
   | apagado            | parpadeo muy lento (1 seg | el *minero* está   |
   |                    | encendido, 1 seg apagado) | operativo y su     |
   |                    |                           | tasa de hash sobre |
   |                    |                           | 80% de la tasa de  |
   |                    |                           | hash esperada      |
   +--------------------+---------------------------+--------------------+
   | parpadeo rápido    | N/A                       | Anulación LED      |
   |                    |                           | solicitada por     |
   |                    |                           | usuario (``miner   |
   |                    |                           | fault_light on ``) |
   +--------------------+---------------------------+--------------------+

*********************
Identificar un minero
*********************

Parpadeo LED
============

La herramienta local del minero también puede usarse para identificar un
dispositivo en particular al activar el parpadeo agresivo del **LED rojo**:

.. code:: bash

   miner fault_light on

De forma Similar, para desactivar el LED corra:

.. code:: bash

   miner fault_light off

Script discover
===============

El script *discover.py* puede ser usado para descubrir dispositivos
mineros soportados en la red local y tiene dos modos de trabajo.
Primero, clona el repositorio y prepara el ambiente usando los siguientes
comandos:

.. code:: bash

    # clone repository
    git clone https://github.com/braiins/braiins-os.git
    
    cd braiins-os/braiins-os/
    virtualenv --python=/usr/bin/python3 .env
    source .env/bin/activate
    python3 -m pip install -r requirements.txt

Modo escuchar
-------------

En este modo, las direcciones IP y MAC del dispositivo son mostradas
luego de presionar el botón IP Report. El parámetro ``--format`` puede
usarse para cambiar el formato predeterminado de la información IP/MAC.

.. code:: bash

   python3 discover.py listen --format "{IP} ({MAC})"

   10.33.10.191 (a0:b0:45:02:f5:35)

Modo buscar
-----------

En este modo, el script busca dispositivos soportados en el rango de red
especificado. Se espera que el parámetro incluya una lista de direcciones
IP o una subred IP con una máscara (ejemplo abajo) para buscar toda una
subred.

Por cada dispositivo, la salida incluye una dirección MAC, dirección IP,
información de sistema, nombre host, y un nombre de usuario minero
configurado.

.. code:: bash

   python3 discover.py scan 10.55.0.0/24

   00:7e:92:77:a0:ca (10.55.0.133) | bOS am1-s9_2018-11-27-0-c34516b0 [nand] {1015120 KiB RAM} dhcp(miner-w3) @userName.worker3
   00:94:cb:12:a0:ce (10.55.0.145) | Antminer S9 Fri Nov 17 17:57:49 CST 2017 (S9_V2.55) {1015424 KiB RAM} dhcp(antMiner) @userName.worker5

*******************************
Entrar/Salir del Modo Recuperar
*******************************

Los usuarios típicamente no tienen que entrar en modo recuperar mientras
usan Braiins OS de manera estándar. El proceso ``restore2factory.py``
para degradar lo usa para restaurar el firmware de fábrica original del
fabricante. También puede ser útil para reparar o investigar en un sistema
actualmente instalado.

El modo recuperar puede ser invocado de las siguientes maneras:

   *  *Botón IP SET button* - mantener presionado por *3s* hasta que el LED verde parpadee
   *  *tarjeta SD* - la primera partición en FAT conteniendo el archivo *uEnv.txt* con una línea **recovery=yes**
   *  *herramienta minero* - llamar ``miner run_recovery`` desde la línea de comandos del minero

Se puede salir del modo recuperar reiniciando el dispositivo. Si el dispositivo reinicia al modo recuperar,
significa que hay un problema con la instalación o configuración.
