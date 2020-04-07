############
Installation
############

.. contents::
	:local:
	:depth: 1

***************
Getting Started
***************

This document is a quick-start guide on how to install Braiins OS on your mining device. There are two ways to test and use Braiins OS:

  1. **Boot from SD card** with Braiins OS image, effectively keeping the stock firmware in the built-in flash memory. In case you encounter
     any issues, you can simply boot the stock firmware from the internal memory. This is a safe method we suggest to start with.

  2. **Permanently re-flash the stock firmware**, effectively replacing the manufacturer’s firmware completely with Braiins OS. In this method,
     the only way to go back to the default stock setup is to restore the manufacturer’s firmware from a backup that you create during install
     or by flashing a factory firmware.

Due to the aforementioned reasons, it is highly recommended to install Braiins OS firmware **only on devices with SD card slots**.

In order to start mining using Braiins OS and BOSminer you will need to:

 * have a supported ASIC miner
 * get the latest version of Braiins OS
 * install Braiins OS
 * configure Braiins OS and start mining

*Note: Commands used in this manual are for instructional purposes. You might need to adjust file paths and names appropriately.*

**************************
Installation/Upgrade Guide
**************************

For better navigation among different installation/upgrade paths, use the following guide:

 * **Stock -> Braiins OS+ (latest version)** - Follow the guide in the sections :ref:`sd_card_method` or :ref:`remote_ssh_method` below
 * **Braiins OS (older versions) -> Braiins OS+ (latest version)** - Follow this section of the upgrade guide :ref:`upgrade_community_bos_plus`
 * **Braiins OS (older versions) -> Braiins OS Community Edition (latest version)** Follow this section of the upgrade guide :ref:`downgrade_bos_plus_community`
 * **Braiins OS Community Edition (latest version) -> Braiins OS+ (latest version)** Follow this section of the upgrade guide :ref:`upgrade_community_bos_plus`
 * **Braiins OS+ (latest version) -> Braiins OS Community Edition (latest version)** Follow this section of the upgrade guide :ref:`downgrade_bos_plus_community`
 * **Braiins OS+ -> Stock** - Follow the this section of the upgrade guide :ref:`downgrade_bos_stock`

.. _sd_card_method:

**************
SD card Method
**************

 * Download the latest release of transitional firmware image from our `website <https://braiins-os.com/>`_.
   You can verify the signatures using the public key,
   which is `available here. <https://slushpool.com/media/download/braiins-os.gpg.pub>`_
 * Flash the downloaded image on an SD card (e.g. using `Etcher <https://etcher.io/>`_).
 * Adjust the jumpers to boot from SD card (instead of NAND memory), as shown below.

	.. |pic1| image:: ./s9-jumpers.png
	    :width: 45%
	    :alt: S9 Jumpers

	.. |pic2| image:: ./s9-jumpers-board.png
	    :width: 45%
	    :alt: S9 Jumpers Board

	|pic1|  |pic2|

 * Insert the SD card into the device, then start the device.
 * After a moment, you should be able to access the Braiins OS interface through the device’s IP address.

**Using single SD card on multiple device**

The most recently used MAC address is stored on the SD card overlay
partition to check if the SD has been inserted into the same device. If the
current MAC address differs from the previous one, then the network and
system configuration is reset to its default and ``/etc/miner_hwid`` is
deleted.

HW_ID is determined from NAND if it stores Braiins OS firmware. If NAND is corrupted
or it contains stock firmware, then the file ``/etc/miner_hwid`` is used
if it exists, otherwise a new HW_ID is generated and stored to
``/etc/miner_hwid`` to preserve HW_ID until the next boot.

Flash Braiins OS from SD card to the internal memory (NAND)
============================================================

It is also possible to install Braiins OS on the internal memory (NAND) while running the firmware from the SD card.
In order to permanently flash Braiins OS on the NAND, connect to the miner via SSH and use the following command:

::

  miner nand_install

.. _remote_ssh_method:

*******************
Remote (SSH) Method
*******************

Installation of Braiins OS using the so-called *SSH Method* consists of the following steps:

 * *(Custom Firmware)* Flash stock firmware (this step can be skipped if the device is running on stock firmware or on a previous versions of Braiins OS).
 * *(Only Windows)* Install *Ubuntu for Windows 10* available from the Microsoft Store `here. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
 * Run the following commands in your command line terminal (replace the placeholders ``IP_ADDRESS`` accordingly) :

*(Note that the commands are compatible with Ubuntu and Ubuntu for Windows 10. If you are using a different distribution of Linux or a different OS, please check the corresponding documentation and edit the commands as necessary.)*

::

  # Prepare the enviroment and download the firmware (this step can be skipped if it was already done before)
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv
  wget -c https://feeds.braiins-os.org/20.03/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-0-6ec1a631.tar.gz -O - | tar -xz && cd ./braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-0-6ec1a631
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate && python3 -m pip install -r requirements.txt && deactivate
  
  # Install Braiins OS on the device
  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-0-6ec1a631 && source .env/bin/activate
  python3 upgrade2bos.py IP_ADDRESS

*************************************
Installing/Upgrading multiple devices
*************************************

In case when you need to perform installation or upgrade on multiple devices, you can use
our configuration spreadsheet that will will generate commands for different use cases.

The spreadsheet is available `here <https://docs.google.com/spreadsheets/d/1H3Zn1zSm6-6atWTzcU0aO63zvFzANgc8mcOFtRaw42E>`_
