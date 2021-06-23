##############
Advanced Guide
##############

.. contents::
	:local:
	:depth: 1

.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

********
Road map
********

There are many tools, packages, and scripts that can be used to manage Braiins OS+. For better navigation, use the following tree:

 * Install Braiins OS+
 
  * Using BOS Toolbox (:ref:`bosbox_install`)
  * Using web package (:ref:`web_package_install`)
  * Using SD card (:ref:`sd_install`)
  * Using SD card and miner tool (:ref:`miner_nand_install`)
  * Using SSH scripts (:ref:`ssh_package_install`)

 * Unlock SSH on Antminer S9
 
  * Using BOS Toolbox (:ref:`bosbox_unlock`)  

 * Update Braiins OS+
 
  * Using BOS Toolbox (:ref:`bosbox_update`)
  * Using OPKG (:ref:`opkg_update`)
  * Using sysupgrade package (:ref:`sysupgrade_switch_braiinsplus`)
  * Using bos2bos script (:ref:`bos2bos`)
  
 * Switching to Braiins OS (version without autotuning)
 
  * Using sysupgrade package (:ref:`sysupgrade_switch_braiinsos`)
  * Using bos2bos script (:ref:`bos2bos`)
  
 * Switching to Braiins OS+ (version with autotuning)
 
  * Using OPKG (:ref:`opkg_switch_to_braiinsplus`)
  * Using sysupgrade package (:ref:`sysupgrade_switch_braiinsplus`)
  * Using bos2bos script (:ref:`bos2bos`)
  
 * Reset to initial Braiins OS version (version, which was installed for the first time on device) - factory reset
 
  * Using OPKG (:ref:`opkg_factory_reset`)
  * Using SD card (:ref:`sd_factory_reset`)
  * Using "miner" tool (:ref:`miner_factory_reset`)
  * Using bos2bos script (:ref:`bos2bos`)
  
 * Uninstall Braiins OS+
 
  * Using BOS Toolbox (:ref:`bosbox_uninstall`)
  * Using SSH scripts (:ref:`ssh_package_uninstall`)

 * Turn on/off Nightly feeds

  * Using "miner" tool (:ref:`miner_nightly`)

 * Turn on/off auto-upgrade

  * Using "miner" tool (:ref:`miner_autoupgrade`)

 * Run custom shell commands on the miner

  * Using BOS Toolbox (:ref:`bosbox_command`)

 * Scan the network to identify miners using BOS Toolbox

  * Using BOS Toolbox (:ref:`bosbox_scan`)

 * Listen for incoming broadcast from devices using BOS Toolbox
  
  * Using BOS Toolbox (:ref:`bosbox_listen`)

.. _bosbox:

************
BOS Toolbox
************

BOS Toolbox is a new tool that allows users to easily install, uninstall, update, detect, configure Braiins OS and run custom commands on the device. It also enables commands to be executed in batch mode, which makes the management of a larger number of devices easier. BOS Toolbox also automatically download the latest firmware. This is the recommended way to manage your machines.

=====
Usage
=====

  * Download the **BOS Toolbox** from our `website <https://braiins-os.com/>`_.
  * Create a new text file, change the ".txt" ending to ".csv" and insert the IP addresses on which you want execute the commands. Put that file in the directory where the BOS Toolbox is located. **Use only one IP address per line!**
  * Follow the sections bellow

=======================================
Features, PROs and CONs of this method:
=======================================

  + installs Braiins OS+ remotely and automatically unlocks SSH on Antminer S9 during the installation
  + updates Braiins OS+ remotely
  + uninstalls Braiins OS+ remotely
  + configures Braiins OS+ remotely
  + runs custom commands on machines
  + scans the network for machines
  + migrates the whole configuration by default (can be adjusted) when installing Braiins OS+
  + migrates the network configuration by default (can be adjusted) when uninstalling Braiins OS+
  + parameters are available to customize the process
  + turns on autotuning on default power limit when installing Braiins OS+
  + batch mode available to manage multiple devices at once
  + easy to use
  
  - does not work on X17 devices with locked SSH

.. _bosbox_install:

======================================
Install Braiins OS+ using BOS Toolbox
======================================

  * Download **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file, change the ".txt" ending to ".csv" and insert the IP addresses on which you want execute the commands. Put that file in the directory where the BOS Toolbox is located. **Use only one IP address per line!**
  * Once you have downloaded BOS Toolbox, run it in GUI mode by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Install** section, fill the **Miner(s)** option by selecting the created text file and press **Start**.

