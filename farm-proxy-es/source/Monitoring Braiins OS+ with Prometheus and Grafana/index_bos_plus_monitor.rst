
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

.. _monitoring:

=================================================
Monitoreo de Braiins OS+ con Prometheus y Grafana
=================================================

.. contents::
  :local:
  :depth: 2

Introducción
============

A partir de Braiins OS+ 22.02.2, cada minero produce estadísticas en un formato que Prometheus puede digerir fácilmente. Los datos de Prometheus se pueden visualizar en Grafana.

Prometheus es un conjunto de herramientas de monitoreo y alerta de sistemas de código abierto. Prometheus recopila y almacena sus métricas como datos de series temporales, es decir, la información de las métricas se almacena con la marca de tiempo en la que se registró, junto con pares clave-valor opcionales denominados etiquetas.

Grafana open source es un software de análisis y visualización de código abierto. Le permite consultar, visualizar, alertar y explorar sus métricas. Le proporciona herramientas para convertir los datos de su base de datos de series temporales en gráficos y visualizaciones interesantes.

Prometheus + Grafana es casi un estándar de la industria para monitoreo, visualización y alertas. Para Braiins OS+, el punto final para las métricas de Prometheus está disponible en ``[IP ADDRESS]:8081/metrics``.

.. attention::
   
   El monitoreo se puede ejecutar como una aplicación independiente o ya está incluido en la pila acoplable Braiins Farm Proxy.

Limitations
-----------

-  Solo un conjunto limitado de métricas está disponible para dispositivos S9
-  El monitoreo solo funciona para mineros con Braiins OS+ instalado
-  Las arquitecturas admitidas para el escáner ssh son Linux AMD64, RPi de 64 o 32 bits

Configuración
=============

Inicio Rápido
-------------

Inicio rápido para el caso de que el monitoreo se ejecute como una aplicación independiente (no como parte de Braiins Farm Proxy).

- ``git clone https://github.com/braiins/bos-farm-monitor.git``
- Cambiar la lista de rangos de direcciones IP para escanear ``./scan_crontab``
- ``docker-compose up -d``
- Ir a localhost:3000

Requisitos Previos
------------------

**Linux**

-  Siga `las instrucciones de instalación de la ventana acoplable <https://docs.docker.com/engine/install/ubuntu/>`__
-  Siga `los pasos opcionales de instalación de la ventana acoplable <https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user>`__
-  Siga `la instalación de docker-compose <https://docs.docker.com/compose/install/>`__

**RPi**

-  Siga uno de los muchos manuales, por ejemplo, https://jfrog.com/connect/post/install-docker-compose-on-raspberry-pi/

Instalación
-----------

Inicio rápido en caso de que el monitoreo se ejecute como una aplicación independiente (no como parte de Braiins Farm Proxy).

**Verificar prerrequisito instalado**

.. code-block::

    docker version
    docker-compose --version
    git --version

**Descarga el repositorio de Brains**

Puedes clonar el repositorio usando git:

.. code-block::

   sudo apt update
   sudo apt install git
   git clone https://github.com/braiins/bos-farm-monitor.git

Puede descargar el archivo zip con todos los archivos `https://github.com/braiins/bos-farm-monitor/archive/refs/heads/master.zip <https://github.com/braiins/bos-farm-monitor/archive/refs/heads/master.zip>`__

Configuración de Prometheus
---------------------------

Antes de que pueda comenzar a monitorear su granja, deberá preparar 
la configuración basada en ejemplos en el directorio de configuración. 
Hay dos archivos:

-  ``/config/prometheus_scan.yml``
-  ``/config/prometheus_static.yml``

La única diferencia importante entre los dos es que es mejor usar "escanear" en caso de que sus mineros 
tengan direcciones IP asignadas por DHCP, mientras que "estática" se puede usar cuando sus mineros tienen direcciones IP estáticas.

**Configuración predeterminada**

La configuración predeterminada en tiene las siguientes características:

