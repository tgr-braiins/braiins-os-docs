
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#############
Configuración
#############

.. contents::
  :local:
  :depth: 2

La configuración de Braiins Farm Proxy puede dividirse en dos categorías principales - configuración de enrutamiento y la configuración de los programas acompañantes (que son Prometheus y Grafana).

*****************************
Configuración de enrutamiento
*****************************

La configuración de enrutamiento se hace en el archivo TOML utilizando una estructura específica. La estructura del archivo de configuración corresponde con el diagrama de enrutamiento de tasa de hash y usa la terminología explicada arriba. La configuración es explicada en ajustes de ejemplo en el texto adicional.

Braiins Farm Proxy tiene 3 ejemplos predefinidos de configuraciones TOML que están ubicadas en el directorio *./farm-proxy/config*:

  * *minimal.toml*: el archivo mas pequeño de configuración que puede tener un servidor, un objetivo y agregación por defecto.
  * *sample.toml*: contiene algunos otros parámetros y comentarios explicándolos. Se usa como la configuración predeterminada en el archivo *docker-compose.yml*. También contiene un servidor, un objetivo.
  * *two_servers_two_targets.toml*: ejemplo de como exponer mas servidores a los mineros o definir un servidor de respaldo para Braiins Farm Proxy.

La configuración de enrutamiento consiste de 5 segmentos: Servidor, Objetivo, Enrutamiento, Meta de Enrutamiento y Nivel Meta de Enrutamiento.

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/grZ3aQG9mHE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Servidor
========

Server es una plantilla para conexiones downstream (aguas abajo). Cada servidor puede utilizarse solo por un dominio de enrutamiento.

.. code-block:: shell

      [[server]]
      name = "s1"
      port = 3336
      slushpool_bos_bonus = "<slushpool username>"
      bos_referral_code = "<Braiins OS+ referral code>"



* **name**: nombre del servidor. Es visible como un valor de la dimensión “server” en todas las métricas downstream (envíos, participaciones, conexiones) en monitoreo Grafana.
* **port**: define el puerto que Braiins Farm Proxy abrirá y aceptará conexiones de mineros.
* **slushpool_bos_bonus**: Nombre de usuario Slush Pool para el cual el bono Braiins OS+ es aplicado.
* **bos_referral_code**: Código de referido Braiins OS+.
   
Objetivo
========

Target es una plantilla para conexiones upstream (aguas arriba). Puede ser compartido entre varios dominios de enrutamiento.

.. code-block:: shell

      [[target]]
      name = "MP-GL1"
      url = "stratum+tcp://<mining-pool-address>"
      user_identity = "<userName.workerName>"

* **name**: nombre del objetivo. Es visible como un valor de la dimensión “upstream” en todas las métricas upstream (envíos, participaciones, conexiones) en monitoreo Grafana.
* **url**: URL (enlace) del pool minero.
* **user_identity**: identidad bajo la cual la tasa de hash será enviada. El **NombreUsuario** debe existir en el pool objetivo de lo contrario el pool no tiene la forma de asociar su tasa de hash con su cuenta.

Dominio de Enrutamiento
=======================

El dominio de enrutamiento define fronteras y preferencias para la asignación de tasa de hash a los objetivos deseados.

.. code-block:: shell

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal 1"
      hr_weight = 100
      [[routing.goal.level]]
      targets = ["MP-GL1"]

* **from**: Lista de servidores que son usados en Braiins Farm Proxy como proxies de agregación.
* **goal**: Lista de las reglas de enrutamiento. El atributo **name** del objetivo es visible en el tablero Grafana para medidas upstream relacionadas. El atributo **hr_weight** significa la proporción de la preferencia de distribución de la tasa de hash. Tenga cuidado del peso y no del porcentaje. Por ejemplo, la proporción de pesos 2:1 distribuirá la tasa de hash a los puntos finales objetivo aproximadamente 67% de la tasa de hash irá a un objetivo con peso 2 y 33% de la tasa de hash irá a un objetivo con peso 1. En el ejemplo de configuraciones mas abajo, puede ver como distribuir la tasa de hash en varios objetivos.
* El nivel de la meta de enrutamiento lista los **targets** que serán aplicados como puntos finales upstream.