You can use the following **arguments** to adjust the process:

====================================  ====================================  ============================================================
GUI Options                           Command line arguments                Description
====================================  ====================================  ============================================================
Password                              -p PASSWORD, --password PASSWORD      Administration password
Farm-ID                               --bos-mgmt-id [BOS_MGMT_ID]	          Set Braiins OS+ Manager identifier
Set Power limit                       --psu-power-limit [PSU_POWER_LIMIT]   Set PSU power limit (in Watts)
Set Pool user                         --pool-user [POOL_USER]               Set the username and workername for Slush Pool
Latest Stable release                 *N/A* - Default Option                Selected by default, recommended option
Open-Source version                   --open-source         		            Use for installation of the Open-Source version (exclusive with **nightly** and **feed-url**)
Nightly version                       --nightly             		            Use for installation of the Nightly version (exclusive with **open-source** and **feed-url**)
Custom Feeds-URL                      --feeds-url [FEEDS_URL]		            Override default feeds server URL (exclusive with **open-source** and **nightly**)
Firmware version                      --fw-version [FW_VERSION]	            Select specific firmware version
No Auto-Upgrade                       --no-auto-upgrade                     Turn off Auto-Upgrade (not recommended)
Don't migrate pool settings           --no-keep-pools                       Do not keep miner pool configuration
Migrate network settings              *N/A* - Default Option                Keep miner network configuration, selected by default (recommended)
Don't migrate network settings        --no-keep-network                     Do not keep miner network configuration (use DHCP)
Don't keep hostname                   --no-keep-hostname                    Do not keep miner hostname and generate new one based on MAC
Keep hostname                         --keep-hostname                       Keep miner hostname
Post-upgrade                          --post-upgrade [POST_UPGRADE]         Path to directory with stage3.sh script
*N/A* - Not implemented               -h, --help                            show this help message and exit
*N/A* - Not implemented               --backup                              Backup miner before the installation (NAND and configuration)
*N/A* - Not implemented               --no-nand-backup                      Skip full NAND backup (config is still being backed up)
*N/A* - Not implemented               --no-wait                             Do not wait until system is fully upgraded and rebooted
*N/A* - Not implemented               --dry-run                             Do all upgrade steps without the actual upgrade
====================================  ====================================  ============================================================

**Command example:**

::

  bos-toolbox.bat install --psu-power-limit 1200 --password root listOfMiners.csv

This command will install Braiins OS+ on the miners, that are specified in the *listOfMiners.csv* file and set the power limit to 1200 on all of them. The command will also automatically unlock the Antminer S9.

.. _bosbox_update:

=====================================
Update Braiins OS+ using BOS Toolbox
=====================================

  * Download the **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file, change the ".txt" ending to ".csv" and insert the IP addresses on which you want execute the commands. Put that file in the directory where the BOS Toolbox is located. **Use only one IP address per line!**
  * Once you have downloaded BOS Toolbox, run it in GUI mode by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Update** section, fill the **Miner(s)** option by selecting the created text file and press **Start**.

You can use the following **arguments** to adjust the process:

====================================  ====================================  ============================================================
GUI Options                           Arguments                             Description
====================================  ====================================  ============================================================
Package                 				      PACKAGE_NAME                          Package name for update (select 'firmware' for firmware update)
Password                              -p PASSWORD, --password PASSWORD      Administration password
Farm-ID                               --bos-mgmt-id [BOS_MGMT_ID]           Set Braiins OS+ Manager identifier
Ignore non-responsive miners          -i, --ignore                          no halt on errors
*N/A* - Not implemented               --h, --help                           show this help message and exit
====================================  ====================================  ============================================================


**Command example:**

::

  bos-toolbox.bat update listOfMiners.csv firmware

This command will look for an update for the miners, that are specified in the *listOfMiners.csv* and update them if there is a new version of firmware.

.. _bosbox_uninstall:

========================================
Uninstall Braiins OS+ using BOS Toolbox
========================================

  * Download the **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file in your text editor and insert the IP addresses on which you want execute the commands. **Use only one IP address per line!** (Note that you can find the IP address in the Braiins OS+ web interface by going to *Status -> Overview*.) Then save the file in the same directory as you saved the BOS Toolbox and change the ".txt" ending to ".csv". 
  * Once you have downloaded BOS Toolbox, run it by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Uninstall** section, fill the **Miner(s)** option by selecting the created text file and press **Start**.

You can use the following **arguments** to adjust the process:

