.. toctree::
   :hidden:
   :maxdepth: 3
   :caption: Table of Contents:
   :glob:

   self
   Setup/index*
   Configuration/index*
   Basic User's Guide/index*
   Development/index*

---------------

.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>


#####
简介
#####

Braiis OS是用于ASIC矿机的完全开源操作系统。在2018年，Braiins OS是首个启用显性AsicBoost的矿机固件。它现在也支持新的挖矿协议——阶层Stratum V2协议的完整应用。此外，Braiins OS与我们新的挖矿软件组件BOSminer配合运作，BOSminer是用Rust语言从头编写，对过时的CGminer的替代品。

目前支持的设备有比特大陆蚂蚁矿机S9, S9i, S9j, S17, S17 Pro, S17+, T17 和 T17+。在不久的将来计划发布对蚂蚁矿机S17e, T17e 和 Whatsminer M20S 的支持。

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
 * 自动升级机制

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
  

 您也可以向我们的客服团队 `发送VIP请求 <https://help.slushpool.com/en/support/tickets/new>`_ 。


*********
更新日志
*********

20.09
---------------------------

本次发布包括对蚂蚁矿机S17和S17 Pro的支持，和对S9系列的维护性更新。

* 在所有类型的矿机上

  * 【特性】如果芯片温度传感器失效，芯片温度将由运算版温度估算得出
  * 【特性】如果发送请求到域名，将整合DNS罩（DNSmasq）到本地DNS缓存以节约资源
  * 【BUG修复】修复了设置矿池组按固定比例（Fixed Share Ratio）或百分比（Quota）分配算力不正确的问题
  * 【特性】我们改进了对不稳定矿池的退避算法（Backoff Algorithm），如果矿池无误运行一小时才能被视为稳定


* 蚂蚁矿机S17

  * 【备注】用于蚂蚁矿机S17系列上的Braiins OS固件，没有区分S17和S17 Pro，这两种矿机基本没大区别。

20.06
---------------------------

本次更新的发布旨在通过应用新功能和修复关键问题，改进Braiins OS和BOS工具箱的可用性。 

  * 在所有类型的矿机上

    * 【解决方法】新增对基于yyimp的矿池（例如prohashing）的支持，这种矿池在错误地发送以'0x'开头的，不符合比特币改进提案BIP-310规范的版本滚动掩码
    * 【特性】新增对阶层Stratum V1协议中的密码的支持，它们被一些矿池用于算法切换和其他用途
    * 【特性】自动升级机制的应用。矿机将定期检查，并在发现有升级可用后自动升级Braiins OS的新版本。在从原厂固件切换到Braiins OS时，这一功能将默认启用。但是在从Braiins OS或Braiins OS+的旧版本升级的情况下，必须手动启动这一功能
    * 【特性】通过日志轮转工具（Lograte）改进系统日志。系统日志将被自动压缩并保存在设备的NAND上，从而能保存更长时段的日志
    * 【特性】升级了BOS工具箱，现在它可以批量运行自定义命令了
    * 【BUG修复】使用SD卡方式安装映像刷矿机内置储存NAND时，现已能正确地从SD卡上迁移配置，而不是从内置储存NAND上的旧系统。
    * 【BUG修复】修复了在系统刷新缓冲区前关闭矿机时 *bosminer.toml* 文件为空的问题
    * 【BUG修复】IP Report按钮能正常使用了

  * 在蚂蚁矿机S9上

    * 【特性】我们切回使用赛灵思的I2C控制器核心进行与电压控制器的通信，并拓展了其用于噪声环境的故障过滤
    * 【特性】拓展了用于与计算芯片通信的串口输入引脚线（UART Rx）的故障过滤

20.04
---------------------------

本次发布的更新解决了大多数用户遇到的一些问题，例如安装/卸载的困难以及S9矿机上I2C控制器的一个主要问题。同时，我们也提供了固件的预先发行版了，现在使用 **BOS** 工具箱您就能启用它。

  * 在所有类型的矿机上

    * 【特性】对重联的支持——我们在固件中应用了（`client.reconnect`）重联命令（在阶层Stratum V1协议中)，和阶层Stratum V2协议的重联消息
    * 【特性】改进了安装/卸载（**upgrade2bos**和**restore2factory**这两个进程）（从原厂固件过渡到Braiins OS等情况的进程同理）
    * 【特性】通过命令行命令（`--pool-user`）可以自定义矿池用户
    * 【特性】您原厂矿机固件中之前的矿池设置，现在会自动转移到BOSminer的配置中了。您也可以使用（`--no-keep-pools`）命令停用自动转移。
    * 【特性】我们现在提供（基于pyinstaller的）二进制格式的**upgrade2bos**进程，它内置有最新的Braiins OS安装映像文件
    * 【特性】同样提供的也有（基于pyinstaller的）二进制格式的**restore2factory**进程，且现在不需要去下载或找到合适的原厂固件了。 
    * 【特性】默认停用了又占地方又花时间的原厂固件备份，可以通过（`--backup`）命令恢复启用。
    * 【特性】首次安装中保留主机名（Host name）的功能，可以通过（`--keep-hostname`）和（`--no-keep-hostname`）这两个命令控制，从而能超控根据MAC地址自动生成主机名。 
    * 【特性】在网页端后台的**BOS**工具箱中（以及旧版**矿机**中），现已集成了对开启/关闭预先发布版的支持
    * 【特性】由于现在系统开启了 **日志轮替** 和对超过32KiB的'/var/log/syslog.old'旧系统记录文件进行自动压缩的功能， **BOSminer** 将能够提供的 **更长时间** 的 **系统日志** 。
    * 【BUG修复】SD卡固件中现已包含之前报错缺失的Slush Pool矿池验证公钥（阶层Stratum V2协议）
    * 【BUG修复】拒绝率现已显示正确值
    * 【BUG修复】从服务器收到的未知的阶层Stratum V1协议消息将保留日志作诊断用
    
  * 在蚂蚁矿机S9上
  
    * 【BUG修复】在一些矿机上有时会出现I2C控制器总线锁死的情况，从而导致与I2C控制器共享总线的运算板供电控制器出现不响应的问题。出现问题的原因是我们将赛灵思的I2C控制器核心，整合进了现场可编程逻辑门阵列（FPGA）的位元流。我们已在SoC上切换到了I2C总线，位元流只将外围信号（IIC0）引导到相应的FPGA针脚上。

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