En caso de que el granjero use Braiins OS+ en sus dispositivos, **el enrutamiento de la tasa de desarrollo se hace automáticamente.**

Configuración de los Equipos
============================

Para apuntar la tasa de hash al Braiins Farm Proxy, los equipos deben ser reconfigurados. El (enlace) URL del pool en el firmware de los equipos debe colocarse como:

 * Stratum V1: ``stratum+tcp://<dirección-farm-proxy>:<puerto_servidor>``
 *  Stratum V2: ``stratum2+tcp://<dirección-farm-proxy>:<puerto_servidor>/<llave_pública>``

Se recomienda tener una conexión pool de respaldo en su minero también en caso de que Braiins Farm Proxy no esté funcionando.

Configuraciones de Ejemplo
==========================

Para tener un mejor entendimiento del uso y configuración de Braiins Farm Proxy, vayamos a través de 3 ejemplos.

* **Configuración mínima**: la configuración mas fácil posible, un servidor un pool objetivo. No es adecuado para el mundo real por su simplicidad pero describe la lógica de la configuración.

.. code-block:: shell

      # Minimal sample configuration
      [[server]]
      name = "s1"                                
      port = 3336

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "simpleFarm.worker"

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal 1"
      [[routing.goal.level]]
      targets = ["SP-GL"]


* **Configuración básica**: Ejemplo de una operación minera en un en una sola instalación ubicada en Europa. El objetivo primario es Slush Pool (dirección EU), pero está respaldada por direcciones general y Rusa. La granja tiene 700 máquinas ASIC y su agregación deseada es 100. Eso significa que deben haber entre 6 y 7 conexiones upstream hacia el target. El ingreso de la granja es aumentado por utilizar el firmware BOS+ y minar en Slush Pool.

.. code-block:: shell

      # Basic sample configuration
      [[server]]
      name = "s1"
      port = 3336

      [[target]]
      name = "SP-EU"
      url = "stratum+tcp://eu.stratum.slushpool.com"
      user_identity = "basicFarm.proxy"
      aggregation = 100

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "basicFarm.proxy"
      aggregation = 100

      [[target]]
      name = "SP-RU"
      url = "stratum+tcp://ru-west.stratum.slushpool.com"
      user_identity = "basicFarm.proxy"
      aggregation = 100

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal 1"
      # Primary
      [[routing.goal.level]]
      targets = ["SP-EU"]
      # Back-up 1
      [[routing.goal.level]]
      targets = ["SP-GL"]
      # Back-up 2
      [[routing.goal.level]]
      targets = ["SP-RU"]

* **Dueños múltiples de los equipos**: La granja tiene equipos dedicados a la minería en Slush Pool con el puerto de escucha 3336 y otros equipos dedicados a la minería en Antpool en el puerto 3337. Antpool requiere un extranonce máximo de 4 y debe ser configurado en la configuración de  Braiins Farm Proxy configuration. Esta configuración de ejemplo es adecuada en el caso de que los equipos tengan 2 dueños y por o tanto múltiples servidores son definidos y utilizados. Instancias múltiples de Braiins Farm Proxy (digamos en nuestro ejemplo que son 2 máquinas Raspberry Pi) con 2 configuraciones diferentes pueden ser utilizadas.
   
.. code-block:: shell

      # Advanced sample configuration
      [[server]]
      name = "s1"
      port = 3336

      [[server]]
      name = "s2"
      port = 3337
      extranonce_size = 2

      [[target]]
      name = "SP-EU"
      url = "stratum+tcp://eu.stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50                                                      

      [[target]]
      name = "Antpool-1"
      url = "stratum+tcp://ss.antpool.com:3333"
      user_identity = "antPoolUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[target]]
      name = "Antpool-2"
      url = "stratum+tcp://ss.antpool.com:443"
      user_identity = "antPoolUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[routing]]
      from = ["s1","s2"]
      [[routing.goal]]
      name = "Goal SP"
      # Primary Slush Pool
      [[routing.goal.level]]
      targets = ["SP-EU"]
      # Back-up Slush Pool
      [[routing.goal.level]]
      targets = ["SP-GL"]
      #
      [[routing.goal]]
      name = "Goal Ant"
      # Primary Antpool
      [[routing.goal.level]]
      targets = ["Antpool-1"]
      # Back-up Antpool
      [[routing.goal.level]]
      targets = ["Antpool-2"]

