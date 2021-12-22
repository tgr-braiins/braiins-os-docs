.. toctree::
   :hidden:
   :maxdepth: 3
   :caption: Table of Contents:
   :glob:

   self
   Setup/index*
   Configuration/index*
   Basic User's Guide/index*
   Development/index*
   Braiins OS+ Manager/index*

---------------

.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#####
Intro
#####

Braiins OS+ is an operating system for ASIC miners. It is based on the `Braiins OS <https://braiins-os.com/community-edition>`_ product and provides additional proprietary algorithms for autotuning of miners. When a user provides maximum allowed power consumption in Watts, the system will automatically optimize the mining process to maximize hash rate. This process works across a wide spectrum of inputs, allowing you to optimize for the best possible efficiency or maximum hash rate based on economical considerations. Internal testing shows that for the Antminer S9, it’s possible to achieve efficiency of 70J/THs or even better for low Watts setting. For high power consumption, hash rate can increase by 20%+ (comparing to Antminer S9, 13.5 TH/s stock setting with ~ 94J/TH).

Currently supported devices are Bitmain’s Antminer S9, s9i, S9j, S17, S17 Pro, S17+, T17, T17+, S17e and T17e. Antminer x19 and Whatsminer M20S support is planned for the near future.

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
* Auto-upgrade mechanism
* Dynamic Power Scaling, which lowers the power limit in case of high temperatures, for continuous mining
* **BTC Tools Support**

  * Braiins OS+ is supported by BTC Tools - batch management tool for miners. New versions of Braiins OS+ are supported - upgrade via Toolbox if you are using versions before 20.11. S9 and as well as x17 miners with Braiins OS+ are supported. BTC Tools for Windows/Linux can be downloaded here `here <https://btccom.zendesk.com/hc/en-us/articles/360020105012>`_. On the same page, documenation on how to use BTC Tools is available.
  
  * With exception of the below, Braiins OS+ supports all features of BTC Tools.
  
  * Limitations of BTC Tools when using with Braiins OS+:

    * Settings in Overclock/Underclock section does not affect Braiins OS+
    * Enhanced LPM will turn autotuning on and set miner's power limit to 2/3 of the default power limit for the miner
    * Disabling Enhanced LPM keeps autotuning on its last state and sets miner's power limit to the default power limit for the miner (hardware specific)
    * Note: both LPM and Enhanced LPM options are used only when "Power Control" is checked. Otherwise, machine specific settings is kept.
    * Autotuning cannot be currently turned off with BTC Tools and power limit cannot be set to specific value
    * Hardware attribute is not populated
    * In case when there are multiple groups configured on the miner, only pools associated with the first one are shown in the tool

*******************
Support and Contact
*******************

Have questions?
Our dev and support teams are always available to help.

Join our Telegram group:

  * `EN group <https://t.me/BraiinsOS>`_
  * `RU group <https://t.me/BraiinsOS_RU>`_
  * `ZH group <https://t.me/BraiinsOS_ZH>`_

You can also `send VIP request <https://help.slushpool.com/en/support/tickets/new>`_ to our support team.


*********
Changelog
*********

21.12
---------------------------

This is a major release that provides support for Antminer S19J Pro (beta)

* All Mining Hardware

  * [feature] autotune profile is being added to the Get Help files, for better support
  * [feature] immersion mode toggle button added to the web interface
  * [feature] logs are now less verbose, annoying temperature messages have been removed
  * [feature] logs no longer contain color codes as it confuses web log console
  * [feature] log reason for miner shutdown
  * [bug] fixed issue with per-hashboard hashrate showing the total hashrate in the graphs
  * [bug] voltage ramping has been reworked and is now quicker
  * [bug] Bosminer with autotuning off now correctly starts with user defined configuration
  * [bug] Removed logrotate information from syslog

* Antminer X17, X19

  * [feature] support for Antminer S19J Pro (beta)
  * [feature] improved power consumption prediction for Antminer S19J Pro
  * [feature] chip temperature for the X19 models is being estimated based on PCB temperature
  * [feature] removed fan override for the autotuning, default is 100%
  * [bug] fixed an issue with chips not reachable on X19 models

* Known issues:

  * Aftermarket control boards sometimes freeze completely

21.09.3
---------------------------

This is a minor bug fix release for Antminer X19 family

* Antminer family

  * [bug] machine override in bosminer.toml no longer causes the web frontend to block pool settings
  * [feature] EEPROM content is written into system log when autodetection fails for troubleshooting reasons

* Antminer X19

  * [bug] fixed autodetection problem that was confusing some S19Pro for S19 machines
  * [feature] further improve autodetection of S19 machines

21.09.2
---------------------------

This is a miner bug fix release for Antminer X17/X19 family

* Antminer X17, X19

  * [bug] enable tuner configuration for S17Pro machine
  * [bug] fixed power controller lockups

21.09.1
---------------------------

This is a minor release that extends X19 power supply limit for immersion setups.

