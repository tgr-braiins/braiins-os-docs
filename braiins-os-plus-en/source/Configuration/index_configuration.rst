
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>


.. _configuration:

#############
Configuration
#############

.. contents::
  :local:
  :depth: 2

****************************************
Configure Braiins OS+ using BOS Toolbox
****************************************

You can easily configure Braiins OS+ on multiple devices using the **BOS Toolbox**. In order to do so, follow the steps in the section :ref:`bosbox_configure`.

*****************
BTC Tools Support
*****************

  * Braiins OS+ is supported by BTC Tools - batch management tool for miners. New versions of Braiins OS+ are supported - upgrade via Toolbox if you are using versions before 20.11. S9 and as well as x17 miners with Braiins OS+ are supported. BTC Tools for Windows/Linux can be downloaded here `here <https://btccom.zendesk.com/hc/en-us/articles/360020105012>`_. On the same page, documenation on how to use BTC Tools is available.

  * With exception of the below, Braiins OS+ supports all features of BTC Tools.

  * Limitations of BTC Tools when using with Braiins OS+:

    * Settings in Overclock/Underclock section does not affect Braiins OS+
    * LPM checkbox works for S9 only and enables/disables asicboost. However, pools that support mining.configure and version rolling are still required.
    * Enhanced LPM will turn autotuning on and set miner's power limit to 2/3 of the default power limit for the miner
    * Disabling Enhanced LPM keeps autotuning on its last state and sets miner's power limit to the default power limit for the miner (hardware specific)
    * Note: both LPM and Enhanced LPM options are used only when "Power Control" is checked. Otherwise, machine specific settings is kept.
    * Autotuning cannot be currently turned off with BTC Tools and power limit cannot be set to specific value
    * Hardware attribute is not populated
    * In case when there are multiple groups configured on the miner, only pools associated with the first one are shown in the tool

************************************************
Configure Braiins OS+ using Remote (SSH) Package
************************************************

The installation script uses two types of arguments:

   * positional arguments - required for the installation to be completed;
   * optional arguments - optional (i.e. not required) for the installation to be completed.

The syntax for the installation script is the following:

  ::

    usage: upgrade2bos.py [-h] [--no-backup] [--no-nand-backup]
                      [--no-keep-network] [--keep-hostname] [--no-wait]
                      hostname

**Positional arguments:**

.. code-block:: none

    hostname [hostname ...] Hostname or an IP address of the selected miner

**Optional arguments:**

.. code-block:: none

  -h, --help            show this help message and exit
  --no-backup           skip miner backup before upgrade
  --no-nand-backup      skip full NAND backup (config is still being backed
                        up)
  --no-keep-network     do not keep miner network configuration (use DHCP)
  --keep-hostname       keep miner hostname
  --no-wait             do not wait until system is fully upgraded


*************
Pool Settings
*************

Users can specify multiple pools. All the pools in one group use the fail-over multi-pool strategy, which means
that BOSminer will automatically switch to the second pool if the first pool dies.

Configuration is available through web GUI (*Miner -> Configuration*) or in the configuration file ``/etc/bosminer.toml``.

The syntax is the following:

  ::

     [[group]]
     name = 'Default'
     quota = 1

     [[group.pool]]
     enabled = true
     url = 'stratum2+tcp://v2.stratum.slushpool.com/u95GEReVMjK6k5YqiSFNqqTnKU4ypU2Wm8awa6tmbmDmk1bWt'
     user = 'username.workername'
     password = 'secret'

  * *name* - Name of the pool group (explained in the section *Pool Groups* below)
  * *quota* - User set quota for the group (explained in the section *Pool Groups* below)
  * *enabled* - Initial state of the pool after BOSminer initialization (default=true)
  * *url* - Mandatory argument for server URL specified in the format
    ``scheme://HOSTNAME:PORT/POOL_PUBLIC_KEY``. You don't have to
    specify an explicit port for *Stratum V2* on Slush Pool. The reason is
    that the protocol is still in development and we alternate between
    two default ports (**3336** and **3337**) across protocol
    upgrades. Miners that don't upgrade would still be able to use the
    previous version of the protocol. Miners that do upgrade won't
    have to worry about upgrading their mining URL with a new port.
    There is a *new* required element of the URL in the path and that
    is the public key advertised by the pool that the mining software
    uses to verify the authenticity of the mining endpoint that it
    connects to. This prevents man-in-the-middle-attacks that attempt
    to steal hashrate. Any such attempt results in failed verification
    and the software refuses to use the given pool entry.
  * *user* - Mandatory argument for username specified in format ``USERNAME.WORKERNAME``
  * *password* - Optional password settings

