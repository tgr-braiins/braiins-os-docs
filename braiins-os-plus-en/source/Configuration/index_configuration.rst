#############
Configuration
#############

.. contents::
  :local:
  :depth: 2

****************************************
Configure Braiins OS+ using BOS+ Toolbox
****************************************

You can easily configure Braiins OS+ on multiple devices using the **BOS+ Toolbox**. In order to do so, follow the steps in the section :ref:`bosbox_configure`.

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

Tuning can be configured either via web GUI or in the configuration file ``/etc/bosminer.toml``.

To make a configuration change via web GUI, enter the *Miner -> Configuration* menu and edit
the *Autotuning* section.

To make a configuration change in the configuration file, connect to the miner via SSH and edit
the file ``/etc/bosminer.toml``. The syntax is the following:

  ::

     [autotuning]
     enabled = true
     psu_power_limit = 1200

The *enabled* line can hold values *true* for enabled autotuning, or *false* for disabled autotuning.
The *psu_power_limit* can hold numeric values (min. 100 and max. 5000), representing the PSU power
limit (in Watts) for three hashboards and the control board.

Alternatively, it's possible to turn on autotuning automatically after the installation finishes
specifying the ``--power-limit POWER_LIMIT``   argument in the installation command.

In order to change power limit on multiple devices, you can use
our configuration spreadsheet that will will generate commands for different use cases.

The spreadsheet is available `here <https://docs.google.com/spreadsheets/d/1H3Zn1zSm6-6atWTzcU0aO63zvFzANgc8mcOFtRaw42E>`_

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