====================================  ====================================  ============================================================
GUI Options                           Arguments                             Description
====================================  ====================================  ============================================================
Password                              -p PASSWORD, --password PASSWORD      Administration password
Default stock firmware                *N/A* - Default Option                Default setting
Custom Feeds-URL                      --feeds-url [FEEDS_URL]		            Override default feeds server URL
*N/A* - Not implemented               --nand-restore			                  Use full NAND restore from previous backup
*N/A* - Not implemented               BACKUP_PATH                           Path to a directory or tgz file with data for miner restore
*N/A* - Not implemented               --h, --help                           Show this help message and exit
====================================  ====================================  ============================================================

**Command example:**

::

  bos-toolbox.bat uninstall listOfMiners.csv

This command will uninstall Braiins OS+ from the miners, that are specified in the *listOfMiners.csv* file and install a default stock firmware.

**Warning:** The stock firmware that's installed when you uninstall Braiins OS+ is not suitable for mining! Upgrade to a newer version of stock firmware for your specific hardware model before you start mining.

**Important note:** 
*BACKUP_PATH* is optional. Use only in conjuction with *--nand-restore* argument. Restoring firnware from backup **is not recommended**.

.. _bosbox_configure:

===========================================
Configure Braiins OS+ using BOS Toolbox
===========================================

  * Download the **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file in your text editor and insert the IP addresses on which you want execute the commands. **Use only one IP address per line!** (Note that you can find the IP address in the Braiins OS+ web interface by going to *Status -> Overview*.) Then save the file in the same directory as you saved the BOS Toolbox and change the ".txt" ending to ".csv". 
  * Once you have downloaded BOS Toolbox, run it by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Config** section, fill the **List of miners** option by selecting the created text file, select the action and press **Start**.

You **have to use one** of the following **actions** to adjust the process:

====================================  ============================================================
Arguments                             Description
====================================  ============================================================
load                                  load the current configuration of the miners (specified in 
                                      the CSV file) and insert them to the CSV file
save                                  save the settings from the CSV file to the miners 
                                      (this does not apply them)
apply                                 apply the settings, which were copied from the CSV file to 
                                      the miners
save_apply                            save and apply the settings from the CSV file to the miners
====================================  ============================================================

You can use the following **arguments** to adjust the process:

====================================  ====================================  ============================================================
GUI Options                           Arguments                             Description
====================================  ====================================  ============================================================
Username                              -u USER, --user USER                  Administration username
Password                              -p PASSWORD, --password PASSWORD      Administration password
Change Password                       --change-password		                  Allow password change (to one stated in the *listOfMiners.csv*)
Ignore                                -i, --ignore                          no halt on errors
*N/A* - Not implemented               -h, --help                            show this help message and exit
*N/A* - Not implemented               -c, --check                           dry run sans writes
====================================  ====================================  ============================================================

**Command example:**

::

  bos-toolbox.bat config --user root load listOfMiners.csv
  
  #edit the CSV file using a spreadsheet editor (e.g. Office Excel, LibreOffice Calc, etc.)
  
  bos-toolbox.bat config --user root -p admin --change-password save_apply listOfMiners.csv

The first command will load the configuration of the miners, that are specified in the *listOfMiners.csv* (using the login username *root*) and save it to the CSV file. You can now open the file and edit what you need. After the file was edited, the second command will copy the settings back to the miners, apply them and change the password to one in the password column.

.. _bosbox_scan:

======================================================
Scan the network to identify miners using BOS Toolbox
======================================================

  * Download the **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file in your text editor and insert the IP addresses on which you want execute the commands. **Use only one IP address per line!** (Note that you can find the IP address in the Braiins OS+ web interface by going to *Status -> Overview*.) Then save the file in the same directory as you saved the BOS Toolbox and change the ".txt" ending to ".csv". 
  * Once you have downloaded BOS Toolbox, run it by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Scan** section, select the IP range for scan and press **Start**.

You can use the following **arguments** to adjust the process:

====================================  ====================================  ============================================================
GUI Options                           Arguments                             Description
====================================  ====================================  ============================================================
Password                              -p PASSWORD, --password PASSWORD      Administration password
Save output                           -o OUTPUT, --output OUTPUT            Save list of found IP addresses to a file
Verbose                               -v, --verbose                         Report networking errors
*N/A* - Not implemented               -h, --help                            Show this help message and exit
*N/A* - Not implemented               -j JOBS, --jobs JOBS                  Number of concurrent jobs to scan network
====================================  ====================================  ============================================================

**Command Example:**

