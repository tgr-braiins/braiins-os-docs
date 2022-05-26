
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#####
设置
#####

.. toctree::
   :maxdepth: 3
   :glob:

   *

************
安装
************

Braiins矿场代理具有自己的公开Github `资料库 <https://github.com/braiins/farm-proxy>`_ 基本上是4个Docker容器的堆栈。您可以通过想使用硬件的Linux终端来安装代理。

在开始的时候，需要安装几个先决条件：

**Linux**

 * 按照 `docker installation instructions <https://docs.docker.com/engine/install/ubuntu/>`_ 安装
 * 按照 `optional docker post installation steps <https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user>`_ 安装
 * 按照 `docker-compose installation <https://docs.docker.com/compose/install/>`_ 安装

**RPi**

  * 按照任何指南针安装，例如： https://jfrog.com/connect/post/install-docker-compose-on-raspberry-pi/

**验证已安装的先决条件**

::

      docker version
      docker-compose --version
      git --version

**下载Braiins矿场代理资料库**

::

      sudo apt update
      sudo apt install git
      git clone https://github.com/braiins/farm-proxy.git

Docker堆栈包含以下的容器:
 * *farm-proxy*: 容器包含Braiins矿场代理二进制文件,
 * *grafana*: 容器包含Grafana应用,
 * *nodeexporter*: 容器包含用于Prometheus数据库的硬件和操作系统数据的输出器,
 * *prometheus*: 容器包含Prometheus数据库.

Braiins矿场代理的独立二进制文件可以从公共的Github `下载 <https://github.com/braiins/farm-proxy/releases>`_.

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/AXfyDhbx4WY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

*****
启动
*****

矿场运营商设置使用Braiins矿场代理的矿场后，代理就可以开始了（配置本身在下面的文字中详细描述）。在Linux终端运行命令 ``docker-compose up -d``。要看所有的Docker容器是否在运行，点击命令 ``docker ps`` 来列出并检查容器的状态。

*******
重启
*******

如果需要重启Braiins矿场代理，运行 ``docker restart farm-proxy`` 的命令。该命令仅重启矿场代理的容器。在对TOML配置文件进行任何变化的情况下，矿场代理需要重启。如果想重启Dockers容器，也可以用类似的方法，将 "farm-proxy "替换为需要重启的容器的名称。

****
停止
****

有时，矿场运营商可能想停止Braiins矿场代理。这可以在Linux终端用 ``docker stop farm-proxy`` 命令完成。如果对 *docker-compose.yml* 文件进行有任何变化，就需要这样做。为恢复运行代理，请运行 ``docker-compose up -d farm-proxy`` 的命令。如果想停止另一个Docker容器，也可以这样做。

*******
升级
*******

建议关注Braiin矿场代理的Github资源库，如果有版本更新，可以收到通知。为升级到新的版本，只需在Linux终端运行 ``git pull origin master`` 的命令。如果您对配置文件中进行了任何变化，您可能想在升级前保存修改过的文件或者用 ``git stash`` 命令藏起来它们。

*********
卸载
*********

为从您的系统中彻底删除矿场代理和开箱即用的监控，按照以下的步骤操作：

1. 停止矿场代理和开箱即用的监控：``docker-compose down``,
2. 移除容器：``docker container prune``,
3. 删除映像: ``docker image prune -a``,
4. 删除已存储的监控数据：``docker volume prune``.

