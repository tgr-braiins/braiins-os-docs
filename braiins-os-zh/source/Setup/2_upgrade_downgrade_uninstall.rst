#####################################
Upgrade, Downgrade and Uninstallation
#####################################

.. contents::
	:local:
	:depth: 2

.. _upgrade_bos:

****************
Firmware Upgrade
****************

The firmware upgrade process uses a standard mechanism for
installing/upgrading software packages within any OpenWrt based system.
Follow the steps below to perform the firmware upgrade.

Upgrade via web interface
=========================

The firmware periodically checks for availability of a new version and automatically updates the system. In
case the auto-update feature is disabled, a blue **Upgrade** button appears on
the right side of the top bar. Proceed to click on the button and
confirm to start the upgrade.

Alternatively, you can update the repository information manually by
clicking the *Update lists* button in the System > Software menu. In
case the button is missing, try to refresh the page. To trigger the
upgrade process, type ``firmware`` into the *Download and install
package* field and press *OK*.

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
migration to Braiins OS and can be restored using the following commands (replace the placeholders ``BACKUP_ID_DATE`` and ``IP_ADDRESS`` accordingly):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-0-6ec1a631 && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS

Using factory firmware image
=============================

On an Antminer S9, you can alternatively flash a factory firmware image
from the manufacturer’s website, with ``FACTORY_IMAGE`` being file path
or URL to the ``tar.gz`` (not extracted!) file. Supported images with
corresponding MD5 hashes are listed in the
`platform.py <https://github.com/braiins/braiins/blob/master/braiins-os/upgrade/am1/platform.py>`__
file.

Run (replace the placeholders ``FACTORY_IMAGE`` and ``IP_ADDRESS`` accordingly):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-0-6ec1a631 && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS
