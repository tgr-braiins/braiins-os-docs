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
مقدمه
#####

Braiins OS is a fully open-source operating system for ASIC miners. It was the first firmware to implement
overt AsicBoost in 2018, and it now features an implementation of the new mining protocol, Stratum V2.
Additionally, Braiins OS works in tandem with our new software component, BOSminer, which we've written
from scratch in Rust language as a replacement for the outdated CGminer.

Currently supported devices are Bitmain’s Antminer S9, S9i, and S9j. Antminer S17 support is planned
for the near future.

********
امکانات
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

************************
پشتیبانی و ارتباط با ما
************************

سوالی دارید؟
تیم های پشتیبانی و پشتیبانی ما همیشه برای کمک در دسترس هستند.

به گروه تلگرام ما بپیوندید:

  * `EN group <https://t.me/BraiinsOS>`_
  * `FA group <https://t.me/BraiinsOS_SlushPool_FA>`_
  * `RU group <https://t.me/BraiinsOS_RU>`_
  * `ZH group <https://t.me/BraiinsOS_ZH>`_

*********
Changelog
*********

20.03
---------------------------

See WHATSNEW.MD (Will be published 3/31 on github)

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