Pool Groups
===========

  Users can create multiple different pool groups. All pools within one group use the fail-over
  multi-pool strategy described above. When multiple pool groups are created, the work is
  distributed to each group with the load-balance strategy, either on a Quota basis or
  with a Fixed Share Ratio.

  Example:

  Group 1 has two pools specified and is assigned a Quota of "1". Group 2 has just one pool specified
  and is assigned a Quota of "2".

  - The work is assigned to the groups with a 1:2 ratio 
  - Group 2 will get twice the amount of work assigned as Group 1.
  - If the first pool in Group 1 dies, BOSminer will switch to the second pool in Group 1.


  It's possible to use Fixed Share Ratio instead of Quota, which will split the work by a specified
  percentage. A Quota of 1:1 is equivalent to a Fixed Share Ratio of 0.5 (50%) - both of those
  settings will split the work in half and send it to the two groups.

  Configuration is available through web GUI (*Miner -> Configuration*) or in the configuration
  file ``/etc/bosminer.toml``.

  Example of two groups and multiple pools:

  ::

     [[group]]
     name = 'MyGroup1'
     quota = 1

     [[group.pool]]
     enabled = true
     url = 'stratum2+tcp://v2.stratum.slushpool.com/u95GEReVMjK6k5YqiSFNqqTnKU4ypU2Wm8awa6tmbmDmk1bWt'
     user = 'userA.worker'

     [[group.pool]]
     enabled = true
     url = 'stratum+tcp://stratum.slushpool.com:3333'
     user = 'userA.worker'

     [[group]]
     name = 'MyGroup2'
     quota = 2

     [[group.pool]]
     url = 'stratum+tcp://stratum.slushpool.com:3333'
     user = 'userB.worker'

With this setup, the work will be split between the two groups in ratio 1:2. By default, the miner
will be mining on the first pool from the group "MyGroup1" and on the one pool defined in the group
"MyGroup2". If the first pool in "MyGroup1" dies, the miner will be mining on the second pool from
the group "MyGroup1". Since a second pool url isn't specified for "MyGroup2", nothing will be done
if the pool in "MyGroup2" fails.

*******************
Hash Chain Settings
*******************

Optional configuration for overriding the default settings for all hash chains. This allows the
users to control the frequency and voltage of each hash chain and allows them to turn AsicBoost o
n and off. While autotuning is enabled, these settings are ignored. The global hash chain settings
can also be overridden by per-chain settings.

Configuration is available through web GUI (*Miner -> Configuration*) or in the configuration file ``/etc/bosminer.toml``.

The syntax is the following:

  ::

     [hash_chain_global]
     asic_boost = true
     frequency = 650.0
     voltage = 8.8

  * *asic_boost* - Enable or disable AsicBoost support (default=true)
  * *frequency* - Set default chip frequency in MHz for all hash chains (default=650.0)
  * *voltage* - Set default voltage in V for all hash chains (default=8.8)

The syntax for per-chain settings is the following:

  ::

     [hash_chain.6]
     frequency = 650.0
     voltage = 8.8

  * *[hash_chain.6]* - Override the global settings for hash chain '6'
  * *frequency* - Override the global chip frequency in MHz for hash chain '6' (default='hash_chain_global.frequency')
  * *voltage* - Override the global voltage in V for hash chain '6' (default='hash_chain_global.voltage')

***************************
Temperature and Fan Control
***************************

Temperature Control Mode
========================

  Braiins OS+ supports automatic temperature control (using `PID controller <https://en.wikipedia.org/wiki/PID_controll>`__).
  The controller can operate in one of three modes:

  -  **Automatic** - Miner software tries to regulate the fan
     speed so that miner temperature is approximately at the target
     temperature (which can be configured). The allowed temperature range
     is 0-200 degree Celsius.
  -  **Manual** - Fans are kept at a fixed, user-defined speed,
     no matter the temperature. This is useful if you have your own way of
     cooling the miner or if the temperature sensors don’t work. Allowed
     fan speed is 0%-100%. The control unit monitors only hot and dangerous temperatures.
  -  **Disabled** - **WARNING**: this may damage the device because no control is done!

  The temperature control mode can be changed in the *Miner -> Configuration* page or in the configuration file ``/etc/bosminer.toml``.

  **Warning**: misconfiguring fans (either by turning them off or to a
  level that is too slow, or by setting the target temperature too high)
  may irreversibly **DAMAGE** your miner.

