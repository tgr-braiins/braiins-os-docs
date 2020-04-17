##################
用户基本指南
##################

.. contents::
	:local:
	:depth: 2

**************
网页端统计数据
**************

下方解释了在矿机的网页图形交互界面（GUI）主页中显示的一些数据的意思。

计算板
===========

   * **ID**                    - 计算板ID, 由控制板上连接计算板接口标注的编号决定
   * **REAL HASH RATE**        - 计算板当前的实际算力
   * **NOMINAL HASH RATE**     - 计算板基于目前芯片频率，理应能达到的算力
   * **VOLTAGE**               - 计算板Voltage used on the hash chain
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
   * **ACTIVE**                - Active status: "Yes" - jobs are submited to the pool; "No" - pool is not used
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
always shows the *heartbeat* (i.e. flashes at a load average based
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

发现脚本
===============

  The script *discover.py* is to be used to discover
supported mining devices in the local network and has two working modes.
First, clone the repository and prepare the enviroment using the following commands:

.. code:: bash

    # clone repository
    git clone https://github.com/braiins/braiins-os.git
    
    cd braiins-os
    virtualenv --python=/usr/bin/python3 .env
    source .env/bin/activate
    python3 -m pip install -r requirements.txt

监听模式
-----------

在此模式下，按下IP Report按钮后，矿机的IP和MAC地址将会显示。参数 ``--format`` 可以用于改变IP/MAC信息的默认格式。

.. code:: bash

   python3 discover.py listen --format "{IP} ({MAC})"

   10.33.10.191 (a0:b0:45:02:f5:35)

扫描模式
---------

在此模式下，脚本扫描指定的网络范围以查询支持的设备。该参数应该包含IP地址列表或带掩码IP子网络（以下表列），以扫描整个子网络。

每个设备的输出包含MAC地址，IP地址，系统消息，主机名以及挖矿用户名。

.. code:: bash

   python3 discover.py scan 10.55.0.0/24

   00:7e:92:77:a0:ca (10.55.0.133) | bOS am1-s9_2018-11-27-0-c34516b0 [nand] {1015120 KiB RAM} dhcp(miner-w3) @userName.worker3
   00:94:cb:12:a0:ce (10.55.0.145) | Antminer S9 Fri Nov 17 17:57:49 CST 2017 (S9_V2.55) {1015424 KiB RAM} dhcp(antMiner) @userName.worker5

************************
Enter/Exit 恢复模式
************************

标准使用Braiins OS时，用户通常无需进入恢复模式。 ``restore2factory.py`` 降级过程使用它来恢复原始的原厂固件。在修复/检查当前安装系统时，也可以使用恢复模式。


恢复模式能以两种方式调用：

  *  *IP set按钮*——按下3秒钟，然后绿色LED会闪烁
  *  *SD卡* - SD卡——第一个文件分配表分区中包含带有**recovery=yes**命令的*uEnv.txt*文件
  *  *miner utility* - 矿机使用程序——从矿机的命令行启动 ``miner run_recovery`` 

通过重启设备可以退出恢复模式。 如果设备重新启动到恢复模式，则意味着安装/配置存在问题。