* **Diversificación de pools**: Una granja que dedica su tasa de hash a 3 pools utilizando 1 instancia servidor de Braiins Farm Proxy y múltiples puntos finales objetivo con una asignación de tasa de hash 100:80:20 ~ aproximadamente 50% de la tasa de hash va para la meta “Goal SP”, 40% de la tasa de hash va para la meta “Goal Ant” y 10% va para la meta “Goal BTC.com”.

.. code-block:: shell

      # Diversification of pools
      [[server]]
      name = "s1"
      port = 3336
      extranonce_size = 2

      [[target]]
      name = "SP-EU"
      url = "stratum+tcp://eu.stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50

      [[target]]
      name = "SP-GL"
      url = "stratum+tcp://stratum.slushpool.com"
      user_identity = "slushPoolUser.proxy"
      aggregation = 50

      [[target]]
      name = "Antpool-1"
      url = "stratum+tcp://ss.antpool.com:3333"
      user_identity = "antUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[target]]
      name = "Antpool-2"
      url = "stratum+tcp://ss.antpool.com:443"
      user_identity = "antUser.proxy"
      aggregation = 50
      extranonce_size = 4

      [[target]]
      name = "BTCcom-1"
      url = "stratum+tcp://eu.ss.btc.com:1800"
      user_identity = "btcUser.proxy"
      aggregation = 50

      [[target]]
      name = "BTCcom-2"
      url = "stratum+tcp://eu.ss.btc.com:443"
      user_identity = "btcUser.proxy"
      aggregation = 50

      [[routing]]
      from = ["s1"]
      [[routing.goal]]
      name = "Goal SP"
      hr_weight = 100
      # Primary Slush Pool
      [[routing.goal.level]]
      targets = ["SP-EU"]
      # Back-up Slush Pool
      [[routing.goal.level]]
      targets = ["SP-GL"]
      #
      [[routing.goal]]
      name = "Goal Ant"
      hr_weight = 80
      # Primary Antpool
      [[routing.goal.level]]
      targets = ["Antpool-1"]
      # Back-up Antpool
      [[routing.goal.level]]
      targets = ["Antpool-2"]
      #
      [[routing.goal]]
      name = "Goal BTC.com"
      hr_weight = 20
      # Primary BTC.com
      [[routing.goal.level]]
      targets = ["BTCcom-1"]
      # Back-up BTC.com
      [[routing.goal.level]]
      targets = ["BTCcom-2"]

* **Ubicación diferente de la operación minera**: Granjas de minería con varios contenedores mineros o en edificios en ubicaciones distintas usarían una instancia de Braiins Farm Proxy para cada una de las ubicaciones o por cada contenedor con un servidor downstream y un objetivo upstream con distintos identificadores de equipo para cada ubicación / contenedor para diferenciar la tasa de hash de cada ubicación / contenedor. Es posible enlazar los Farm Proxies jerárquicamente para agregar tasa de hash desde Farm Proxies de contenedores individuales via otra instancia de Braiins Farm Proxy.
   
Parametros de Configuración
===========================

Lista de parametros tanto obligatorios como opcionales disponibles en la configuración Braiins Farm Proxy configuration. Los parámetros son asignados a las secciones de configuración correspondientes.

Server
------

 * **name**: frase: distingue mayúsculas y minúsculas con longitud mínima 1 (obligatorio), nombre del servidor,
 * **port**: entero (obligatorio), puerto dedicado al Braiins Farm Proxy,
 * **extranonce_size**: entero (opcional), extranonce provisto al dispositivo downstream (ASIC), debe ser al menos 2 menos que *extranonce_size* del *target*, por defecto es *4*,
 * **validates_hash_rate**: booleano (true/false, opcional), parámetro que define si el proxy debe validar el envío de downstream, por defecto es *true*,
 * **use_empty_extranonce1**: booleano (true/false, opcional), parámetro que define si 1 byte mas de extra nonce puede ser usado (no todo dispositivo lo admite), por defecto es *false*,
 * **submission_rate**: real (opcional), tasa de presentación downstream deseada (minero -> proxy) definida como número de envíos por segundo, por defecto es *0.2* (1 envío cada 5 segundos),
 * **slushpool_bos_bonus**: frase: distingue mayúsculas y minúsculas con longitud mínima 0 (opcional), Nombre de usuario Slush Pool a quien se le aplica el descuento de Braiins OS+,
 * **bos_referral_code**: frase: distingue mayúsculas y minúsculas con longitud mínima 6 (opcional), El código de remisión Braiins OS+ se le proporcionará con toda su longitud para recibir el bono.
   