::

  #scan the network, in the range 10.10.10.0 - 10.10.10.255
  bos-toolbox.bat discover scan 10.10.10.0/24

  #scan the network, in the range 10.10.0.0 - 10.10.255.255
  bos-toolbox.bat discover scan 10.10.0.0/16

  #scan the network, in the range 10.0.0.0 - 10.255.255.255
  bos-toolbox.bat discover scan 10.0.0.0/8

.. _bosbox_listen:

============================================================
Listen for incoming broadcast from devices using BOS Toolbox
============================================================

  * Download the **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file in your text editor and insert the IP addresses on which you want execute the commands. **Use only one IP address per line!** (Note that you can find the IP address in the Braiins OS+ web interface by going to *Status -> Overview*.) Then save the file in the same directory as you saved the BOS Toolbox and change the ".txt" ending to ".csv". 
  * Once you have downloaded BOS Toolbox, run it by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Listen** section press **Start** to start listening for incoming broadcast from devices (when the IP report button is pressed).

You can use the following **arguments** to adjust the process:

====================================  ====================================  ============================================================
GUI Options                           Arguments                             Description
====================================  ====================================  ============================================================
Save output                           -o OUTPUT, --output OUTPUT            Save list of found IP addresses to a file
Format                                --format FORMAT                       change default formatting string for device information; the
                                                                            tags '{IP}' and '{MAC}' will be replaced with actual data
*N/A* - Not implemented               -h, --help                                                                  Show this help message and exit
====================================  ====================================  ============================================================

.. _bosbox_command:

================================================
Run custom commands on miners using BOS Toolbox
================================================

  * Download the **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file in your text editor and insert the IP addresses on which you want execute the commands. **Use only one IP address per line!** (Note that you can find the IP address in the Braiins OS+ web interface by going to *Status -> Overview*.) Then save the file in the same directory as you saved the BOS Toolbox and change the ".txt" ending to ".csv". 
  * Once you have downloaded BOS Toolbox, run it by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Command** section, fill the **Miner(s)** option by selecting the created text file, the command to run and press **Start**.

You can use the following **arguments** to adjust the process:

====================================  ====================================  ============================================================
GUI Options                           Arguments                             Description
====================================  ====================================  ============================================================
Show remote output                    -o, --output                          Capture and print remote output
Show hostname output                  -O, --output-hostname                 Capture and print remote output
Password                              -p PASSWORD, --password PASSWORD      Administration password
*N/A* - Not implemented               -h, --help                            show this help message and exit
*N/A* - Not implemented               -j JOBS, --jobs JOBS                  number of concurrent jobs
*N/A* - Not implemented               -a, --auto                            Use ssh if rpc is not available
*N/A* - Not implemented               -l, --legacy                          Use ssh
*N/A* - Not implemented               -L, --no-legacy                       Use rpc
====================================  ====================================  ============================================================

**Command example:**

::

  #stop BOSminer, effectively stopping mining and decreasing the power draw to minimum
  bos-toolbox.bat command -o list.csv stop

.. _bosbox_unlock:

============================================
Unlock SSH on Antminer S9 using BOS Toolbox
============================================

**Note:** The unlock functionality is a part of the installation process and is done automatically.
 
  * Download the **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file, change the ".txt" ending to ".csv" and insert the IP addresses on which you want execute the commands. Put that file in the directory where the BOS Toolbox is located. **Use only one IP address per line!**
  * Once you have downloaded BOS Toolbox, run it by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Unlock** section, fill the **Miner(s)** option by selecting the created text file and press **Start**.

You can use the following **arguments** to adjust the process:

====================================  ====================================  ============================================================
GUI Options                           Arguments                             Description
====================================  ====================================  ============================================================
Username                              -u USERNAME, --username USERNAME      username for webinterface
Password                              -p PASSWORD, --password PASSWORD      password for webinterface
*N/A* - Not implemented               --h, --help                           show this help message and exit
*N/A* - Not implemented               --port PORT                           port of antminer webinterface
*N/A* - Not implemented               --ssl                                 whether to use SSL
====================================  ====================================  ============================================================


**Command Example:**

::

  bos-toolbox.bat unlock -u root -p root listOfMiners.csv

This command will unlock SSH on the miners, that are specified in the *listOfMiners.csv*.

.. _web_package:

***********
Web Package
***********

The Web package can be used to switch from stock firmware, which was released before 2019. It should also work on other stock-based firmwares. This package cannot be used on stock firmware, released in 2019 and later, because of the signature verification, that was implemented. The signature verification prevents the usage of other than original stock firmwares.