* Antminer X19

  * [feature] Extend power limit upto 6500 W on APW12. This is for modified PSU's that can handle this power limit!

21.09
---------------------------

This is a major release that presents full web interface overhaul and an improved SD card installation method.

* All Mining Hardware

  * [feature] system running from SD card now supports upgrade and auto-upgrade like in case of system running from internal memory (NAND)
  * [feature] BOSminer will now automatically pause mining, if there is no pool alive, reducing the power consumption to minimum
  * [feature] new web interface, with dark mode and translation support (previously available in nightly builds)

* Antminer X17

  * [feature] improved manual model override, to cover the situation where all 3 hashboards have valid EEPROM's but the content is for hashboards from a different model. Typical scenario: you have a second hand S17 machine and the previous owner has rewritten the hashboard EEPROM's with T17e profiles. 

21.06.1
---------------------------

This is a minor bug fix release that improves Antminer T17e support.

* Antminer X17

  * [bug] use proper chip initialization voltage for T17e

21.06
---------------------------

This is a major release that provides improved support for the Antminer X17(including e) family.

* Antminer X17

  * [feature] improved tuner ensures optimum miner performance at user configured power levels
  * [feature] support for S17e and T17e
  * [feature] improved support for T17, T17+, S17, S17+
  * [feature] Braiins OS+ Manager support enabled for the entire x17 family
  * [feature] improved DPS, Dynamic Power Scaling now also automatically upscales the power limit, when the miner's temperature is at least 5 degrees bellow the HOT limit and the fans are running bellow 80%.
  * [feature] BOSminer will run and ignore incorrect configurations only when Braiins OS+ Manager is used so that the configuration can be fixed. If Braiins OS+ Manager is not used, BOSminer will power off when there is an incorrect configuration.


21.04
---------------------------

This is a major release for Antminer S9 that adds support for Braiins OS+ Manager - a cloud solution for miners management and monitoring.

* All mining hardware types

  * [feature] support for Braiins OS+ Manager - a cloud solution for miner management and monitoring, created in collaboration with FarmGod
  * [feature] BOSminer has now reduced additional network traffic to absolute minimum when probing for alive stratum servers
  * [bug] autotuning is now being enabled automatically when using the SD boot method
  * [bug] BOSminer will run even when the configuration is incorrect to avoid connection loss due to BOSminer being stopped
  * [bug] fixed an issue with long reconnect to pools when the public IP was changed

21.02
---------------------------

* All mining hardware types

  * [feature] the web interface now has a Support Tool that can generate archive with logs that can be sent to us
  * [feature] new GUI dashboard provides better overview of miner health and performance in one condensed page
  * [feature] toolbox improvements include listing miners from discover script and single IP command
  * [feature] image for SD card has auto-install feature to NAND that eliminates the need for using a desktop machine to trigger installation from SD completely


* Antminer X17

  * [feature] mining on X17 family can be quickly paused/resumed which is suitable for farms participating in grid programs. E.g. "pause" command looks like this: `echo '{"command":"pause"}' | nc IP_ADDRESS 4028`

20.12
---------------------------

* All mining hardware types

  * [bug] nightly builds from now on will point to nightly feeds as expected
  * [bug] DHCP server on the network interface has been disabled. Apologies, for this typo


* Antminer X17

  * [feature] for miners with locked machine, we now provide a mechanism to configure the SD image so that it would install BOS+ into NAND fully automatically
  * [feature] all X17 models with Macronix NAND flash memory are now supported
  * [feature] new configuration section [model_detection] has been added that allows overriding result of hardware autodetection and honor the preset hardware type in the configuration. This is to cover the situation where all 3 hashboards have corrupted EEPROM's. See use_config_fallback configuration option
  * [feature] new FPGA allows overclocking upto 950 MHz (NOTE: this frequencies are realistic only for immersion super cooling setups!)
  * [feature] voltage setting has been made more robust to support machines that had problems with voltage setting within a specified timeout

20.11
---------------------------

This is a major release that improves performance of X17 family tuning and overall operation.

* All mining hardware types

  * [feature] - there is now a single BOS Toolbox download which can be used for all hardware types. It also allows for mixing S9 and X17 models in one list.csv file, so users can do everything in batch (install, configure, uninstall, etc.) even with multiple hardware types.

* Antminer X17

  * [feature] frequency of the entire family limitted to 750 MHz
  * [feature] improved tuning of the whole family
  * [feature] implemented workarounds for failing hashboards
  * [feature] support for T17, T17+
  * [bug] improved performance of S17+ hardware
  * [bug] API lockup when tuner is running has been fixed, the charts in web interfaces no longer get stuck for couple seconds between tuner restarts

* Antminer S9

  * [feature] - the BOS Toolbox is the same for Braiins OS and Braiins OS+ now with Braiins OS+ being the default. Users who want to install the open-source version can do so with the parameter --open-source

20.10
---------------------------

This is a major release that adds beta support for Antminer S17+.

