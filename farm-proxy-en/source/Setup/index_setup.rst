
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

************
Installation
************

Braiins Farm Proxy has its own public Github `repository <https://github.com/braiins/farm-proxy>`_ and it is basically a stack of 4 Docker containers. You can easily install it with the Linux terminal of the hardware which will host the proxy.

At the beginning it is required to install a couple of prerequisites:

**Linux**

 * Follow `docker installation instructions <https://docs.docker.com/engine/install/ubuntu/>`_
 * Follow `optional docker post installation steps <https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user>`_
 * Follow `docker-compose installation <https://docs.docker.com/compose/install/>`_

**RPi**

  * Follow one of the many manuals - e.g. https://jfrog.com/connect/post/install-docker-compose-on-raspberry-pi/

**Verify Prerequisites Installed**

::

      docker version
      docker-compose --version
      git --version

**Download Braiins Farm Proxy Repository**

::

      sudo apt update
      sudo apt install git
      git clone https://github.com/braiins/farm-proxy.git

Docker stack consists of following containers:
 * *farm-proxy*: container with the Braiins Farm Proxy binary,
 * *bos_scanner*: container with the ssh scanner, which scans for miners with Braiins OS+ firmware on a defined network for the purpose of preparing data for the Grafana dashboard **Farm Dashboard**,
 * *grafana*: container with Grafana application,
 * *prometheus*: container with the Prometheus database.

A standalone binary of Braiins Farm Proxy can be downloaded from the public Github `here <https://github.com/braiins/farm-proxy/releases>`_.

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/AXfyDhbx4WY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

*****
Start
*****

When the mining operation with usage of Braiins Farm Proxy is configured by the farm operator, the proxy can get started (configuration itself is described in detail in the following text). Run the command in the Linux terminal ``docker-compose up -d``.To see if all the Docker containers are running, hit command ``docker ps`` to list them and check their status.

*******
Restart
*******

In case Braiins Farm Proxy needs a restart, run the command ``docker restart farm-proxy``. The command will restart only the farm-proxy container. Restart of farm-proxy is needed in case of any changes in TOML configuration file. If you wish to restart another Docker container it can be done similarly by replacing “farm-proxy” with the name of the container that needs a restart.

****
Stop
****

Sometimes the mining operator may want to stop Braiins Farm Proxy. It can be done in Linux terminal with the command ``docker stop farm-proxy``. It is needed in case of any changes in the file *docker-compose.yml*. To run proxy again, run the command ``docker-compose up -d farm-proxy``. If you wish to stop another Docker container it can be done similarly.

*******
Upgrade
*******

Braiins  recommends miners to monitor the Braiins Farm Proxy Github repository to be  notified if a new version is available. To upgrade to a newer version just run the command ``git pull origin master`` in the Linux terminal. If you made any changes in the configuration files you might want to save the modified files or stash them with command ``git stash`` before the upgrade. If you made any changes in the docker-compose.yml file, you will need to make them again after the upgrade since the ``git pull`` pulls the docker-compose.yml from the Github repository.

*********
Uninstall
*********

To completely remove farm-proxy and out-of-the-box monitoring from your system you need to perform the following steps:

1. Stop farm-proxy & out-of-the-box monitoring: ``docker-compose down``,
2. Remove containers: ``docker container prune``,
3. Remove images: ``docker image prune -a``,
4. Remove stored monitoring data: ``docker volume prune``,
5. Remove the folder: ``rm -rf farm-proxy/``.
