##################
用户基本指南
##################

.. contents::
	:local:
	:depth: 2

**************
网页端统计数据
**************

下面是对矿机网页界面（GUI）主页中的一些数据的说明。

运算板（Hash Chains）
===========

   * **ID**                    - 运算板编号（由连接到控制板上的接口编号决定）
   * **REAL HASH RATE**        - 实时算力（运算板当前的实际算力）
   * **NOMINAL HASH RATE**     - 名义算力（运算板基于当前芯片频率，理应能达到的算力）
   * **VOLTAGE**               - 电压 （运算板电压）
   * **FREQUENCY**             - 频率（芯片平均频率）
   * **BOARD TEMP**            - 运算板温度 
   * **CHIP TEMP**             - 芯片温度
   * **ASIC#**                 - 工作中芯片数
   * **CORE#**                 - 工作中核心数
   * **HARDWARE ERRORS**       - 硬件故障数（硬件误算导致的无效工作）
   * **HW ERROR HASH RATE**    - 硬件故障算力损耗（实时算力和名义算力的差值）

矿池（Pools）
=====

   * **ID**                    - 矿池编号（由用户自定）
   * **URL**                   - 矿池地址（矿池的URL地址）
   * **USER**                  - 用户名（矿池上的用户名或矿工名，由用户自定）
   * **STATUS**                - 矿池状态（"Alive" 连接正常，"Dead" 连接失效）
   * **ACTIVE**                - 活跃状态（"Yes" 算力提交到矿池，"No" 矿池不活跃）
   * **ACCEPTED**              - 已接受（已被矿池接受的份额（Share）数）
   * **REJECTED**              - 已拒绝（已被矿池拒绝的份额（Share）数）
   * **STALE**                 - 已过时（已过时不再有效的份额（Share）数）
   * **LAST DIFFICULTY**       - 最后难度（最后的份额难度）
   * **GENERATED WORK**        - 已产生工作
   * **ASICBOOST**             - AsicBost状态（"Yes"开启 "No"关闭）

总览（Summary）
=======

   * **HASH RATE 1M**          - 分钟平均算力
   * **HASH RATE 15M**         - 刻钟平均算力
   * **HASH RATE 24H**         - 日平均算力
   * **FOUND BLOCKS**          - 已找到区块
   * **ACCEPTED**              - 已接受（已被矿池接受的份额（Share）数）
   * **DIFFICULTY ACCEPTED**   - 最后接受难度（矿池最后接受的提交份额（Share）的难度）
   * **REJECTED**              - 已拒绝（已被矿池拒绝的份额（Share）数）
   * **DIFFICULTY REJECTED**   - 最后拒绝难度（矿池最后拒绝的提交份额（Share）的难度）
   * **REJECTION RATIO**       - 拒绝比（被拒绝份额（Share）占总份额的比率）
   * **ELAPSED TIME**          - 开机时间
   * **HARDWARE ERRORS**       - 硬件故障数（硬件误算导致的无效工作）
   * **SHARES/1M**             - 每分钟接受量（平均每分钟被接受的份额（Share）数量）

风扇监控（Fan Monitor）
===========

   * **ID**                    - 风扇序号
   * **SPEED**                 - 风扇速度
   * **RPM**                   - 风扇转速

*************************
矿机LED灯信号（Miner Signalization (LED)）
*************************

矿机的LED灯信号取决于矿机的当前工作模式。在矿机前面板上有一个 **绿色** 和一个 **红色** 的两个LED灯，它们一起能表示出矿机的（*恢复* 和*正常*）这两种模式。矿机（内部）控制板上的LED灯表示矿机的*心跳* （即按平均占用率闪烁）。

恢复模式（Recovery Mode）
=============

Recovery mode is signaled by the **flashing green LED** (50 ms on, 950
ms off) on the front panel. The **red LED** represents access to a NAND
disk and flashes during factory reset when data is written to NAND.

恢复模式，以**绿色LED灯闪烁**（50毫秒亮，950毫秒灭）表示。**红色LED灯**代表访问矿机内部储存（NAND），比如在恢复原厂设置期间写入数据到NAND时闪烁。

正常模式（Normal Mode）
===========

The normal mode state is signaled by the combination of the front panel
**red** and **green LEDs** as specified in the table below:

   +--------------------+---------------------------+--------------------+
   | 红色LED            | 绿色LED                    | 含义           |
   +====================+===========================+====================+
   | 常亮                | 熄灭                      | *bosminer* 或      |
   |                    |                           | *bosminer_monitor* |
   |                    |                           | 不工作	       |
   +--------------------+---------------------------+--------------------+
   | 慢闪               | 熄灭                       | 哈希率低于预期      |
   |                    |                           | 哈希率的80%，       |
   |                    |                           | 或者矿机无法        |
   |                    |                           | 连接到任何矿池      |
   |                    |                           | （所有矿池死掉了）   |
   |                    |                           |		         |
   |                    |                           | 	                 |
   +--------------------+---------------------------+--------------------+
   | 熄灭                | 极慢闪 (1 秒亮，1秒灭）    | *矿机* 正常工作，   |
   |                    | 		            | 且哈希率高于预期的   |
   |                    |                           | 哈希率的80%         |
   |                    |                           | 			 |
   |                    |                           |	                 |
   +--------------------+---------------------------+--------------------+
   | 快闪  	       | 不适用                    | LED用户超控         |
   |                    |                           |(``miner fault_lig	 |
   |                    |                           | ht on``)	         |
   |                    |                           |     	         |
   +--------------------+---------------------------+--------------------+

*******************
Identifying a miner
*******************

LED blinking
============

可以通过让矿机的**红色LED**快闪的方式，在矿场里找出具体的某个矿机。

.. code:: bash

   miner fault_light on

同样，也可以禁用LED快闪运行：

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