* All mining hardware types

  * [feature] procd now waits up to 20s to allow proper shutdown of BOSminer
  * [feature] BOSminer monitor now only spins the fans for when BOSminer has been stopped in order to cool down the machine
  * [bug] stratum client no longer complains about 'Stratum: unexpected accepted solution #0'
  * [bug] stratum client incorrect state bug has been fixed (i.e. you should not see "ERRO BUG: 'finish_shutdown_or_recover': unexpected state 'Starting'" anymore)
  * [feature] referral program support has been made more robust to support multiple hardware types in a single referral configuration
  * [feature] BOS management protocol is now relayed between devfee stratum V2 connections in case of fail over to a backup connection

* Antminer S9

  * there were no hardware specific changes

* Antminer S17

  * [feature] support for S17+ has been added
  * [feature] default temperature limits have been lowered even further to target temp: 72 C, hot temp: 85 C, dangerous temp: 92 C as the S17 family is very sensitive to overheating due to quality of the solder material used on the hashboard PCB's
  * [feature] we have added automatic detection of control board variant (C49 vs C52) to drive fans properly
  * [feature] Braiins OS would refuse to install on X17 machines that have the 'Macronix' NAND flash. Currently, only the 'Micron' NAND flash is supported
  * [feature] autodetection of S17, S17Pro, S17+ has been implemented and there is a single image for all of these machine types
  * [feature] power limits are now dynamically calculated based on the detected machine

20.09.1
---------------------------

This is a bug fix release.

* All mining hardware types

  * [feature] we have disabled rebind protection in DNSmasq to recover original name resolution behavior. What it means is that mining farm DNS server can serve responses that point to private (local) IP ranges. This improves user experience should a farm have a local stratum proxy accessible by name.
  * [feature] support for optional mining.{ping/pong} stratum messages that some pools use for checking miner liveness
  * [bug] workaround for a yet-another-broken stratum V1 implementation has been deployed. The problem is that some stratum V1 implementation don't mark result as 'null' in response that carries an error but put various things into it (e.g. false). The stratum client would abort a connection in such case. We have made this into a warning log message and the client ignores such anomalies and can extract the useful payload out of it
  * [bug] bosminer.toml format version is now correctly being migrated

* Antminer S17

  * [feature] hot temperature limit has been lowered to 100 C
  * [debug feature] last error of a machine is now by default being shipped to our logging server. This is to simplify debugging any S17 issues. If this temporary feature is not desired, it can be disabled in /etc/init.d/bosminer by replacing "PROG=/usr/bin/bosminer-panic-wrapper" with "PROG=/usr/bin/bosminer"

20.09
---------------------------

This release brings support for Antminer S17 and S17 Pro and includes a maintenance release for the Antminer S9 family.

* All mining hardware types

  * [feature] implemented referral program - sellers of Braiins OS+ can now acquire a referral package (with a referral ID and configuration file) which will send them a portion of the devfee collected when applied by the referees.

* Known issues

  * [issue] displayed power consumption for S17 and S17 Pro is lower than the actual power consumption, this will be improved in the next releases.
  * [issue] BOSminer is slow at reconnecting to the pool when the internet provider changes the IP address for the user

20.06
---------------------------

This release aims to improve the usability of Braiins OS+ and BOS Toolbox by implementing new features and fixing the most critical issues.

  * All mining hardware types

    * [workaround] Support for yiimp based pools (e.g. prohashing) that incorrectly send a version rolling mask starting with '0x', which doesn't comply with the BIP-310 specification
    * [feature] Support stratum V1 passwords since they are used by some pools for algorithm switching and other hacks
    * [feature] Implementation of auto-upgrade mechanism. The machine will periodically check for a new version of Braiins OS and upgrade to it automatically when found. This feature is turned on by default when switching from stock firmware, but it has to be turned on manually when upgrading from an older version of Braiins OS
    * [feature] Improved system logging with the implementation of logrotate. System logs are now automatically compressed and saved on the NAND of the device which allows longer logs to be stored
    * [feature] Updated BOS Toolbox, which can now run custom commands in batch
    * [bug] NAND install from an SD card now properly migrates the configuration from the SD card, instead of from the old system on the NAND
    * [bug] Fixed the issue with *bosminer.toml* being empty when the miner is turned off before the system flushes the buffer
    * [bug] IP report button now works correctly
    * [feature] Autotuning subsystem now saves performance profiles into /etc/bosminer-autotune.json. The performance profiles are recorded for each power level and board index
    * [feature] Dynamic Power Scaling now automatically lowers the power limit of the miner by a user-set amount if the device reaches the *Hot Temperature*. Upon reaching the minimal power limit, the miner shuts down in order to cool down. The miner starts to work on the original power limit again after a user-set period of time

  * Antminer S9

    * [feature] We have switched back to Xilinx I2C IP core for communication with voltage controllers and extended it with glitch filtering for noisy environments
    * [feature] UART Rx line for communicating with hashing chips has been extended with glitch filtering


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
