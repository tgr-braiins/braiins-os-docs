####################
配置（Configuration）
####################

.. contents::
  :local:
  :depth: 2

***************************
安装参数（Install arguments）
***************************

安装脚本使用两种参数：

   * 位置参数（Positional Arguments）——完成安装所必需的参数。
   * 可选参数（Optional Arguments）——安装过程中可选（即非必须）的参数。

安装脚本的句法如下：

  ::

    usage: upgrade2bos.py [-h] [--no-backup] [--no-nand-backup]
                      [--no-keep-network] [--keep-hostname] [--no-wait]
                      hostname

**位置参数：**

.. code-block:: none

    hostname [hostname ...] 选中矿机的矿机用户名或IP地址

**可选参数：**

.. code-block:: none

  -h, --help            显示帮助信息并退出
  --no-backup           在矿机固件升级后跳过备份
  --no-nand-backup      跳过对矿机内置储存（NAND）的完整备份（但仍备份矿机配置）
  --no-keep-network     不保留矿机的网络参数（使用DHCP）
  --keep-hostname       保留矿机用户名
  --no-wait             直到系统完成完整升级过程，不等待


***********************
矿池设置（Pool Settings）
***********************

用户可以同时设置多个矿池。在同组（Group）下的矿池，使用“多矿池故障转移策略”（Fail-over multipool strategy），这意味着在一个矿池不可用的情况下， BOSminer将自动切换到第二个矿池。

在矿机网页界面（*Miner（矿机） -> Configuration（配置）*）中，或在 ``/etc/bosminer.toml`` 配置文件中可以进行设置。

句法如下：

  ::

     [[group]]
     name = 'Default'
     quota = 1

     [[group.pool]]
     enabled = true
     url = 'stratum2+tcp://v2.stratum.slushpool.com/u95GEReVMjK6k5YqiSFNqqTnKU4ypU2Wm8awa6tmbmDmk1bWt'
     user = 'username.workername'
     password = 'secret'

  * *name* - 矿池组名（在下一部分关于矿池组的介绍中会具体说明）
  * *quota* - 用户设定矿机组内矿池的算力比例配额（在下一部分关于矿池组的介绍中会说明）
  * *enabled* - BOSminer初始化后的矿池初始状态 （默认值=true （矿池组启用））
  * *url* - 矿池服务器URL地址是必要参数，它以
    ``scheme://HOSTNAME:PORT/POOL_PUBLIC_KEY`` 为格式。
    使用Slush Pool矿池时，您无需为阶层Stratum V2协议指定特定的端口。
    因为目前该协议还在开发过程中，我们的矿池会在两个默认端口 （**3336** 和 **3337**）间切换。
    未升级的矿工仍可继续使用旧版阶层Stratum协议。已进行升级的矿工也无需担心因为新端口的原因，需要更新矿池服务器URL地址。
    在矿池服务器URL地址中，现在需要填写一个新元素——矿池的公钥，挖矿软件需要使用矿池的公钥来验证连接到的挖矿终点。
    如果对矿工算力进行中间人攻击则会验证失败，软件会拒绝所给的矿池地址，从而预防中间人攻击窃取矿工的算力
  * *user* - 用户名是必要参数，它以 ``USERNAME.WORKERNAME`` （用户名.矿工名）的格式指定
  * *password* - 密码的设置是非必须的

矿池组（Pool Groups）
===================

  用户可以创建多个不同的矿池组。位于同组内的矿池都使用上文所述的“多矿池故障转移策略”（Fail-over multipool strategy）。
  在创建了多个多池组的情况下，算力会基于比例配额（Quota basis），或基于固定百分比（Fixed Share Ratio）按照负载平衡的策略进行分配。

  案例说明:

  1号矿池组的比例配额（Quota）为"1"，其中有2个矿池地址。2号矿池组的比例配额为"2"，其中只有1个矿池地址。
  
  - 两个矿池组的算力分配为1：2。
  - 分配到2号矿池组的算力始终会是分配到1号的两倍。
  - 如果1号矿池组中的第一个矿池地址不可用，BOSminer将会自动切换到1号矿池组中的第二个矿池地址。
  
  基于固定百分比（Fixed Share Ratio）和基于比例配额（Quota basis）的算力分配模式不可以同时使用，只能二选一！
  在矿池组比例配额为1：1的情况下，就相当于设置了0.5（50%）的固定百分比。 即对半分配发送到两个矿池组的算力。

  在矿机网页界面（*Miner（矿机） -> Configuration（配置）*）中，或在配置文件 ``/etc/bosminer.toml`` 中可以进行设置。
  
  两个矿池组和多个矿池地址的设置案例：

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

在上面的设置案例中，算力以1：2的比例分到了两个矿池组。
默认情况下，矿机会选择在1号组"MyGroup1"内的第一个矿池地址，和在2号组"MyGroup2"内设置的矿池地址挖矿。
如果1号组"MyGroup1"内的第一个矿池地址不可用，矿机会自动切换到组内的第二个矿池地址挖矿。
如果2号组"MyGroup2"内设置的矿池地址不可用，矿机则什么也不会做。

*******************************
运算板设置（Hash Chain Settings）
*******************************

运算板设置能超控所有运算板的默认设置，由矿工自行选择。
它让矿工能直接设置每个运算板的频率和电压，以及开关AsicBoost功能。
对单个运算板的设置能够超控所有运算板的全局设置。
**当矿机的自动调整功能（Autotuning）开启时，上述设置一律无效！**

在矿机网页界面（*Miner（矿机） -> Configuration（配置）*）中，或在配置文件 ``/etc/bosminer.toml`` 中可以进行设置。

句法示例如下：

  ::

     [hash_chain_global]
     asic_boost = true
     frequency = 650.0
     voltage = 8.8

  * *asic_boost* - 设置启用或禁用AsicBoost支持（默认值=true）
  * *frequency* - 为所有运算板设定以兆赫兹Mhz为单位的默认芯片频率 （默认值=650.0）
  * *voltage* - 为所有运算板设定以伏V为单位的默认电压（默认值=8.8）

设置超控单个运算板的句法示例如下：

  ::

     [hash_chain.6]
     frequency = 650.0
     voltage = 8.8

  * *[hash_chain.6]* - 超控'6'号运算板的全局设置
  * *frequency* - 超控'6'号运算板以兆赫兹Mhz为单位的全局芯片频率设置（默认值='hash_chain_global.frequency'）
  * *voltage* - 超控'6'号运算板以伏V为单位的全局芯片电压设置（默认值='hash_chain_global.voltage'）

******************************************
温度和风扇控制（Temperature and Fan Control）
******************************************

温度控制模式（Temperature Control Mode）
======================================

  Braiins OS+支持自动风扇控制 （使用 `PID控制器 <https://zh.wikipedia.org/wiki/PID%E6%8E%A7%E5%88%B6%E5%99%A8>`__）。
  控制器能在三种模式下运行：

  -  **自动（Automatic）** - 矿机软件自动调整风扇转速，使矿机的温度大概保持在一个目标温度。
     目标温度可调，它的允许设置范围在0-200摄氏度之间。
  -  **手动（Manual）** - 无论温度如何，风扇转速始终保持固定在用户自定义的转速。
     如果您有自己的降温方法，或在温度传感器不起作用的情况下，这一模式是很有用的。
     允许设置的风扇转速范围为0%-100%。控制器仅监控过热和危险温度。
  -  **禁用（Disabled）** - **警告**： 没有温度控制，设备可能会损坏！

  温度控制模式可以在矿机网页界面（*Miner（矿机） -> Configuration（配置）*）中，或在 ``/etc/bosminer.toml`` 配置文件中可以进行设置。

  **警告**: 不正确地配置风扇（无论是关闭风扇还是使用过低的转速，或设置太高的目标温度）可能导致您的矿机不可逆转地 **损坏** 。

默认温度限制（Default temperature limits）
========================================

  设置默认温度限制是为了防止矿机的过热及损坏。

  * **目标温度（Target temperature）** 指矿机会尝试保持的温度（*默认值* 为 **89°C**）。
  * **过热温度（Hot temperature）** 指风扇会开始以100%转速运行的阈值温度（*默认值* 为 **100°C**）。
  * **危险温度（Dangerous temperature）** 指为防止矿机的过热及损坏，BOSminer会自动关闭的阈值温度（*默认值* 为 **110°C**）。

  默认温度限制的温度值可以在 *Miner（矿机）  -> Configuration（配置）* 页面中，或在 ``/etc/bosminer.toml`` 配置文件中调整。
  
在 ``bosminer.toml`` 配置文件中的温度和风扇控制（Temperature and Fan Control configuration in bosminer.toml）
=========================================================================================================

  在配置文件 ``/etc/bosminer.toml`` 中，编辑相应行可以修改默认值。

  句法如下：

  ::

     [temp_control]
     mode = 'auto'
     target_temp = 89
     hot_temp = 100
     dangerous_temp = 110

  * *mode* - 温度控制模式设定 （默认值='auto'（自动））
  * *target_temp* - 设定以摄氏度为单位的目标温度（默认值=89.0）。 该选项仅在 'temp_control.mode' （温度控制模式）设定为 'auto' （自动）的情况下可用！
  * *hot_temp* - 设定以摄氏度为单位的过热温度（默认值=100.0）。 当矿机达到该温度时，风扇转速会自动调整为100%。
  * *dangerous_temp* - 设定以摄氏度为单位的危险温度（默认值=110.0）。 当矿机达到该温度时，矿机将会自动关闭！**警告：** 将危险温度值设置太高会损坏矿机！


  ::

     [fan_control]
     speed = 100
     min_fans = 1

  * *speed* - 设定以 %为单位（默认值=70）的风扇固定转速。 当 *temp_control.mode* 风扇控制模式）设定为 'auto'（自动）时，请不要使用本选项！
  * *min_fans* - 设定BOSminer运行所需要的最少风扇数量 （默认值=1）。
  * 要想完全 **禁用风扇控制**, 请将 'speed' （转速）和'min_fans' （最少风扇数）设定为0。

风扇的运行（Fan operation）
=========================

  1. 一旦温度传感器启动，风扇控制也将启用。如温度传感器失效，或温度读数为零，风扇转速将自动设置为全速。
  2. 如果当前模式为“固定风扇转速（Fixed fan speed）”，风扇将调节到设定的转速。
  3. 如果当前模式为“自动风扇控制（Automatic fan control)”，风扇的转速调整由温度决定。
  4. 如果矿机温度超过 *过热温度（HOT temperature）*, 风扇转速将自动设为100%（即使在“固定风扇转速（Fixed fan speed）”模式下）。
  5. 如果矿机温度超过 *危险温度（DANGEROUS temperature）*, BOSminer将会关闭（即使在“固定风扇转速（Fixed fan speed）”模式下）。


*************************
SSH远程密码（SSH password）
*************************

您可以通过SSH从远程主机运行以下的命令来设置矿机的密码，请您使用您自己想用的密码替换下方命令中的 *[newpassword]* 项。

  注：Braiins OS 不会保留已执行命令的历史记录。

  .. code:: bash

     ssh root@[miner-hostname-or-ip] 'echo -e "[newpassword]\n[newpassword]" | passwd'

如需在多台主机上同时执行此操作，可以使用 `p-ssh <https://linux.die.net/man/1/pssh>`__。

*********************************
MAC地址和IP地址（MAC & IP address）
*********************************

默认情况下，安装新固件后矿机的MAC地址，是从矿机（NAND）上的原有固件（原厂或Braiins OS）继承而来并保持不变。
同理，新安装Braiins OS的矿机开机后的IP地址和之前应该也是一样的。

此外，您也可以通过修改（位于SD卡第一个FAT分区中）的 ``uEnv.txt`` 文件中的 ``ethaddr=`` 参数，指定一个具体的MAC地址。
