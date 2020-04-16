####################
配置
####################

.. contents::
  :local:
  :depth: 2

*****************
安装参数（Install arguments）
*****************

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

    hostname [hostname ...] 选中矿机的矿机名或IP地址

**可选参数：**

.. code-block:: none

  -h, --help            显示帮助信息并退出
  --no-backup           在矿机固件升级后跳过备份
  --no-nand-backup      跳过对矿机内置储存（NAND）的完整备份（但仍备份矿机配置）
  --no-keep-network     不保留矿机的网络参数（使用DHCP）
  --keep-hostname       保留矿机用户名
  --no-wait             直到系统完成完整升级过程，不等待


*************
矿池设置
*************

用户可以同时设置多个矿池。在同组（Group）下的矿池，使用“多矿池故障转移策略”（Fail-over multipool strategy），这意味着在一个矿池不可用的情况下， BOSminer将自动切换到第二个矿池。

在网站端的（*Miner（矿机） -> Configuration（配置）*）中，或在 ``/etc/bosminer.toml`` 配置文件中可以进行设置。

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

矿池组
===========

  用户可以创建多个不同的矿池组。位于同组内的矿池都使用上文所述的“多矿池故障转移策略”（Fail-over multipool strategy）。
  在创建了多个多池组的情况下，算力会基于比例配额（Quota basis），或基于固定百分比（Fixed Share Ratio）按照负载平衡的策略进行分配。

  案例说明:

  1号矿池组的比例配额（Quota）为"1"，其中有2个矿池地址。2号矿池组的比例配额为"2"，其中只有1个矿池地址。
  - 两个矿池组的算力分配为1：2。
  - 分配到2号矿池组的算力始终会是分配到1号的两倍。
  - 如果1号矿池组中的第一个矿池地址不可用，BOSminer将会自动切换到1号矿池组中的第二个矿池地址。
  
  基于固定百分比（Fixed Share Ratio）和基于比例配额（Quota basis）的算力分配模式不可以同时使用！
  在矿池组比例配额为1：1的情况下，就相当于设置0.5（50%）的固定百分比。 即对半分配发送到两个矿池组的算力。

  Configuration is available through web GUI (*Miner -> Configuration*) or in the configuration
  file ``/etc/bosminer.toml``.

  Example of two groups and multiple pools:

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

With this setup, the work will be split between the two groups in ratio 1:2. By default, the miner
will be mining on the first pool from the group "MyGroup1" and on the one pool defined in the group
"MyGroup2". If the first pool in "MyGroup1" dies, the miner will be mining on the second pool from
the group "MyGroup1". Since a second pool url isn't specified for "MyGroup2", nothing will be done
if the pool in "MyGroup2" fails.

*******************
Hash Chain Settings
*******************

Optional configuration for overriding the default settings for all hash chains. This allows the
users to control the frequency and voltage of each hash chain and allows them to turn AsicBoost o
n and off. While autotuning is enabled, these settings are ignored. The global hash chain settings
can also be overridden by per-chain settings.

Configuration is available through web GUI (*Miner -> Configuration*) or in the configuration file ``/etc/bosminer.toml``.

The syntax is the following:

  ::

     [hash_chain_global]
     asic_boost = true
     frequency = 650.0
     voltage = 8.8

  * *asic_boost* - Enable or disable AsicBoost support (default=true)
  * *frequency* - Set default chip frequency in MHz for all hash chains (default=650.0)
  * *voltage* - Set default voltage in V for all hash chains (default=8.8)

The syntax for per-chain settings is the following:

  ::

     [hash_chain.6]
     frequency = 650.0
     voltage = 8.8

  * *[hash_chain.6]* - Override the global settings for hash chain '6'
  * *frequency* - Override the global chip frequency in MHz for hash chain '6' (default='hash_chain_global.frequency')
  * *voltage* - Override the global voltage in V for hash chain '6' (default='hash_chain_global.voltage')

***************************
Temperature and Fan Control
***************************