=====
Usage
=====

  * Download the **Web Package** from our `website <https://braiins-os.com/>`_.
  * Follow the sections bellow

=======================================
Features, PROs and CONs of this method:
=======================================

  + replaces stock firmware with Braiins OS+ without additional tools
  + migrates the network configuration
  + migrates pool URLs, users and passwords
  + turns on autotuning on default power limit
  
  - cannot be used on stock firmware released in 2019 and later
  - cannot configure the installation (e.g. it will always migrate the network settings)
  - no batch-mode (unless you create your own scripts)

.. _web_package_install:

=====================================
Install Braiins OS+ using Web package
=====================================

  * Download the **Web Package** from our `website <https://braiins-os.com/>`_.
  * Log-in on your miner and go to the section *System -> Upgrade*.
  * Upload the downloaded package and flash the image.

.. _sd:

*************
SD card image
*************

If you are running stock firmware, which was released in 2019 and later, the only way to install Braiins OS+ is to insert an SD card with Braiins OS+ flashed on it. In 2019, the SSH connection was locked and the signature verification in the web interface prevents the usage of other than stock firmware usage.

=====
Usage
=====

  * Download the **SD card image** from our `website <https://braiins-os.com/>`_.
  * Follow the sections bellow

=======================================
Features, PROs and CONs of this method:
=======================================

  + replaces SSH locked stock firmware with Braiins OS+
  + uses the network configuration stored on the NAND (this can be turned off, see the section *Network settings* bellow)
  + turns on autotuning on default power limit
  
  - does not migrate pool URLs, users and passwords
  - no batch-mode

.. _sd_install:

=================================
Install Braiins OS+ using SD card
=================================

 * Download the SD card image from our `website <https://braiins-os.com/>`_.
 * Flash the downloaded image on an SD card (e.g. using `Etcher <https://etcher.io/>`_). *Note: Simple copy to SD card will not work. The SD card has to be flashed!*
 * **(Antminer S9 only)** Adjust the jumpers to boot from SD card (instead of NAND memory), as shown below.

  .. |pic1| image:: ../_static/s9-jumpers.png
      :width: 45%
      :alt: S9 Jumpers

  .. |pic2| image:: ../_static/s9-jumpers-board.png
      :width: 45%
      :alt: S9 Jumpers Board

  |pic1|  |pic2|

 * Insert the SD card into the device, then start the device.
 * If the auto-install version of SD image was used, the system will be automatically installed to the internal memory (NAND). The installation is completed, once both LEDs start to blink at the same time. After the installation completes, remove the SD card to boot Braiins OS+ from the NAND.
 * After a moment, you should be able to access the Braiins OS+ interface through the device’s IP address.

.. _sd_network:

================
Network settings
================
 
 By default, network configuration stored on the NAND is used, while running Braiins OS+ from an SD card. This feature can be turned off, by following the steps bellow:

  * Mount the first FAT partition of the SD card
  * Open the file uEnv.txt and insert the following string (make sure there is only one string per line)

  ::

    cfg_override=no

Disabling usage of old network settings is beneficial for the users, that have problems with the miner not being visible in the network (e.g. static IP address used on NAND is out of range of the network). By doing so, DHCP is used.

.. _sd_nand_install:

============
NAND install
============

The SD card can be used to replace the firmware running on NAND with Braiins OS+. This can be done either:
  * using the web interface - section *System -> Install current system to device (NAND)*
  * using the *miner* tool, via SSH - follow this section of the guide :ref:`miner_nand_install`
  * Add ``nand_install=yes`` to the ``uEnv.txt`` file for automatic NAND installation.

.. _sd_factory_reset:

=======================================
Braiins OS+ factory reset using SD card
=======================================

You can do a factory reset, by following the steps bellow:

  * Mount the first FAT partition of the SD card
  * Open the file uEnv.txt and insert the following string (make sure there is only one string per line)

  ::

    factory_reset=yes

.. _ssh_package:

****************************
Remote (SSH) install package
****************************

With the *Remote (SSH) install package* you can install or uninstall Braiins OS+. This method is not recommended, as it requires a Python setup. Use the BOS Toolbox instead.

=====
Usage
=====

  * Download the **Remote (SSH) install package** from our `website <https://braiins-os.com/>`_.
  * Follow the sections bellow

