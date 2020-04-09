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


Upgrade via SSH
===============

After connecting to the miner via SSH, the upgrade to the latest firmware can be triggered using the following commands:

::

  opkg update && opkg install firmware

Since the firmware installation results in a reboot, the following
output is expected:

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

In order to upgrade from older version or the Community Edition to Braiins OS+, connect to the miner via SSH
and use the following commands:

::

    opkg update && opkg install bos_plus

.. _downgrade_bos_plus_community:

**************************************
Upgrade/Downgrade to Community Edition
**************************************

In order to upgrade from older version of Braiins OS or downgrade from Braiins OS+, connect to the miner via
SSH and use the following command (replace the placeholder ``IP_ADDRESS`` accordingly):

::

  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-03-29-0-6ec1a631_arm_cortex-a9_neon.tar && sysupgrade -F /tmp/firmware.tar'

.. _downgrade_bos_stock:

***********************************
Reset to initial Braiins OS version
***********************************

The current firmware package can be downgraded to the version which was initially installed when
replacing the stock firmware. This can be done using the

 -  *IP SET button* - hold it for *10s* until red LED flashes
 -  *SD card* - edit the *uEnv.txt* file so it contains the line **factory_reset=yes**
 -  *miner utility* - call ``miner factory_reset`` from the miner’s
    command line (while connected via SSH)
 -  *opkg package* - call ``opkg remove firmware`` from the miner’s
    command line (while connected via SSH)

***************************
Flashing a factory firmware
***************************

Using previously created backup
===============================

By default, a backup of the original firmware is created during the
migration to Braiins OS+ and can be restored using the following commands (replace the placeholders ``BACKUP_ID_DATE`` and ``IP_ADDRESS`` accordingly):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS

Using factory firmware image
=============================

On an Antminer S9, you can alternatively flash a factory firmware image
from the manufacturer’s website, with ``FACTORY_IMAGE`` being file path
or URL to the ``tar.gz`` (not extracted!) file. Supported images with
corresponding MD5 hashes are listed in the
`platform.py <https://github.com/braiins/braiins-os/blob/master/upgrade/am1/platform.py>`__
file.

Run (replace the placeholders ``FACTORY_IMAGE`` and ``IP_ADDRESS`` accordingly):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS
