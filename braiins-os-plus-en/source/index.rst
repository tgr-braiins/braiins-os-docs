.. toctree::
   :hidden:
   :maxdepth: 3
   :caption: Table of Contents:
   :glob:

   self
   Setup/index*
   Configuration/index*
   Basic User's Guide/index*

---------------

#####
Intro
#####

Braiins OS+ is an operating system for ASIC miners. It is based on the `Braiins OS <https://braiins-os.com/community-edition>`_ product and provides additional proprietary algorithms for autotuning of miners. When a user provides maximum allowed power consumption in Watts, the system will automatically optimize the mining process to maximize hash rate. This process works across a wide spectrum of inputs, allowing you to optimize for the best possible efficiency or maximum hash rate based on economical considerations. Internal testing shows that for the Antminer S9, it’s possible to achieve efficiency of 70J/THs or even better for low Watts setting. For high power consumption, hash rate can increase by 20%+ (comparing to Antminer S9, 13.5 TH/s stock setting with ~ 94J/TH).

Currently supported devices are Bitmain’s Antminer S9, S9i, and S9j. Antminer S17 support is planned for the near future.

********
Features
********

 * State-of-the-art autotuning optimization to maximize hash rate or efficiency
 * Open-source operating system
 * Stratum V2 implementation with improved data efficiency and hashrate hijacking prevention
 * CGminer replacement (BOSminer) written from scratch in Rust language
 * Quick startup (5-7 seconds)
 * No random crashes due to undefined behavior
 * Bulk installation
 * Automatic updates with the standard opkg system
 * Fully customizable fan control (enables immersion cooling)
 * Advanced monitoring to prevent overheating and other issues

*******************
Support and Contact
*******************

Have questions?
Our dev and support teams are always available to help.

Join our Telegram group:

  * `EN group <https://t.me/BraiinsOS>`_
  * `RU group <https://t.me/BraiinsOS_RU>`_
  * `ZH group <https://t.me/BraiinsOS_ZH>`_

  You can also `send VIP request <https://slushpool.kayako.com/en-us/conversation/new/11>`_ to our support team.


*********
Changelog
*********

20.04
---------------------------

This release covers mostly user facing issues, installation/deinstalation difficulties and 1 major problem with I2C controller on S9's. Also, we now have nightly builds that are easy to enable via **bos** tool.

  * All mining hardware types

    * [feature] support for reconnect - we have implemented support for `client.reconnect` (stratum V1) and reconnect message for V2
    * [feature] installation/deinstallation (aka **upgrade2bos** and **restore2factory**) process (transition from factory firmware to Braiins OS or vica versa) has been improved:
    * [feature] custom pool user (`--pool-user`) can be set on command line
    * [feature] pool settings from the factory firmware are now automatically being migrated to BOSminer configuration. Migration can be disabled by specifying (`--no-keep-pools`)
    * [feature] we now provide binary form of **upgrade2bos** (based on pyinstaller) that contains the latest Braiins OS installation image
    * [feature] similarly, **restore2factory** (based on pyinstaller) is now available in binary form and doesn't require any longer downloading/finding out the correct factory firmware.
    * [feature] disk space and time consuming backup of the original firmware is now disabled by default (can be enabled by `--backup`)
    * [feature] keeping host name while performing first time install is now driving by 2 options `--keep-hostname` and `--no-keep-hostname` allowing to force override and automatic hostname generation based on MAC address
    * [feature] support for enabling/disabling nightly builds has been integrated into **bos** utility (and its legacy **miner** counterpart).
    * [feature] system now provides **logs** covering **longer timespan** of **BOSminer** operation due to enabling **log rotation** and compression of '/var/log/syslog.old' when it is bigger than 32 KiB
    * [bug] SD card image now contains slushpool authority public key that was missing
    * [bug] rejection rate is now correctly being displayed
    * [bug] unknown stratum V1 messages received from the server are now being logged for diagnostics

  * Antminer S9
  
    * [feature] Tuner status is now shown in the GUI. TUNERSTATUS API command was added.
    * [bug] some devices were experiencing random I2C controller bus lockups and would fail to communicate with hashboard power controllers connected to the shared I2C bus. We have found out that the cause was the Xilinx I2C controller core that we have integrated into the FPGA bitstream. We have switched to the I2C present in the SoC and the bitstream only routes the signal of the peripheral (IIC0) to corresponding FPGA pins.

20.03
---------------------------

  * All mining hardware types

    * [feature] configuration file allows specifying a power limit of the PSU that the autotuning algorithm
      will take into account in order to maximize the TH/W produced by the mining device

  * Antminer S9

    * [feature] Autotuning based on a user-specified power limit

************
Known Issues
************

The following lists issues that are known to be present in released version.

20.03 (Updated 3/30/2020)
-------------------------

  * GUI

   * Reference line in hashrate chart has incorrect value for average nominal hashrate. Issue
     only present when less than 3 hash chains are operational.
   * Rejection ratio is multiplied by 100. As an example, when rejection rate is 0.1%, then
     10% will be shown.

  * Configuration

    * SD Card installation will report missing Stratum V2 authentication key in the Miner/Configuration
      section (Error: missing upstream authority key for securing stratum2+tcp connection in pool").
      User can configure connection (including the key) in the configuration, or directly in
      the ``/etc/bosminer.toml`` file.