-  Trabajo para raspar métricas de Braiins OS+ llamado braiinsos-data
-  Reetiquetado de direcciones de extremos de métricas (eliminación del puerto 8081)
-  Análisis de direcciones IP:

   -  Segundo octeto: etiqueta ``site_id``
   -  Tercer octeto: etiqueta ``subnet_id``
   -  Cuarto octeto: etiqueta ``host_id``

-  Eliminación de algunas métricas más intensivas en datos (puede volver a agregarlas, solo asegúrese de que su instancia tenga el tamaño adecuado)
-  Creación de etiquetas estáticas para prometheus_static.yml: la etiqueta se asigna dinámicamente cuando se usa prometheus_scan.yml (más sobre esto más adelante).

**Estructure su granja para una buena observabilidad**

Para una granja más grande, es posible que desee agrupar a los mineros en algunas agrupaciones lógicas para que pueda ver el rendimiento de los componentes individuales. La agrupación puede diferir según el tamaño y la estructura de su granja, algunos de los elementos más típicos en la topología de la granja son:

-  Edificio
-  Sección
-  Tanque
-  Pasillo
-  Fila

Para conseguirlo tienes las siguientes opciones:

 **Use subredes y analice octetos de direcciones IP**
   Si tiene direcciones IP estáticas y las está utilizando para organizar a sus mineros, la forma más fácil de preparar los datos para los informes es mejorar la configuración de Prometheus con etiquetas derivadas de las direcciones IP. El siguiente ejemplo muestra cómo hacerlo. Obviamente, puede usar nombres diferentes a sección, tanque, minero.

   .. code-block::

      relabel_configs:
      # Extract the second octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.(\\d+)\\.\\d+\\.\\d+.*"
        target_label: "section"
      # Extract the third octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.\\d+\\.(\\d+)\\.\\d+.*"
        target_label: "tank"
      # Extract the last octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.\\d+\\.\\d+\\.(\\d+).*"
        target_label: "miner"
 
 **Use trabajos separados junto con una etiqueta personalizada opcional**
   Una configuración de Prometheus (almacenada en prometheus.yml) puede contener varios trabajos. Por ejemplo, puede crear trabajos separados para cada edificio o contenedor. Cada métrica tiene una etiqueta de trabajo, lo que lo convierte en un enfoque muy conveniente para agrupar instancias (mineros). En caso de que tenga otros trabajos (que no sean de minería) en su configuración, es posible que desee agregar una etiqueta personalizada a cada trabajo para que pueda usar esa etiqueta para filtrar/agrupar. Un ejemplo que podría usarse en la sección relabel_configs para agregar una etiqueta de edificio a cada instancia que es monitoreada por el trabajo con el valor "Edificio A":

   .. code-block::

      - target_label: "building"
        replacement: "Building A"
 **Usar múltiples instancias de Prometheus**
   En el caso de miles o más mineros, podría ser más fácil configurar una instancia de Prometheus separada para cada grupo de mineros. Consulte la documentación de Prometheus sobre cómo configurar la `federación <https://prometheus.io/docs/prometheus/latest/federation/>`__.

 **Usar nombre de usuario/nombre de trabajador y volver a etiquetar (no recomendado)**
   El uso de nombre de usuario/nombre de trabajador para codificar información sobre la ubicación física de los mineros es un enfoque que se usa típicamente con aplicaciones de monitoreo heredadas. Este enfoque no funciona bien con la forma en que Prometheus administra y almacena series temporales, que no se parece en nada a una base de datos relacional tradicional. No recomendamos usar nombre de usuario/nombre de trabajador para estructurar su granja con Prometheus por las siguientes razones:

   -  la mayoría de las métricas no tienen el nombre del trabajador, ya que las etiquetas y las uniones deberían crearse en las consultas (ralentizan las cosas, son propensas a errores)
   -  puede haber múltiples nombres de usuario/nombres de trabajadores asociados con un solo minero; esto hace que las uniones sean aún más difíciles (agregación previa necesaria con lógica qué valor elegir)

 **Use múltiples rangos de IP con enfoque de escaneo**
   Si tiene mineros con IP asignada por DHCP y está utilizando el escaneo de su red para llevar a los mineros a Prometheus, puede definir múltiples rangos de red y cada rango puede tener un valor único definido y asignado a la etiqueta (más sobre eso en la siguiente sección ).

