
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#####
Setup
#####

.. toctree::
   :maxdepth: 3
   :glob:

   *

***********
Instalación
***********

Braiins Farm Proxy tiene su propio `repositorio <https://github.com/braiins/farm-proxy>`_ Github público y es básicamente una pila de 4 contenedores Docker. Puede instalar fácilmente con la terminal Linux del equipo que alojará al proxy.

Al principio se requiere instalar un par de requisitos:

**Linux**

 * Siga las `instrucciones de instalación docker <https://docs.docker.com/engine/install/ubuntu/>`_
 * Siga los `pasos opcionales post instalación docker <https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user>`_
 * Siga la `instalación docker-compose <https://docs.docker.com/compose/install/>`_

**RPi**

  * Siga cualquiera de los muchos manuales - ej: https://jfrog.com/connect/post/install-docker-compose-on-raspberry-pi/

**Verifique los requisitos Instalados**

::

      docker version
      docker-compose --version
      git --version

**Descargue el Repositorio Braiins Farm Proxy**

::

      sudo apt update
      sudo apt install git
      git clone https://github.com/braiins/farm-proxy.git

La pila Docker consiste de los siguientes contenedores:
 * *farm-proxy*: contenedor con el binario Braiins Farm Proxy,
 * *bos_scanner*: contenedor con el escáner ssh, que escanea en busca de mineros con firmware Braiins OS+ en una red definida con el fin de preparar los datos para el tablero de Grafana **Farm Dashboard**,
 * *grafana*: contenedor con la aplicación Grafana,
 * *prometheus*: contenedor con la base de datos Prometheus.

Un binario independiente del Braiins Farm Proxy puede descargarse desde el Github público `aquí <https://github.com/braiins/farm-proxy/releases>`_.

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/AXfyDhbx4WY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

*******
Iniciar
*******

Cuando la operación minera con el uso de Braiins Farm Proxy se configura por el operador de la granja, el proxy puede iniciarse (la configuración misma es descrita en detalle en el texto siguiente). Corra el comando en la terminal Linux ``docker-compose up -d``.Para ver si todos los contenedores Docker están corriendo, presione el comando ``docker ps`` para listarlos y revisar sus estados.

*********
Reiniciar
*********

En caso de que Braiins Farm Proxy necesite un reinicio, corra el comando ``docker restart farm-proxy``. El comando reiniciará solo el contenedor de farm-proxy. El reinicio de farm-proxy es necesario en caso de cualquier cambio al archivo de configuración TOML. Si desea reiniciar otro contenedor Docker puede hacerse de manera similar reemplazando “farm-proxy” con el nombre del contenedor que necesite reiniciar.

*******
Detener
*******

A veces el operador minero podría desear detener Braiins Farm Proxy. Puede hacerse en la terminal Linux con el comando ``docker stop farm-proxy``. Es necesario en caso de cambios en el archivo *docker-compose.yml*. Para correr el proxy de nuevo, corra el comando ``docker-compose up -d farm-proxy``. Si desea detener otro contenedor Docker puede hacerlo de manera similar.

**********
Actualizar
**********

Es recomendado seguir el repositorio Github Braiins Farm Proxy y recibir la notificación si una versión nueva está disponible. Para actualizar a una nueva versión solo corra el comando ``git pull origin master`` en la terminal Linux. Si hizo algún cambio a los archivos de configuración podría querer guardar los archivos modificados o esconderlos con el comando ``git stash`` antes de actualizar.

***********
Desinstalar
***********

Para remover completamente farm-proxy y la supervisión inmediata de su sistema necesita hacer los siguientes pasos:

1. Detener farm-proxy y supervisión inmediata: ``docker-compose down``,
2. Remover contenedores: ``docker container prune``,
3. Remover imágenes: ``docker image prune -a``,
4. Remover datos de vigilancia almacenados: ``docker volume prune``.