=======================================
Features, PROs and CONs of this method:
=======================================

  + installs Braiins OS+ remotely
  + uninstalls Braiins OS+ remotely
  + migrates the whole configuration by default (can be adjusted) when installing Braiins OS+
  + migrates the network configuration by default (can be adjusted) when uninstalling Braiins OS+
  + parameters are available to customize the process
  + turns on autotuning on default power limit when installing Braiins OS+
  
  - no batch-mode (unless you create your own scripts)
  - requires a long setup
  - does not work on miner with locked SSH

.. _ssh_package_environment:

=========================
Preparing the environment
=========================

First, you need to prepare the Python environment. This consists of the following steps:

* *(Only Windows)* Install *Ubuntu for Windows 10* available from the Microsoft Store `here. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Run the following commands in your command line terminal:

*(Note that the commands are compatible with Ubuntu and Ubuntu for Windows 10. If you are using a different distribution of Linux or a different OS, please check the corresponding documentation and edit the commands as necessary.)*

::

  #Update the repositories and install dependencies
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv
  
  #Download and extract the firmware package
  #Antminer S9
  wget -c https://feeds.braiins-os.com/20.10/braiins-os_am1-s9_ssh_2020-10-25-0-908ca41d-20.10-plus.tar.gz -O - | tar -xz
  
  #Antminer S17
  wget -c https://feeds.braiins-os.com/20.11/braiins-os_am2-s17_ssh_2020-11-27-0-5eb922d4-20.11-plus.tar.gz -O - | tar -xz

  #Change the directory to the unpacked firmware folder
  #Antminer S9
  cd ./braiins-os_am1-s9_ssh_VERSION
  
  #Antminer S17
  cd ./braiins-os_am2-s17_ssh_VERSION

  #Create a virtual environment and activate it
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate
  
  #Install the required Python packages
  python3 -m pip install -r requirements.txt

.. _ssh_package_install:

=====================================
Install Braiins OS+ using SSH package
=====================================

Installation of Braiins OS+ using the so-called *SSH Method* consists of the following steps:

* *(Custom Firmware)* Flash stock firmware. This step can be skipped if the device is running on stock firmware or on a previous versions of Braiins OS. *(Note: It is possible, that Braiins OS+ can be installed directly over a custom firmware, but as they differ from the stock version, it might be necessary to flash stock firmware first.)*
* *(Only Windows)* Install *Ubuntu for Windows 10* available from the Microsoft Store `here. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Prepare the Python environment, which is described in the section :ref:`ssh_package_environment`.
* Run the following commands in your command line terminal (replace the placeholder ``IP_ADDRESS`` accordingly) :

*(Note that the commands are compatible with Ubuntu and Ubuntu for Windows 10. If you are using a different distribution of Linux or a different OS, please check the corresponding documentation and edit the commands as necessary.)*

::

    #Change the directory to the unpacked firmware folder (if not already in the firmware folder)
  #Antminer S9
  cd ./braiins-os_am1-s9_ssh_VERSION
  
  #Antminer S17
  cd ./braiins-os_am2-s17_ssh_VERSION

  #Activate the virtual environment (if it is not already activated)
  source .env/bin/activate
  
  #Run the script to install Braiins OS+
  python3 upgrade2bos.py IP_ADDRESS

**Note:** *for more information about the arguments that can be used, use the* **--help** *argument.*

.. _ssh_package_uninstall:

=======================================
Uninstall Braiins OS+ using SSH package
=======================================

.. _ssh_package_uninstall_image:

Using factory firmware image
=============================

First, you need to prepare the Python environment, which is described in the section :ref:`ssh_package_environment`.

On an Antminer  you can flash a factory firmware image
from the manufacturer’s website, with ``FACTORY_IMAGE`` being file path
or URL to the ``tar.gz`` (not extracted!) file. Supported images with
corresponding MD5 hashes are listed in the
`platform.py <https://github.com/braiins/braiins/blob/master/braiins-os/upgrade/am1/platform.py>`__
file.

Run (replace the placeholders ``FACTORY_IMAGE`` and ``IP_ADDRESS`` accordingly):

::

  #Antminer S9
  cd ~/braiins-os_am1-s9_ssh_2020-09-07-1-463cb8d0-20.09-plus && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS
  
  #Antminer S17
  cd ~/braiins-os_am2-s17_ssh_2020-11-27-0-5eb922d4-20.11-plus && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS

**Note:** *for more information about the arguments that can be used, use the* **--help** *argument.*

.. _ssh_package_uninstall_backup:

Using previously created backup
===============================

First, you need to prepare the Python environment, which is described in the section :ref:`ssh_package_environment`.

