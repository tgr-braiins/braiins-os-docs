#####################################
升级，降级和卸载
#####################################

.. contents::
	:local:
	:depth: 2

.. _upgrade_bos:

****************
固件升级
****************

固件的升级，是在任何基于OpenWrt的系统内，使用标准机制来安装/升级软件包的过程。
请按以下的步骤来进行固件升级。

通过网络界面升级
=========================

固件会定期检查新的版本。如有新版本发布，顶部栏的右侧会出现一个蓝色的 **Upgrade**（升级）按钮。点击按钮，并确认开始升级。

此外，您也可以手动在目录“系统>软件”中点击升级列表(*Update lists*)按钮来更新版本库信息。如该按钮不存在，请尝试刷新页面。要启动升级过程，请将 ``firmware`` 命令输入到下载和安装包(Download and install package)项内，然后按OK。


通过SSH升级
===============

将矿机通过SSH连接之后，最新固件升级可以使用以下的命令启动：

::

  opkg update && opkg install firmware

因为安装固件导致重启，会出现以下的输出:

::

  ...
  Collected errors:
  * opkg_conf_load: Could not lock /var/lock/opkg.lock: Resource temporarily unavailable.
    Saving config files...
    Connection to 10.10.10.1 closed by remote host.
    Connection to 10.10.10.1 closed.

.. _upgrade_community_bos_plus:

**********************
Upgrade to Braiins OS+
**********************

为了从旧版或社区版本升级Braiins OS+, 请通过SSH连接矿机并运行以下命令：

::

    opkg update && opkg install bos_plus

.. _downgrade_bos_plus_community:

**************************************
升级/降级社区版本
**************************************

为了从旧版或社区版本升级Braiins OS+, 请通过SSH连接矿机并运行以下命令：
In order to upgrade from older version of Braiins OS or downgrade from Braiins OS+, connect to the miner via
SSH and use the following command (replace the placeholder ``IP_ADDRESS`` accordingly):

::

  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-03-29-0-6ec1a631_arm_cortex-a9_neon.tar && sysupgrade -F /tmp/firmware.tar'

.. _downgrade_bos_stock:

***********************************
回滚到之前的Braiins OS版本
***********************************

如果您想降级当前固件包到之前您替换矿机原厂固件时的Braiins OS版本，请通使用以下的命令：

 -  *IP set按钮* - ——按下*10秒*，直到红色LED闪烁。
 -  *SD card* - 编辑 *uEnv.txt* 文件，以包含 **factory_reset=yes** 行
 -  *矿机*——在矿机的命令行执行 ``miner factory_reset`` 命令（同时要保持通过SSH的连接）
 -  *opkg包* ——在矿机的命令行执行 ``opkg remove firmware`` 命令（同时要保持通过SSH的连接）
    
    ——在矿机的命令行执行 opkg remove firmware命令（同时要保持通过SSH的连接）

***************************
刷回原厂固件
***************************

用之前的备份刷回
===============================

默认情况下，在迁移到Braiins OS的过程中会自动创建一份原始固件的备份，并且可以按照以下的命令恢复它：(如果需要的话，替换占位符``BACKUP_ID_DATE`` 和 ``IP_ADDRESS`` ):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS

用原厂固件映像刷回
=============================

在蚂蚁矿机S9上，您也可以用矿机制造商的网站上提供的映像来刷回原厂固件， ``FACTORY_IMAGE`` 会作为 ``tar.gz``（未提取的！）的文件路径或URL。在 `platform.py <https://github.com/braiins/braiins-os/blob/master/upgrade/am1/platform.py>`__ 文件内，列出了所有支持的映像以及相应的MD5哈希值。


Run (replace the placeholders ``FACTORY_IMAGE`` and ``IP_ADDRESS`` accordingly):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS
