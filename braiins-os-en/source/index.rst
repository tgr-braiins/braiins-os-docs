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

---------------

#####
Intro
#####

Braiins OS is a fully open-source operating system for ASIC miners. It was the first firmware to implement
overt AsicBoost in 2018, and it now features an implementation of the new mining protocol, Stratum V2.
Additionally, Braiins OS works in tandem with our new software component, BOSminer, which we've written
from scratch in Rust language as a replacement for the outdated CGminer.

Currently supported devices are Bitmain’s Antminer S9, s9i, S9j, S17, S17 Pro, S17+, T17 and T17+. Antminer S17e, T17e and Whatsminer M20S support is planned for the near future.

********
Features
********

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

*******************
Support and Contact
*******************

Have questions?
Our dev and support teams are always available to help.

Join our Telegram group:

  * `EN group <https://t.me/BraiinsOS>`_
  * `RU group <https://t.me/BraiinsOS_RU>`_
  * `ES group <https://t.me/BraiinsOS_ES>`_
  * `FA group <https://t.me/BraiinsOS_SlushPool_FA>`_
  * `ZH group <https://t.me/BraiinsOS_ZH>`_

*********
Changelog
*********

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

  * [feature] chip temperature is approximated from the board temperature in cause of faulty temperature sensor
  * [feature] integrated DNSmasq for local DNS cache to reduce the number if requests to domain names
  * [bug] fixed the issue with Fixed Share Ratio and Quota, where the hashrate was not split correctly
  * [feature] we have improved the backoff algorithm for unstable pools where the pool is considered stable only if it runs without errors for one hour

* Antminer S17

  * [note] Braiins OS for Antminer S17 does not differentiate between a "classic" S17 and the Pro version as both hardware types are almost identical

20.06
---------------------------

This release aims to improve the usability of Braiins OS and BOS Toolbox by implementing new features and fixing the most critical issues.

  * All mining hardware types

    * [workaround] Support for yiimp based pools (e.g. prohashing) that incorrectly send a version rolling mask starting with '0x', which doesn't comply with the BIP-310 specification
    * [feature] Support stratum V1 passwords since they are used by some pools for algorithm switching and other hacks
    * [feature] Implementation of auto-upgrade mechanism. The machine will periodically check for a new version of Braiins OS and upgrade to it automatically when found. This feature is turned on by default when switching from stock firmware, but it has to be turned on manually when upgrading from an older version of Braiins OS
    * [feature] Improved system logging with the implementation of logrotate. System logs are now automatically compressed and saved on the NAND of the device which allows longer logs to be stored
    * [feature] Updated BOS Toolbox, which can now run custom commands in batch
    * [bug] NAND install from an SD card now properly migrates the configuration from the SD card, instead of from the old system on the NAND
    * [bug] Fixed the issue with *bosminer.toml* being empty when the miner is turned off before the system flushes the buffer
    * [bug] IP report button now works correctly

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
  
    * [bug] some devices were experiencing random I2C controller bus lockups and would fail to communicate with hashboard power controllers connected to the shared I2C bus. We have found out that the cause was the Xilinx I2C controller core that we have integrated into the FPGA bitstream. We have switched to the I2C present in the SoC and the bitstream only routes the signal of the peripheral (IIC0) to corresponding FPGA pins.

20.03
---------------------------

See `WHATSNEW.MD <https://github.com/braiins/braiins/blob/master/braiins-os/whatsnew.md>`_ on github

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