If you created a backup of the original firmware during the installation of Braiins OS+, you can restore it by using the following commands (replace the placeholders ``BACKUP_ID_DATE`` and ``IP_ADDRESS`` accordingly):

::

  #Antminer S9
  cd ~/braiins-os_am1-s9_ssh_2020-09-07-1-463cb8d0-20.09-plus && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS
  
  #Antminer S17
  cd ~/braiins-os_am2-s17_ssh_2020-11-27-0-5eb922d4-20.11-plus && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS

**Note: This method is not recommended as the backup creation is very finicky. The backup can be corrupted and there is no way to check it. Use at your own risk and make sure, you can access the miner and insert an SD card to it in case the restoration does not finish successfully!**

.. _opkg:

****
OPKG
****

OPKG commands can be used after connecting to the miner via SSH. There are many OPKG commands, but regarding Braiins OS+, you need to use only the following:

  * *opkg update* - updates the package lists. It's recommended to use this command before other OPKG commands.
  * *opkg install PACKAGE_NAME* install the defined package. It's recommended to use *opkg update* to update the package lists before installing packages.
  * *opkg remove PACKAGE_NAME*

Since the firmware change results in a reboot, the following
output is expected:

::

  ...
  Collected errors:
  * opkg_conf_load: Could not lock /var/lock/opkg.lock: Resource temporarily unavailable.
    Saving config files...
    Connection to 10.10.10.1 closed by remote host.
    Connection to 10.10.10.1 closed.

=======================================
Features, PROs and CONs of this method:
=======================================

  + update Braiins OS+ remotely
  + switch to Braiins OS+ from other versions remotely
  + revert to the initial version of Braiins OS remotely
  + migrates the configuration and continue to mine without a need to configure anything (when updating or switching to Braiins OS+)
  
  - no batch-mode (unless you create your own scripts)

.. _opkg_update:

=============================
Update Braiins OS+ using OPKG
=============================

With OPKG you can easily update your current installation of Braiins OS+, by connecting to the miner via SSH and using the following commands:

::

  opkg update
  opkg install firmware

  #you can also connect to the miner and run the commands at the same time
  ssh root@IP_ADDRESS "opkg update && opkg install firmware"

This will migrate the configuration and continue to mine without a need to configure anything.

.. _opkg_switch_to_braiinsplus:

====================================================
Switch to Braiins OS+ from other versions using OPKG
====================================================

With OPKG you can easily switch to Braiins OS+, by connecting to the miner via SSH and using the following commands:

::

  opkg update
  opkg install bos_plus

  #you can also connect to the miner and run the commands at the same time
  ssh root@IP_ADDRESS "opkg update && opkg install bos_plus"

This will migrate the configuration and continue to mine without a need to configure anything. Default power limit will be set.

.. _opkg_factory_reset:

====================================
Braiins OS+ factory reset using OPKG
====================================

With OPKG you can easily revert to the initial version of Braiins OS (the version, which was installed for the first time on that device), by connecting to the miner via SSH and using the following commands:

::

  opkg update
  opkg remove firmware

  #you can also connect to the miner and run the commands at the same time
  ssh root@IP_ADDRESS "opkg update && opkg remove firmware"

This will reset the configuration to the state after the first Braiins OS installation.

.. _sysupgrade:

******************
Sysupgrade package
******************

Sysupgrade is used to upgrade the system running on the device. With this method, you can install various versions of Braiins OS or create a backup of the system. Installation of a firmware using *Braiins OS web interface* or using *opkg install firmware* uses this method. It's recommended to use the *Braiins OS web interface* or *opkg install firmware* instead of this method.

=====
Usage
=====

In order to use sysupgrade, you need to connect to the miner via SSH. The syntax is the following:

::

  sysupgrade [parameters] <image file or URL>

The most important parameters are **--help** (to display the help) and **-F** to force the installation. It's not recommended to use this method (besides the way, it is described bellow), unless you really know, what you are doing.

=======================================
Features, PROs and CONs of this method:
=======================================

  + installs various version of Braiins OS, while connected to the miner
  + migrates the configuration
  + parameters are available to customize the process
  
  - no batch-mode (unless you create your own scripts)
  - cannot switch to an older version of Braiins OS (released before 2020)

.. _sysupgrade_switch_braiinsos:

==============================================================================
Switch to Braiins OS (without autotuning) from other versions using Sysupgrade
==============================================================================

In order to upgrade from older version of Braiins OS or downgrade from Braiins OS+, use the following command (replace the placeholder ``IP_ADDRESS`` accordingly):