**Agregar mineros a la configuración**

Existen las siguientes opciones básicas sobre cómo agregar sus mineros a la configuración:

-  Utilice las opciones de detección de servicios proporcionadas por Prometheus
-  Enumere las direcciones IP en el archivo de configuración manualmente

La lista de direcciones IP directamente funciona mejor cuando las direcciones IP asignadas a los mineros son estáticas. En el caso de DHCP, el descubrimiento de servicios es una mejor opción.

**Descubrimiento de servicios**

El descubrimiento de servicios basado en archivos es la opción habilitada de forma predeterminada. Para comenzar a usarlo, deberá configurar el archivo ``./scan_crontab`` en un editor de texto. Los ejemplos actuales son:

.. code-block::

    * */3 * * * * * ssh_scan.sh "1.2.3.0-255" "Building A"
    * */3 * * * * * ssh_scan.sh "1.2.0-255.3" "Building B"

Cada línea escaneará el rango de IP definido para los mineros que respondan y almacenará la lista para que esté disponible para Prometheus. La cadena "Edificio A" / "Edificio B" puede ser un nombre arbitrario. Actualmente, se asignará dinámicamente a la creación de etiquetas. El escaneo se realiza cada tres minutos; puede cambiarlo según el tamaño de su granja y sus necesidades. En caso de que no esté familiarizado con la sintaxis de cron, se explica `aquí <https://www.netiq.com/documentation/cloud-manager-2-5/ncm-reference/data/bexyssf.html>`__.

**Listar direcciones IP**

Para usar una lista estática de direcciones IP, debe cambiar el archivo ``docker-compose.yml``,

Primero, comente la imagen crontab para que el análisis dinámico esté deshabilitado:

.. code-block::

   # bos_scanner:
   # image: braiinssystems/bos_monitor:v1.0.0
   # container_name: bos_scanner
   # volumes:
   #  - ./scan_crontab:/usr/local/share/scan_crontab
   #  - scanner_data:/mnt:rw
   # network_mode: "host"

En segundo lugar, comente el escaneo dinámico y habilite el uso de un archivo de configuración diferente. Debería verse así después de los cambios:

.. code-block::

   #- '--config.file=/etc/prometheus/prometheus_scan.yml'
   - '--config.file=/etc/prometheus/prometheus_static.yml'

Las direcciones IP se enumeran como una matriz en el archivo de configuración
`prometheus_static.yml`. Cambia las entradas con la lista de tus mineros:

.. code-block:

   - targets: ['10.35.31.2:8081','10.35.32.2:8081']

Tenga en cuenta que:

-  El puerto debe agregarse al final de la dirección IP. El puerto 8081 es donde están disponibles las métricas de Prometheus
-  Las direcciones IP se citan y se separan por comas

En caso de que no tenga direcciones IP estáticas, la dirección IP de cualquier minero puede cambiar. Si aún desea utilizar este enfoque estático, intente aumentar el tiempo de concesión a un valor alto (por ejemplo, 48 horas) para su servidor DHCP, de modo que la dirección IP se reasigne incluso cuando el minero esté desconectado durante algún tiempo.

Para incluir a todos los mineros en la lista, puede escanear su granja en busca de dispositivos utilizando BOS Toolbox y generar la configuración a partir de los resultados. Puede usar UX o la línea de comandos para obtener la lista.

Ejemplo de línea de comandos (linux):

.. code-block::

   ./bos-toolbox scan -o ips.txt 10.10.0.0/16
   cat ips.txt \| sed "s/.*/'&:8081'/" \| paste -sd',' \| sed "s/.*/[&]/"

El primer comando escaneará todas las direcciones IP en el rango 10.10.0.0 y 10.10.255.255. El segundo imprimirá una matriz con direcciones IP que puede pegar en la configuración.

Solo se pueden monitorear los mineros con Braiins OS+. En caso de que esté usando mineros sin Braiins OS+, es mejor usar:

.. code-block::
   
   ./bos-toolbox scan 10.10.0.0/16 &> ips.txt
   grep "\| bOS" ips.txt \| cut -d"(" -f2 \| cut -"d)" -f1 \| sed "s/.*/'&:8081'/" \| paste -sd',' \| sed "s/.*/[&]/"

Para diferentes rangos de IP puedes usar:

-  10.10.10.0/24 para el rango 10.10.10.0 - 10.10.10.255
-  10.10.0.0/16 para el rango 10.10.0.0 a 10.10.255.255
-  10.0.0.0/8 para el rango 10.0.0.0 a 10.255.255.25

Empezar a monitorear
--------------------

.. code-block::

   docker-compose up -d

Puede verificar que el contenedor se está ejecutando usando `docker ps`.

Ahora puede ir a: `http://<su_host>:3000`.

Operaciones
-----------

**Cambio de configuración**

Cambie el archivo de configuración según sus necesidades

.. code-block::

   docker-compose restart prometheus

**Actualizando a una versión más reciente**

.. code-block::

   git pull origin master
   docker-compose up -d

Tableros
========

En nuestro repositorio, proporcionamos paneles de muestra que pueden ayudarlo a comenzar a preparar el monitoreo para su granja que mejor se adapte a sus necesidades.

Tablero de la granja
--------------------

Este es el panel de control de alto nivel que monitorea a todos los mineros en su granja. Tiene un selector de fuente de datos incorporado en caso de que tenga varias instancias de Prometheus ejecutándose. También presenta varios informes detallados resaltados en la siguiente captura de pantalla:

  .. |pic3| image:: ../_static/monitoring_dashboard.png
      :width: 100%
      :alt: Dashboard

  |pic3|

Las partes resaltadas en rojo lo llevarán a un informe detallado que enumera las instancias. Las partes resaltadas en azul irán directamente al minero UX.

Ejemplo de tablero de granja: por edificio
------------------------------------------

Dashboard tiene una característica donde las filas de paneles de grafana se muestran automáticamente para cada edificio definido. Esto se crea dinámicamente en función de los valores de la etiqueta del edificio. El flujo completo es el siguiente en la configuración de ejemplo:

-  se crean dos trabajos separados en prometheus.yml
-  cada trabajo tiene una etiqueta de construcción agregada con un valor que representa la construcción
-  El panel de grafana tiene un parámetro de construcción definido que está vinculado a la etiqueta de construcción
-  el encabezado de la fila tiene $building como nombre; esto se expandirá con valores de etiqueta
-  cada panel tiene $building como filtro

Métricas y Etiquetas
====================
Cada serie temporal se identifica de forma única por su nombre de métrica y pares clave-valor opcionales llamados etiquetas. El nombre de la métrica especifica la característica general de un sistema que se mide. Las etiquetas habilitan el modelo de datos dimensionales de Prometheus: cualquier combinación dada de etiquetas para el mismo nombre de métrica identifica una instancia dimensional particular de esa métrica. El lenguaje de consulta permite filtrar y agregar en función de estas dimensiones.

Visión general:

-  ``application_version_details (instance, version_full, toolchain)``
-  ``client_status (instance, connection_type, host, protocol, user, worker)``
-  ``hashboard_nominal_hashrate_gigahashes_per_second (instance, hashboard)``
-  ``hashboard_shares (instance, hashboard, type: valid | invalid | duplicate)``
-  ``miner_metadata (instance, model, os_version)``
-  ``miner_power (instance, type: wall | estimate | limit, socket)``
-  ``temperature (instance, chip_addr, chip_in_domain, voltage_domain,hashboard, location: chip | pcb)``
-  ``stratum_accepted_shares_counter (instance, host, user, worker, protocol, connection_type)``
-  ``stratum_rejected_shares_counter (instance, host, user, worker, protocol, connection_type)``
-  ``stratum_accepted_submits_counter (instance, host, user, worker, protocol, connection_type)``
-  ``stratum_rejected_submits_counter (instance, host, user, worker, protocol, connection_type)``
-  ``tuner_stage (instance, hashboard)``

Application Version Details
---------------------------

Versión de la aplicación que está produciendo series temporales.

``application_version_details``

