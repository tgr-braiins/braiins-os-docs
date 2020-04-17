##################
用户基本指南
##################

.. contents::
	:local:
	:depth: 2

**************
网页端统计数据（Web Statistics）
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
   * **REJECTED**              - 已拒绝（已被矿池拒绝的份额数）
   * **STALE**                 - 已过时（已过时不再有效的份额数）
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
   * **DIFFICULTY ACCEPTED**   - 最后接受难度（矿池最后接受的提交份额的难度）
   * **REJECTED**              - 已拒绝（已被矿池拒绝的份额数）
   * **DIFFICULTY REJECTED**   - 最后拒绝难度（矿池最后拒绝的提交份额的难度）
   * **REJECTION RATIO**       - 拒绝比（被拒绝份额占总份额的比率）
   * **ELAPSED TIME**          - 开机时间
   * **HARDWARE ERRORS**       - 硬件故障数（硬件误算导致的无效工作）
   * **SHARES/1M**             - 每分钟接受量（平均每分钟被接受的份额数量）

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

恢复模式，以**绿色LED灯闪烁**（50毫秒亮，950毫秒灭）表示。**红色LED灯**代表访问矿机内部储存（NAND），比如在恢复原厂设置期间写入数据到NAND时就会闪烁。

正常模式（Normal Mode）
===========

正常模式以**红色**和**绿色LED灯**表示。下表说明了所有信号组合：

   +------------------+------------------+-----------------------+
   | 红色LED灯        | 绿色LED灯        | 含义                  |
   +==================+==================+=======================+
   | 常亮             | 熄灭             | *bosminer* 或         |
   |                  |                  | *bosminer_monitor*    |
   |                  |                  | 不工作                |
   +------------------+------------------+-----------------------+
   | 慢闪             | 熄灭             | 算力低于预期算力的80% |
   |                  |                  | 或矿机无法连接到      |
   |                  |                  | 任何矿池              |
   |                  |                  | （所有矿池地址都失    |
   |                  |                  | 效了）                |
   |                  |                  |                       |
   |                  |                  | 	                 |
   +------------------+------------------+-----------------------+
   | 熄灭             | 极慢闪           | *矿机* 正常工作，     |
   |                  | （1秒亮，1秒灭） | 且算力高于名义算力    |
   |                  |                  | 的80%                 |
   |                  |                  |                       |
   |                  |                  |	                 |
   +------------------+------------------+-----------------------+
   | 快闪  	    | 不适用           | 用户超控LED灯         |
   |                  |                  | （``miner fault_lig   |
   |                  |                  | ht on``）             |
   |                  |                  |                       |
   +------------------+------------------+-----------------------+

*******************
用灯光信号找矿机（Identifying a miner）
*******************

LED灯闪烁（LED blinking）
============

可以通过让矿机上的**红色LED灯**快闪的方式，在矿场里找出具体的某个矿机：

.. code:: bash

   miner fault_light on

要关闭LED灯快闪也是可以的：

.. code:: bash

   miner fault_light off

用于发现矿机的脚本（Discover script）
===============

  脚本文件*discover.py*可以用来在本地网络中发现矿机，它有两个工作模式。首先要做的是运行下面的命令，从Github上复制资料库并准备运行环境：

.. code:: bash

    # clone repository
    git clone https://github.com/braiins/braiins-os.git
    
    cd braiins-os
    virtualenv --python=/usr/bin/python3 .env
    source .env/bin/activate
    python3 -m pip install -r requirements.txt

监听模式（Listen mode）
-----------

在此模式下，按下IP Report按钮后将会显示矿机的IP和MAC地址。参数 ``--format`` 可以用于更改IP/MAC信息的默认格式。

.. code:: bash

   python3 discover.py listen --format "{IP} ({MAC})"

   10.33.10.191 (a0:b0:45:02:f5:35)

扫描模式（Scan mode）
---------

在此模式下，脚本会扫描指定的网络范围，找出支持的矿机。要扫描整个子网，参数设置应该包含IP地址表，或带掩码的IP子网（示例如下）。

输出信息包含每个矿机的MAC地址，IP地址，系统消息，矿机用户名（Hostname）以及矿池用户名。

.. code:: bash

   python3 discover.py scan 10.55.0.0/24

   00:7e:92:77:a0:ca (10.55.0.133) | bOS am1-s9_2018-11-27-0-c34516b0 [nand] {1015120 KiB RAM} dhcp(miner-w3) @userName.worker3
   00:94:cb:12:a0:ce (10.55.0.145) | Antminer S9 Fri Nov 17 17:57:49 CST 2017 (S9_V2.55) {1015424 KiB RAM} dhcp(antMiner) @userName.worker5

************************
进入或退出恢复模式（Enter/Exit Recovery Mode）
************************

正常使用Braiins OS时，通常没有必要进入恢复模式。``restore2factory.py`` 降级脚本可以用来恢复最初的原厂固件。在修复/诊断当前安装的系统时，也可以用恢复模式这一方式来找出并修复问题。

恢复模式的调用方式有两种不同方式：

*  *IP set按钮* - 按下3秒，然后绿色LED会闪烁
*  *SD卡* - 修改SD卡内第一个FAT格式的分区中，含带有**recovery=yes**命令的*uEnv.txt*文件
或

*  *miner utility* - 在矿机实用程序（Miner utility）中的命令行运行 ``miner run_recovery`` 命令

重启设备即可退出恢复模式。 如果设备重启到恢复模式，则表示安装/配置有问题。