::

  #Antminer S9
  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-09-07-0-e50f2a1b-20.09_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'
  
  #Antminer S17
  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am2-s17/firmware_2020-09-07-0-e50f2a1b-20.09_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

This command contains the following commands: 

  * **ssh** - to connect to the miner
  * **wget** - used for downloading files, in this case the firmware package
  * **sysupgrade** - to actually flash the downloaded firmware package

.. _sysupgrade_switch_braiinsplus:

==========================================================
Switch to Braiins OS+ from other versions using Sysupgrade
==========================================================

In order to upgrade from older version of Braiins OS, use the following command (replace the placeholder ``IP_ADDRESS`` accordingly):

::

  #Antminer S9
  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.com/am1-s9/firmware_2020-09-07-1-463cb8d0-20.09-plus_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'
  
  #Antminer S17
  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.com/am2-s17/firmware_2020-11-27-0-5eb922d4-20.11-plus_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'
  
This command contains the following commands: 

  * **ssh** - to connect to the miner
  * **wget** - used for downloading files, in this case the firmware package
  * **sysupgrade** - to actually flash the downloaded firmware package

Note: It's recommended to use the *BOS Toolbox*, *Braiins OS web interface* or *opkg install bos_plus* instead of this method.

.. _bos2bos:

**************
Bos2Bos script
**************

**Bos2Bos script is not recommended to use, unless you experience problems with the installation using the other methods.** This method works, only if Braiins OS is already running on the device.

=======================================
Features, PROs and CONs of this method:
=======================================

  + installs any version of Braiins OS remotely
  + install a clean version of Braiins OS
  + parameters are available to customize the process
  
  - no batch-mode (unless you create your own scripts)

=====
Usage
=====

Usage of the Bos2Bos script requires the following setup:

* *(Only Windows)* Install *Ubuntu for Windows 10* available from the Microsoft Store `here. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Run the following commands in your command line terminal:

*(Note that the commands are compatible with Ubuntu and Ubuntu for Windows 10. If you are using a different distribution of Linux or a different OS, please check the corresponding documentation and edit the commands as necessary.)*

::
  
  #Update the repositories and install dependencies
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv
  
  # clone repository
  git clone https://github.com/braiins/braiins-os.git
  
  #change the directory
  cd ./braiins-os/braiins-os/

  #Create a virtual environment and activate it
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate
  
  #Install the required Python packages
  python3 -m pip install -r requirements.txt

After you succesfully finish the setup, you can use the following commands:

::

  #activate the virtual environment
  source .env/bin/activate

  #basic usage is the following
  python3 bos2bos.py FIRMWARE_URL IP_ADDRESS

  #the description of all available parameters can be displayed using the following command
  python3 bos2bos.py -h

**********
Miner tool
**********

.. _miner_nand_install:

=======================================
SD to NAND install using the Miner tool
=======================================

The SD card can be used to replace the firmware running on NAND with Braiins OS+. This can be done by connecting to the miner via SSH and usage of the following command:

  ::

    miner nand_install


.. _miner_factory_reset:

==============================================
Braiins OS+ factory reset using the Miner tool
==============================================

Factory reset can also be done using the *Miner tool*. Use the following command to do so:

  ::

    miner factory_reset

.. _miner_detect:

========================================
Detect device with LEDs using Miner tool
========================================

You can find a device by turning on LED blinking, using the *Miner tool*. Use the following command to do so:

  ::

    #turn on LED blinking
    miner fault_light on

    #turn off LED blinking
    miner fault_light off

.. _miner_nightly:

==============================================
Turn on/off Nightly feeds using the Miner tool
==============================================

You can turn on Nightly feeds to get updated to the latest nightly builds. These builds aim to fix crucial issues as fast as possible and, because of that, they are not tested as thoroughly as major releases before being published. Use these builds with caution and only if it solves your issues. In order to turn on/off the nightly feeds, use the following command:

  ::

    #turn on nightly feeds
    miner nightly_feeds on

    #turn off nightly feeds
    miner nightly_feeds off

.. _miner_autoupgrade:

=============================================
Turn on/off auto-upgrade using the Miner tool
=============================================

You can turn on the auto-upgrade feature, which will automatically upgrade the system to the latest version. This feature is **turned on** by default after transitioning from a **stock** firmware and **turned off** by default after upgrading from older versions of **Braiins OS** or **Braiins OS+**. In order to manually turn on/off auto-upgrade, use the following command:

  ::

    #turn on auto-upgrade
    miner auto_upgrade on

    #turn off auto-upgrade
    miner auto_upgrade off