**Etiquetas**

-  instance: dirección IP del minero
-  version_full: versión de la aplicación
-  toolchain
   
Client Status
-------------

Estado del cliente: (detenido = 0, en ejecución = 1, fallido = -1)

``client_status``

**Etiquetas**

-  instance: dirección IP del minero
-  connection_type: tipo de conexión, que puede ser de *user* o *dev-fee*
-  host: URL del host, generalmente URL del grupo o proxy
-  protocol: protocolo de minería
-  user: generalmente el nombre de usuario del grupo de minería del cliente
-  worker: nombre del trabajador


Hashboard Nominal Hashrate (Gh/s)
---------------------------------

Hashrate nominal de cada hashboard en Gh/s.

``hashboard_nominal_hashrate_gigahashes_per_second``

**Etiquetas**

-  instance: dirección IP del minero
-  hashboard: rango del hashboard

Hashboard Shares
----------------

Número de acciones válidas producidas por hashboards. Las acciones de hashboard se pueden usar para calcular el hashrate real para hashboard, minero u otro grupo. Esta métrica no proporciona información sobre si el destino aceptó las acciones; para esto, se debe usar stratum_accepted_shares_counter.

``hashboard_shares (counter)``

**Etiquetas**

-  instance: dirección IP del minero
-  hashboard: rango del hashboard
-  type: tipo de las acciones con respecto a su vigencia, *valid* - acciones válidas, *invalid* - acciones no válidas, *duplicate* - acciones duplicadas

**Ejemplos**

Promedio de hashes por segundo durante los últimos 20 segundos para todas las instancias:

.. code-block::

   sum(rate(hashboard_shares[20s])) * 2^32

Promedio de hashes por segundo durante los últimos 20 segundos por instancia:

.. code-block::

   sum by(instance) (rate(hashboard_shares[20s])) * 2^32

Promedio de hashes por segundo durante los últimos 20 segundos para todas las instancias por tipo de minero:

.. code-block::

   sum by (model) (
      (sum by (instance)((rate(hashboard_shares[20s]))) * 2^32)
      * on(instance) group_left(model) count by (instance, model) (miner_metadata)
   )

Miner Metadata
--------------

``miner_metadata``

**Etiquetas**

- instance: dirección IP del minero
- model: modelo del minero
- os_version: versión del firmware

**Ejemplos**

Número de mineros por modelo:

.. code-block::

   count_values by (model) ("x", miner_metadata)

Miner Power
-----------

``miner_power``

**Etiquetas**

-  instance: dirección IP del minero
-  type: 3 tipos, *estimated* - potencia estimada, *limit* - límite de potencia, *psu* - potencia medida, *wall*
-  socket

**Ejemplos**

Consumo total de energía estimado para todas las instancias:

.. code-block::

   sum(miner_power{type="estimated"})

Límite de potencia total para todas las instancias:

.. code-block::

  sum(miner_power{type="limit"})

Stratum Accepted Shares Counter
-------------------------------

Número total de acciones aceptadas por target. Por ejemplo, normalmente hay más objetivos, representados por la etiqueta de host.

``stratum_accepted_shares_counter (counter)``

**Etiquetas**

-  instance: dirección IP del minero
-  connection_type: tipo de conexión, que puede ser de *user* o *dev-fee*
-  host: URL del host, generalmente URL del grupo o proxy
-  protocol: protocolo de minería
-  user: generalmente el nombre de usuario del grupo de minería del cliente
-  worker: nombre del trabajador

**Ejemplos**

Número promedio de acciones aceptadas por segundo durante los últimos 20 segundos para todas las instancias por objetivo:

.. code-block::

   sum by(host) (rate(stratum_accepted_shares_counter[20s]))

Stratum Rejected Shares Counter
-------------------------------

Número total de acciones rechazadas por destino.

``stratum_rejected_shares_counter (counter)``

**Etiquetas**

-  instance: dirección IP del minero
-  connection_type: tipo de conexión, que puede ser de *user* o *dev-fee*
-  host: URL del host, generalmente URL del grupo o proxy
-  protocol: protocolo de minería
-  user: generalmente el nombre de usuario del grupo de minería del cliente
-  worker: nombre del trabajador