Default temperature limits
==========================

  The default temperature limits are set to prevent the miner from overheating and being damaged.

  * **Target temperature** is a temperature that the miner will try to maintain (*default is* **89°C**).
  * **Hot temperature** is a threshold at which the fans start to run at 100% (*default is* **100°C**).
  * **Dangerous temperature** is a threshold at which BOSminer shuts down in order to prevent overheating and damaging the miner (*default is* **110°C**).

  Default temperature limits can be adjusted in the *Miner -> Configuration* page or in the configuration file ``/etc/bosminer.toml``.

Temperature and Fan Control configuration in ``bosminer.toml``
==============================================================

  The default values can be overridden by editing the corresponding lines in the configuration file, located in ``/etc/bosminer.toml``.

  The syntax is the following:

  ::

     [temp_control]
     mode = 'auto'
     target_temp = 89
     hot_temp = 100
     dangerous_temp = 110

  * *mode* - Set temperature control mode (default='auto')
  * *target_temp* - Set target temperature in Celsius (default=89.0). This option is ONLY used when 'temp_control.mode' is set to 'auto'!
  * *hot_temp* - Set hot temperature in Celsius (default=100.0). When this temperature is reached, the fan speed is set to 100%.
  * *dangerous_temp* - Set dangerous temperature in Celsius (default=110.0). When this temperature is reached, the mining is turned off! **WARNING:** setting this value too high may damage the device!


  ::

     [fan_control]
     speed = 100
     min_fans = 1

  * *speed* - Set a fixed fan speed in % (default=70). This option is NOT used when *temp_control.mode* is set to 'auto'!
  * *min_fans* - Set the minimum number of fans required for BOSminer to run (default=1).
  * To completely **disable fan control**, set 'speed' and 'min_fans' to 0.

Fan operation
=============

  1. Once temperature sensors are initialized, fan control is enabled. If
     temperature sensors are not working or they read out a temperature of
     0, fans are automatically set to full speed.
  2. If the current mode is “fixed fan speed”, the fan is set to a given
     speed.
  3. If the current mode is “automatic fan control”, the fan speed is
     regulated by temperature.
  4. In case the miner's temperature is above the *HOT temperature*, fans are set to
     100% (even in “fixed fan speed” mode).
  5. In case the miner's temperature is above the *DANGEROUS temperature*, BOSminer
     shuts down (even in “fixed fan speed” mode).

******************
Tuning Adjustments
******************

Tuning can be configured either via web GUI, using BOS Toolbox or in the configuration file ``/etc/bosminer.toml``.

To make a configuration change via web GUI, enter the *Miner -> Configuration* menu and edit
the *Autotuning* section.

To make a configuration change on multiple devices using the **BOS Toolbox**, follow the steps in the section :ref:`bosbox_configure`.

To make a configuration change in the configuration file, connect to the miner via SSH and edit
the file ``/etc/bosminer.toml``. The syntax is the following:

  ::

     [autotuning]
     enabled = true
     psu_power_limit = 1200

The *enabled* line can hold values *true* for enabled autotuning, or *false* for disabled autotuning.
The *psu_power_limit* can hold numeric values (min. 100 and max. 5000), representing the PSU power
limit (in Watts) for three hashboards and the control board.

Alternatively, it's possible to turn on autotuning automatically after the installation finishes with the ``Set Power Limit`` option (or with the ``--power-limit POWER_LIMIT``   argument in the installation command).

*********************
Dynamic Power Scaling
*********************

Dynamic Power Scaling automatically lowers the power limit of the miner by a user-set amount if the device reaches the *Hot Temperature*. Upon reaching the user-set minimal power limit, the miner shuts down in order to cool down. The miner starts to work on the original power limit again after a user-set period of time.

Dynamic Power Scaling can be configured either via web GUI, using BOS Toolbox or in the configuration file ``/etc/bosminer.toml``.

To make a configuration change via web GUI, enter the *Miner -> Configuration* menu and edit
the *Dynamic Power Scaling* section.

