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
简介
#####

Braiins OS+ 是专为ASIC矿机设计的增强性操作系统。它在已经相当可靠的 `Braiins OS <https://zh.braiins-os.com/community-edition>`_ 社区版的基础上，额外提供独有的矿机自动调整算法。当用户能提供最大允许的功耗（瓦数）时，系统将自动优化挖矿过程，让矿机算力最大化。这一过程具有输入普适性，让您能基于经济上的考虑，对矿机进行最大化效率或最高哈希率的优化。内部测试显示，在蚂蚁矿机S9上使用Braiins OS+能让矿机能效比降到70 J/THs，且在低功耗设定下，这一数值还可能降到更低。同时，增加矿机输入功率，也能提升矿机算力20%或更高（与蚂蚁矿机S9原厂固件，在94 J/THs的能耗比下只有13.5TH/秒相比）。

目前Braiins OS+ 支持的设备，有比特大陆的蚂蚁矿机S9，S9i以及S9j。对蚂蚁矿机S17的支持也将很快推出。

********
特性
********

 * 具有能提高算力或效率的高级自动调整优化功能
 * 开源的操作系统
 * 完整应用改进数据效率和防止算力劫持的阶层Stratum V2协议
 * 内置由Rust语言从头编写的CGminer的代替品——BOSminer
 * 快速开机（5-7秒）
 * 未定义行为不会导致莫名其妙的死机
 * 批量安装
 * 基于标准opkg包系统的自动更新
 * 完整的风扇自定义控制（并支持浸没式冷却）
 * 高级监控系统保证您的矿机健康并预防其他问题

*******************
技术支持与联系方式
*******************

我们的开发和客服团队非常乐意解答您的疑惑。

您也可以加入我们的电报群组：


  * `英文群组 <https://t.me/BraiinsOS>`_
  * `俄语群组 <https://t.me/BraiinsOS_RU>`_
  * `中文群组 <https://t.me/BraiinsOS_ZH>`_ （我们的微信群请详询电报群内客服邀请加入）

 您也可以向我们的客服团队 `发送VIP请求 <https://slushpool.kayako.com/en-us/conversation/new/11>`_ 。


*********
更新日志
*********

20.04
---------------------------

本次发布的更新解决了大多数用户遇到的一些问题，例如安装/卸载的困难以及S9矿机上I2C控制器的一个主要问题。同时，我们也提供了固件的预先发行版了，现在使用*BOS*工具箱您就能启用它。

  * 在所有类型的矿机上

    * 【特性】对重联的支持——我们在固件中应用了（`client.reconnect`）重联命令（在阶层Stratum V1协议中)，和阶层Stratum V2协议的重联消息
    * 【特性】改进了安装/卸载的进程（或者说**upgrade2bos**和**restore2factory**）（从原厂固件过渡到Braiins OS等情况同理）：
    * 【特性】通过命令行命令（`--pool-user`）可以自定义矿池用户了
    * 【特性】您原厂矿机固件中之前的矿池设置，现在会自动移植到BOSminer的配置中了。您也可以使用（`--no-keep-pools`）命令停用自动移植。
    * 【特性】we now provide binary form of **upgrade2bos** (based on pyinstaller) that contains the latest Braiins OS installation image
    * 【特性】similarly, **restore2factory** (based on pyinstaller) is now available in binary form and doesn't require any longer downloading/finding out the correct factory firmware.
    * 【特性】disk space and time consuming backup of the original firmware is now disabled by default (can be enabled by `--backup`)
    * 【特性】keeping host name while performing first time install is now driving by 2 options `--keep-hostname` and `--no-keep-hostname` allowing to force override and automatic hostname generation based on MAC address
    * 【特性】support for enabling/disabling nightly builds has been integrated into **bos** utility (and its legacy **miner** counterpart).
    * 【特性】system now provides **logs** covering **longer timespan** of **BOSminer** operation due to enabling **log rotation** and compression of '/var/log/syslog.old' when it is bigger than 32 KiB
    * 【BUG修复】SD card image now contains slushpool authority public key that was missing
    * 【BUG修复】rejection rate is now correctly being displayed
    * 【BUG修复】unknown stratum V1 messages received from the server are now being logged for diagnostics

  * 在蚂蚁矿机S9上
  
    * 【特性】Tuner status is now shown in the GUI. TUNERSTATUS API command was added.
    * 【BUG修复】some devices were experiencing random I2C controller bus lockups and would fail to communicate with hashboard power controllers connected to the shared I2C bus. We have found out that the cause was the Xilinx I2C controller core that we have integrated into the FPGA bitstream. We have switched to the I2C present in the SoC and the bitstream only routes the signal of the peripheral (IIC0) to corresponding FPGA pins.

20.03
---------------------------

  * 在所有类型的矿机上
      
    * 【特性】配置文件让用户能设定电源PSU的功率限制，自动调整算法会在设定的限制下，最大化矿机的能耗比每瓦算力。

  * 在蚂蚁矿机S9上

    * 【特性】基于用户设定功率限制的自动调整功能。

************
已知问题
************

以下列出了已发布版本中存在的已知问题。

20.03 (更新于 3/30/2020)
-------------------------

  * 矿机网页图形界面（GUI)

   * 算力图表中的平均名义哈希率（Average Nominal Hashrate）参照线数值不准确。此问题只有运行的哈希链数量少于3个时才会发生。
   * 拒绝率（Rejection ratio）被乘了100。例如，当拒绝率实际是0.1%时，显示的是10%。

  * 配置
  
    * 用SD卡安装后，系统可能会在矿机/配置（Configuration）项中，报错缺少阶层Stratum V2协议的验证密钥， 
      (Error: missing upstream authority key for securing stratum2+tcp connection in pool")。
      用户可以在配置项中，或直接在 ``/etc/bosminer.toml`` 文件内调整连接（包括密钥）。