**Ejemplos**

Número promedio de acciones rechazadas por segundo durante los últimos 20 segundos para todas las instancias por destino:

.. code-block::

   sum by(host) (rate(stratum_rejected_shares_counter[20s]))

Stratum Accepted Submits Counter
--------------------------------

Número total de envíos aceptados por destino. Por ejemplo, normalmente hay más objetivos, representados por la etiqueta de host.

``stratum_accepted_submits_counter (counter)``

**Etiquetas**

-  instance: dirección IP del minero
-  connection_type: tipo de conexión, que puede ser de *user* o *dev-fee*
-  host: URL del host, generalmente URL del grupo o proxy
-  protocol: protocolo de minería
-  user: generalmente el nombre de usuario del grupo de minería del cliente
-  worker: nombre del trabajador

**Ejemplos**

Promedio de envíos aceptados por segundo durante los últimos 20 segundos para todas las instancias por objetivo:

.. code-block::

   sum by(host) (rate(stratum_accepted_submits_counter[20s]))

Stratum Rejected Submits Counter
--------------------------------

Número total de envíos rechazados por destino.

``stratum_rejected_submits_counter (counter)``

**Etiquetas**

-  instance: dirección IP del minero
-  connection_type: tipo de conexión, que puede ser de *user* o *dev-fee*
-  host: URL del host, generalmente URL del grupo o proxy
-  protocol: protocolo de minería
-  user: generalmente el nombre de usuario del grupo de minería del cliente
-  worker: nombre del trabajador

**Ejemplos**

Promedio de envíos rechazados por segundo durante los últimos 20 segundos para todas las instancias por objetivo:

.. code-block::

   sum by(host) (rate(stratum_rejected_submits_counter[20s]))


Temperature
-----------

Cada sensor de temperatura disponible proporcionará los datos. Puede haber un sensor en diferentes ubicaciones (pcb o chip).

``temperature``

**Etiquetas**

-  instance: dirección IP del minero
-  chip_addr
-  chip_in_domain
-  voltage_domain
-  hashboard
-  location: chip|pcb

**Ejemplos**

Temperatura máxima promedio en todas las instancias (mineros):

.. code-block::

   avg(max by (instance) (temperature))

Temperatura máxima promedio en todas las instancias (mineros) por tipo de minero:

.. code-block::

   avg by (model) (
     (max by (instance) (temperature)) * on (instance)
     group_left(model) count by (instance, model) (miner_metadata)
   )

Tuner stage
-----------

Etapa del sintonizador:

-  2: perfil de rendimiento de prueba
-  3: ajuste de chips individuales
-  4: estable
-  6: configuración manual en ejecución

``tuner_stage``

**Etiquetas**

-  instance: dirección IP del minero
-  hashboard: rango del hashboard

**Ejemplos**

Número de instancias por etapa:

.. code-block::

   count_values ("Stage", max by (instance) (tuner_stage))

Otros ejemplos
--------------

**Extraer partes de la dirección IP**

Si está administrando su granja asignando diferentes rangos de IP a diferentes partes de su granja, puede ser útil agrupar las métricas por octeto de dirección IP. Ejemplo de temperatura máxima de chip por 3er octeto:

.. code-block::

   max by (segment) (label_replace(
     temperature{location="chip"}, "segment", "$1", "instance","\\d+\\.\\d+\\.(\\d+)\\.\\d+.*"
   ))

Si necesita hacer esto para muchas/todas las métricas, es mejor tener partes de la dirección IP como etiquetas personalizadas. Consulte la sección Configuración con un ejemplo.

Obteniendo ayuda
================

Para obtener más información sobre Prometheus y Grafana, consulte la documentación oficial:

-  `Prometheus Documentation <https://prometheus.io/docs/introduction/overview/>`__
-  `Grafana Documentation <https://grafana.com/docs/>`__

En caso de que tenga preguntas específicas sobre el monitoreo de los mineros de Braiins OS+ con Prometheus y Grafana, comuníquese con nuestro equipo de soporte en Telegram.