To make a configuration change on multiple devices using the **BOS Toolbox**, follow the steps in the section :ref:`bosbox_configure`.

To make a configuration change in the configuration file, connect to the miner via SSH and edit
the file ``/etc/bosminer.toml``. The syntax is the following:

  ::

     [power_scaling]
     enabled = false
     power_step = 100
     min_psu_power_limit = 800
     shutdown_enabled = true
     shutdown_duration = 3.0

The *enabled* line can hold values *true* for enabled Dynamic Power Scaling, or *false* for disabled Dynamic Power Scaling.
The *power_step* can hold numeric values (min. 100 and max. 1000), representing the power limit step-down (in Watts), which happens each time miner hits *HOT* temperature.
The *min_psu_power_limit* can hold numeric values (min. 100 and max. 5000), representing the minimal PSU power limit for the Dynamyc Power Scaling. If *psu_power_limit* is at *min_psu_power_limit* level and miner is still *HOT* and *shutdown_enabled* is true, then miner is shut down for a period of time, defined in the *shutdown_duration* value (in hours). After that, miner is started but with the initial value of *psu_power_limit* (*PSU power limit* in the *Autotuning* section).

************
Auto-upgrade
************

When auto-upgrade is turned on, the machine will periodically check for a new version of Braiins OS+ and upgrade to it automatically when found. This feature is turned on by default after switching from stock firmware, but it has to be turned on manually if the user upgraded from an older version of Braiins OS or Braiins OS+.

Auto-upgrade can be configured either via web GUI or using BOS Toolbox.

To make a configuration change via web GUI, enter the *System -> Upgrade* menu and edit
the *System Upgrade* section.

To make a configuration change on multiple devices using the **BOS Toolbox**, follow the steps in the section :ref:`bosbox_configure`.

Alternatively, it's possible to turn **off** auto-upgrade during the installation by selecting the ``No Auto-Upgrade`` option (``--no-auto-upgrade`` argument in the installation command).

**Note:** The auto-upgrade feature has a time-randomization implemented in order to prevent high bandwidth load on farms. That means that the devices won't all upgrade at the same time. Auto-upgrade checks for new version three times a day.

************
SSH password
************

You can set the miner’s password via SSH from a remote host by running
the below command and replacing *[newpassword]* with your own password.

  * Note: Braiins OS+ does **not** keep a history of the commands executed.

  .. code:: bash

     ssh root@[miner-hostname-or-ip] 'echo -e "[newpassword]\n[newpassword]" | passwd'

To do this for several hosts in parallel you could use
`p-ssh <https://linux.die.net/man/1/pssh>`__.

****************
MAC & IP address
****************

By default, the device’s MAC address stays the same as it is inherited
from firmware (stock or Braiins OS) stored in the device (NAND). That way, once
the device boots with Braiins OS+, it will have the same IP address as it
had with the factory firmware.

Alternatively, you can specify a MAC address of your choice by modifying
the ``ethaddr=`` parameter in the ``uEnv.txt`` file (found in the first
FAT partition of the SD card).

***************
Model Detection
***************

This configuration option allows overriding the result of hardware auto-detection and honor the preset hardware type in the configuration. This is to cover the situation where all 3 hashboards have corrupted EEPROM's. If enabled, the model will be taken from the **[format] - model** option.

In order to enable this functionality, add the following lines to the ``/etc/bosminer.toml`` file. This way, the model will be taken from the **model** field.

  ::

     [model_detection]
     use_config_fallback = true

**Example:** the miner is ``Antminer S17``, but the EEPROMs contain false information, that it's ``Antminer T17e``. In order to override the model detection and set it to a correct model, ``Antminer S17``, correct the ``model`` field and add the lines from above.

Content of ``/etc/bosminer.toml`` - **Wrong model**

  ::

     [format]
     version = '1.2+'
     model = 'Antminer T17e'
     generator = 'BOSer (boser-antminer 0.1.0-4b746172)'
     timestamp = 1629888291
     ...

Content of ``/etc/bosminer.toml`` - **Correct model, after editing**

  ::

     [format]
     version = '1.2+'
     model = 'Antminer S17'
     generator = 'BOSer (boser-antminer 0.1.0-4b746172)'
     timestamp = 1629888291
     
     [model_detection]
     use_config_fallback = true
     ...