Target
------

 * **name**: frase: distingue mayúsculas y minúsculas con longitud mínima 1 (obligatorio), nombre del punto final objetivo,
 * **url**: string (obligatorio), dirección URL del pool de minería,
 * **user_identity**: frase: distingue mayúsculas y minúsculas con longitud mínima 1 (obligatorio),
 * **identity_pass_through**: booleano (true/false, opcional), propagación de la identidad de un equipo individual al pool objetivo (caracteristica de envío para upstream), por defecto *false*,
 * **extranonce_size**: entero (opcional), extranonce aplicado al pool objetivo, debe ser al menos 2 mas que *extranonce_size* del *server*, por defecto es *6* (**¡algunos pool requieren extranonce como máximo 4!: AntPool, Binance Pool, Luxor**),
 * **aggregation**: entero (opcional), número de equipos agregados (ASICs) por una conexión upstream, por defecto es *50*.
   
Routing
-------

 * **name**: frase: distingue mayúsculas y minúsculas con longitud mínima 1 (obligatorio), nombre del dominio de enrutamiento,
 * **from**: lista (obligatoria), lista de servidores que serán usados como proxies de agregación.
   
Routing Goal
------------

 * **name**: frase: distingue mayúsculas y minúsculas con longitud mínima 1 (obligatorio), nombre de la meta de enrutamiento,
 * **hr_weight:** entero (opcional), peso de la relación de preferencia de la distribución de la tasa de hash.
   
Routing Goal Level
------------------

 * **targets**: lista (obligatoria), lista de objetivos que son aplicados como puntos finales en el dominio de enrutamiento.

**************************
Configuración Acompañante
*************************

Otra configuración está predefinida en el archivo *docker-compose.yml* que es una aplicación esencial para correr Braiins Farm Proxy como una pila multi-contenedores Docker. Este archivo de configuración está diseñado de forma que requiera tan pocas ediciones como sea posible. Docker-compose consiste de la configuración de estos servicios:

 * **Prometheus**: corre en el puerto **9090**, puede accederse con su navegador, ej: ``http://<su-host>:9090/``
 * **Node Exporter**: corre en el puerto **9100**, puede accederse con su navegador, ej: ``http:/<su-host>:9100/``
 * **Grafana**: corre en el puerto **3000**, puede accederse con su navegador, ej: ``http://<su-host>:3000/``

Grafana es crucial para el monitoreo de la minería con Braiins Farm Proxy. Prometheus puede ser útil en caso de que el usuario quiera hacer sus propias gráficas para tablero Grafana. Node Exporter es un exportador de métricas de sistema operativo para la base de datos Prometheus.

.. attention::

   El archivo *docker-compose.yml* refiere a un archivo de configuración **sample.toml** en la configuración del contenedor farm-proxy. Si el operador de la granja tiene su propio archivo de configuración y quiere abordarlo al farm-proxy, sample.toml debería reemplazarse por ese archivo. Abajo puede ver una configuración de farm-proxy en el *docker-compose.yml.*


.. code-block:: shell

      farm-proxy:
      image: braiinssystems/farm-proxy:v1.0.0-rc4
      container_name: farm-proxy
      network_mode: "host"
      volumes:
      - "./config/sample.toml:/conf/farm_proxy.yml"
      environment:
      - CONF_PATH=/conf/farm_proxy.yml
      - RUST_LOG=debug
      - RUST_BACKTRACE=full
      restart: unless-stopped
      logging:
      driver: "json-file"
      options:
      max-size: "100m"
      max-file: "50"
      compress: "true"

