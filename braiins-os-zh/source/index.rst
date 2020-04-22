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

Braiis OS是用于ASIC矿机的完全开源操作系统。在2018年，Braiins OS是首个启用显性AsicBoost的矿机固件。它现在也支持新的挖矿协议——阶层Stratum V2协议的完整应用。此外，Braiins OS与我们新的挖矿软件组件BOSminer配合运作，BOSminer是用Rust语言从头编写，对过时的CGminer的替代品。

目前Braiins OS 支持的设备，有比特大陆的蚂蚁矿机S9，S9i以及S9j。对蚂蚁矿机S17的支持也将很快推出。

********
特性
********

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
  * `西班牙语群组 <https://t.me/BraiinsOS_ES>`_
  * `波斯语群组 <https://t.me/BraiinsOS_SlushPool_FA>`_
  * `中文群组 <https://t.me/BraiinsOS_ZH>`_ （我们的微信群请详询电报群内客服邀请加入）
  

 您也可以向我们的客服团队 `发送VIP请求 <https://slushpool.kayako.com/en-us/conversation/new/11>`_ 。


*********
更新日志
*********

20.03
---------------------------

更新历史见Github上的 `WHATSNEW.MD <https://github.com/braiins/braiins/blob/master/braiins-os/whatsnew.md>`_ 文件。

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