Temperature Control Mode
========================

  Braiins OS+ supports automatic temperature control (using `PID controller <https://en.wikipedia.org/wiki/PID_controll>`__).
  The controller can operate in one of three modes:

  -  **Automatic** - Miner software tries to regulate the fan
     speed so that miner temperature is approximately at the target
     temperature (which can be configured). The allowed temperature range
     is 0-200 degree Celsius.
  -  **Manual** - Fans are kept at a fixed, user-defined speed,
     no matter the temperature. This is useful if you have your own way of
     cooling the miner or if the temperature sensors don’t work. Allowed
     fan speed is 0%-100%. The control unit monitors only hot and dangerous temperatures.
  -  **Disabled** - **WARNING**: this may damage the device because no control is done!

  The temperature control mode can be changed in the *Miner -> Configuration* page or in the configuration file ``/etc/bosminer.toml``.

  **Warning**: misconfiguring fans (either by turning them off or to a
  level that is too slow, or by setting the target temperature too high)
  may irreversibly **DAMAGE** your miner.

默认温度限制（Default temperature limits）
==========================

  设置默认温度限制是为了防止矿机的过热及损坏。

  * **目标温度（Target temperature）** 指矿机会尝试保持的温度（*默认值* 为 **89°C**）。
  * **过热温度（Hot temperature）** 指风扇会开始以100%转速运行的阈值温度（*默认值* 为 **100°C**）。
  * **危险温度（Dangerous temperature）** 指为防止矿机的过热及损坏，BOSminer会自动关闭的阈值温度（*默认值* 为 **110°C**）。

  默认温度限制的温度值可以在 *Miner（矿机）  -> Configuration（配置）* 页面中，或在 ``/etc/bosminer.toml`` 配置文件中调整。
  
在 ``bosminer.toml`` 配置文件中的温度和风扇控制
==============================================================

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

Fan operation
=============

  1. Once temperature sensors are initialized, fan control is enabled. If
     temperature sensors are not working or they read out a temperature of
     0, fans are automatically set to full speed.
  2. If the current mode is “fixed fan speed”, the fan is set to a given
     speed.
  3. If the current mode is “automatic fan control”, the fan speed is
     regulated by temperature.
  4. In case the miner's temperature is above the *HOT temperature*, fans are set to
     100% (even in “fixed fan speed” mode).
  5. In case the miner's temperature is above the *DANGEROUS temperature*, BOSminer
     shuts down (even in “fixed fan speed” mode).

******************
Tuning Adjustments
******************

Tuning can be configured either via web GUI or in the configuration file ``/etc/bosminer.toml``.

To make a configuration change via web GUI, enter the *Miner -> Configuration* menu and edit
the *Autotuning* section.

To make a configuration change in the configuration file, connect to the miner via SSH and edit
the file ``/etc/bosminer.toml``. The syntax is the following:

  ::

     [autotuning]
     enabled = true
     psu_power_limit = 1200

The *enabled* line can hold values *true* for enabled autotuning, or *false* for disabled autotuning.
The *psu_power_limit* can hold numeric values (min. 100 and max. 5000), representing the PSU power
limit (in Watts) for three hashboards and the control board.

Alternatively, it's possible to turn on autotuning automatically after the installation finishes
specifying the ``--power-limit POWER_LIMIT``   argument in the installation command.

In order to change power limit on multiple devices, you can use
our configuration spreadsheet that will will generate commands for different use cases.

The spreadsheet is available `here <https://docs.google.com/spreadsheets/d/1H3Zn1zSm6-6atWTzcU0aO63zvFzANgc8mcOFtRaw42E>`_

************
SSH password
************

You can set the miner’s password via SSH from a remote host by running
the below command and replacing *[newpassword]* with your own password.

  * Note: Braiins OS+ does **not** keep a history of the commands executed.

  .. code:: bash

     ssh root@[miner-hostname-or-ip] 'echo -e "[newpassword]\n[newpassword]" | passwd'

To do this for several hosts in parallel you could use
`p-ssh <https://linux.die.net/man/1/pssh>`__.

****************
MAC & IP address
****************

By default, the device’s MAC address stays the same as it is inherited
from firmware (stock or Braiins OS) stored in the device (NAND). That way, once
the device boots with Braiins OS+, it will have the same IP address as it
had with the factory firmware.

Alternatively, you can specify a MAC address of your choice by modifying
the ``ethaddr=`` parameter in the ``uEnv.txt`` file (found in the first
FAT partition of the SD card).
