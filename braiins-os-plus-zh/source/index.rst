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
