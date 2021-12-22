
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

##################
Basic User's Guide
##################

.. contents::
	:local:
	:depth: 2

**************
Web Statistics
**************

Statistics that are displayed on the main page of the Web GUI are explained below.

Hash Chains
===========

   * **ID**                    - Hash Chain ID, corresponds with the labeling on the control board
   * **REAL HASH RATE**        - Actual hash rate of the hash chain
   * **NOMINAL HASH RATE**     - Theoretical hash rate of the hash chain, based on the frequency of the chips
   * **VOLTAGE**               - Voltage used on the hash chain
   * **FREQUENCY**             - Frequency of the chips (average)
   * **BOARD TEMP**            - Temperature reported by the sensors on the hash board
   * **CHIP TEMP**             - Temperature reported by the sensors on the chip
   * **ASIC#**                 - Number of functioning ASIC chips
   * **CORE#**                 - Summary of active cores on all functioning chips
   * **HARDWARE ERRORS**       - Number of hardware faults - invalid work due to a hardware miscalculation
   * **HW ERROR HASH RATE**    - Hash rate "loss" due to HW errors; adding HW Error Hash Rate together with the Real Hash Rate should equal the Nominal Hash Rate

Pools
=====

   * **ID**                    - Pool order, as specified by the user
   * **URL**                   - Mining pool URL address
   * **USER**                  - Username and worker name, as specified by the user
   * **STATUS**                - Status of the pool - "Alive" when the pool is reachable by the miner, "Dead" when the pool is not reachable on that URL
   * **ACTIVE**                - Active status: "Yes" - pool is being used to serve mining jobs; "No" - pool is not used
   * **ACCEPTED**              - Number of submited shares that were accepted by the pool
   * **REJECTED**              - Number of submited shares that were rejected by the pool
   * **STALE**                 - Number of submited shares for a job that is no longer valid
   * **LAST DIFFICULTY**       - Last share difficulty
   * **GENERATED WORK**        - Amount of generated work for the chips to solve
   * **ASICBOOST**             - AsicBoost status - "Yes" for enabled, "No" for disabled

Summary
=======

   * **HASH RATE 1M**          - Average hash rate for the last 1 minute
   * **HASH RATE 15M**         - Average hash rate for the last 15 minutes
   * **HASH RATE 24H**         - Average hash rate for the last 24 hours
   * **FOUND BLOCKS**          - Number of blocks found
   * **ACCEPTED**              - Number of submited shares that were accepted by the pool
   * **DIFFICULTY ACCEPTED**   - Difficulty of the last accepted share
   * **REJECTED**              - Number of submited shares that were rejected by the pool
   * **DIFFICULTY REJECTED**   - Difficulty of the last rejected share
   * **REJECTION RATIO**       - Ratio of rejected shares and total number of shares (including accepted)
   * **ELAPSED TIME**          - Elapsed time since BOSminer started
   * **HARDWARE ERRORS**       - Number of hardware faults - rejected work due to a hardware miscalculation
   * **SHARES/1M**             - Average amount of shares accepted per minute

Fan Monitor
===========

   * **ID**                    - Order of the fans
   * **SPEED**                 - Speed of the fan (% PWM)
   * **RPM**                   - Revolutions per minute of the fan

*************************
Miner Signalization (LED)
*************************

Miner LED signalization depends on its operational mode. There are two
modes (*recovery* and *normal*) which are signaled by the **green** and
**red LEDs** on the front panel. The LED on the control board (inside)
always shows the *heartbeat* (i.e.flashes at a load average based
rate).

Recovery Mode
=============

Recovery mode is signaled by the **flashing green LED** (50 ms on, 950
ms off) on the front panel. The **red LED** represents access to a NAND
disk and flashes during factory reset when data is written to NAND.

Normal Mode
===========

The normal mode state is signaled by the combination of the front panel
**red** and **green LEDs** as specified in the table below:

   +--------------------+---------------------------+--------------------+
   | red LED            | green LED                 | meaning            |
   +====================+===========================+====================+
   | on                 | off                       | *bosminer* or      |
   |                    |                           | *bosminer_monitor* |
   |                    |                           | are not running    |
   +--------------------+---------------------------+--------------------+
   | slow flashing      | off                       | hash rate is below |
   |                    |                           | 80% of expected    |
   |                    |                           | hash rate or the   |
   |                    |                           | miner cannot       |
   |                    |                           | connect to any     |
   |                    |                           | pool (all pools    |
   |                    |                           | are dead)          |
   +--------------------+---------------------------+--------------------+
   | off                | very slow flashing (1 sec | *miner* is         |
   |                    | on, 1 sec off)            | operational and    |
   |                    |                           | hash rate above 80 |
   |                    |                           | % of expected hash |
   |                    |                           | rate               |
   +--------------------+---------------------------+--------------------+
   | fast flashing      | N/A                       | LED override       |
   |                    |                           | requested by user  |
   |                    |                           | (``miner fault_lig |
   |                    |                           | ht on``)           |
   +--------------------+---------------------------+--------------------+

*******************
Identifying a miner
*******************

LED blinking
============

The local miner utility can also be used to identify a particular device
by enabling aggressive blinking of the **red LED**:

.. code:: bash

   miner fault_light on

Similarly to disable the LED run:

.. code:: bash

   miner fault_light off

Discover script
===============

The script *discover.py* is to be used to discover
supported mining devices in the local network and has two working modes.
First, clone the repository and prepare the enviroment using the following commands:

.. code:: bash

    # clone repository
    git clone https://github.com/braiins/braiins-os.git
    
    cd braiins-os/braiins-os/
    virtualenv --python=/usr/bin/python3 .env
    source .env/bin/activate
    python3 -m pip install -r requirements.txt

Listen mode
-----------

In this mode, IP and MAC addresses of the device are displayed after the
IP Report button is pressed. Parameter ``--format`` can be used to
change the default formatting of IP/MAC information.

.. code:: bash

   python3 discover.py listen --format "{IP} ({MAC})"

   10.33.10.191 (a0:b0:45:02:f5:35)

Scan mode
---------

In this mode, the script scans the specified network range for supported
devices. The parameter is expected to include a list of IP addresses or
an IP subnetwork with a mask (example below) to scan a whole subnetwork.

For each device, the output includes a MAC address, IP address, system
info, hostname, and a mining username configured.

.. code:: bash

   python3 discover.py scan 10.55.0.0/24

   00:7e:92:77:a0:ca (10.55.0.133) | bOS am1-s9_2018-11-27-0-c34516b0 [nand] {1015120 KiB RAM} dhcp(miner-w3) @userName.worker3
   00:94:cb:12:a0:ce (10.55.0.145) | Antminer S9 Fri Nov 17 17:57:49 CST 2017 (S9_V2.55) {1015424 KiB RAM} dhcp(antMiner) @userName.worker5

************************
Enter/Exit Recovery Mode
************************

Users don’t typically have to enter recovery mode while using Braiins OS
in a standard way. The ``restore2factory.py`` downgrade process uses it
to restore the original factory firmware from the manufacturer. It can
also be useful when repairing or investigating the currently installed
system.

Recovery mode can be invoked in the following different ways:

   *  *IP SET button* - hold it for *3s* until green LED flashes
   *  *SD card* - first partition with FAT contains file *uEnv.txt* with a line **recovery=yes**
   *  *miner utility* - call ``miner run_recovery`` from the miner’s command line

Recovery mode can be exited by rebooting the device. If the device reboots to the recovery mode, it means that
there is a problem with the installation or configuration.
