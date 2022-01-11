
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>


.. _manager:

###################
Braiins OS+管家
###################

.. contents::
  :local:
  :depth: 1

Braiins OS+管家是用于远程配置装有Braiins OS+固件矿机的云端管理和监控平台。

.. 注意::
   Braiins OS+管家不再开放新用户注册。

其数据通过阶层Stratum V2协议实现传送，相同于固件抽水的通道，并不会增加网络上的负载。

Braiins OS+管家中能设置运维管理单位“矿场”（以下通称矿场）。每个矿场都有独立ID。在装有Braiins OS+固件的矿机上绑定矿场ID后，矿机会每隔120秒传送性能数据到Braiins OS+管家。

每个矿场都可以有不同的配置。每当您更新并保存新配置，管家都会在下一次与矿机交换数据时，对矿场下每台矿机上的配置进行更新。介于是对每台矿机，我们 **强烈建议在同一矿场下最好只绑定同款矿机** ，不同款矿机要放在不同矿场下。

*************************
绑定矿机和矿场
*************************

要想绑定装有Braiins OS+固件的矿机到矿场，您需要：

  - 矿机上Braiins OS+固件版本为21.04或更新
  - 将矿场ID（bos_mgmt_id）与矿机绑定

按照以下步骤可以用BOS工具箱执行绑定操作。
**重要提醒：** 在使用以下的命令前，请 `在此 <https://zh.braiins.com/os/plus/download>`_ 下载最新版BOS工具箱 。

**安装/升级Braiins OS+固件并绑定矿场ID**

如果矿机上运行的Braiins OS+固件版本为21.04或更新，您在安装Braiins OS+时在安装命令后添加矿场ID选项（--bos-mgmt-id参数） 同时绑定矿场ID。

如果使用工具箱的GUI版本，在**Install**部分中填写**Farm-ID**选项并继续进行安装。如果使用工具箱的命令行版本，使用以下的命令在安装时设定矿场ID。用IP地址或者包含多个IP地址的文本文件（批量安装一个IP一行）替换下方命令中的“HOSTS”。用您矿场ID替换下方命令中“FARM_ID”。
   
::

    #Windows
    bos-toolbox.bat install --bos-mgmt-id FARM_ID HOSTS

    #Linux
    ./bos-toolbox install --bos-mgmt-id FARM_ID HOSTS

**在存在的Braiins OS+安装上绑定矿场ID**

如果矿机上运行的Braiins OS+固件版本为21.04或更新，您在安装Braiins OS+时在安装命令后添加矿场ID选项（--bos-mgmt-id参数） 同时绑定矿场ID。

如果使用工具箱的GUI版本，在**Update**部分中填写**Farm-ID**选项并继续进行安装。如果使用工具箱的命令行版本，使用以下的命令在安装时设定矿场ID。
用IP地址或者包含多个IP地址的文本文件（批量安装一个IP一行）替换下方命令中的“HOSTS”。用您矿场ID替换下方命令中“FARM_ID”。
 
::

    #Windows
    bos-toolbox.bat update --bos-mgmt-id FARM_ID HOSTS

    #Linux
    ./bos-toolbox update --bos-mgmt-id FARM_ID HOSTS

******************
矿场配置
******************

**矿工名设置**

为便于矿池辨别矿机矿工名，有关矿场下的矿机如何设置独立/统一矿工名，有四种不同的模式：

  - 矿场下每台矿机统一矿工名 (FARMNAME) - 矿场下所有矿机的算力都会归到以矿场名为矿工名的同一个大矿工名下。
  - 每台矿机独立矿工名 (FARMNAME_IP4) - 矿工名 = 矿场名 + 矿机IP地址的第四段
  - 每台矿机独立矿工名 (FARMNAME_IP3_IP4) - 默认 - 可选，矿工名 = 矿场名 + 矿机IP地址的第三段 + 矿机IP地址的第四段
  - 每台矿机独立矿工名 (FARMNAME_IP2_IP3_IP4) - 可选，矿工名 = 矿场名 + 矿机IP地址的第二段 + 矿机IP地址的第三段 + 矿机IP地址的第四段
  - 每台矿机独立矿工名 (DEVICE-ID / MANUAL OVERRIDE) - 通过点击矿工名旁边的齿轮可以给所选的设备选择矿工名。默认，在该模式下设备ID作为矿工名。

您也可以改变分字符，并使用`_`而不用`x`。但是，请您注意有些矿池不支持包含下划线的矿工名，并选择`_`作为分字符可以导致连接到该矿池的问题。
  
矿工名设置随时可改。

**挖矿配置** 

挖矿配置可以参考 `Braiins OS+固件技术文档中的“配置”（Configuration）部分 <https://docs.braiins.com/os/plus-zh/Configuration/index_configuration.html>`_ 某些情况例如某台矿机缺板运行需要单独配置，建议本地单独配置缺板的矿机而不是用Braiins OS+管家在云端批量配置。除此之外像自动调整、矿机温控系统目标温度或者动态降功耗等这些设置，都是可以批量配置的。

默认情况下，矿机使用各项设置的默认值和创建矿场时的矿池，一般不需要改动各项设置。

点击保存按钮后，新配置将立即被传到矿场下的所有矿机上。

**本地更改配置**

绑定矿场ID之后的矿机上的配置，始终会由云端管家内的矿场配置为主，任何本地更改都会被矿场配置覆盖。如需在本地单独更改配置，您需要将矿机与矿场ID解绑。

******************************
解绑矿机与矿场
******************************

如需将矿机与矿场解绑，并按需自行对矿机进行单独配置，可以通过删除所需解绑的矿机上的bos_mgmt_id文件。 也可以使用BOS工具箱对多台矿机进行批量操作，如下所示：

如果使用工具箱的GUI版本，在**Command**部分中的**Command**选项填写以下的命令：

::

    rm /etc/bos_mgmt_id && /etc/init.d/bosminer restart

如果使用工具箱的命令行版本，请使用以下的命令：
::

    #Windows
    bos-toolbox.bat command -o HOSTS "rm /etc/bos_mgmt_id && /etc/init.d/bosminer restart"
    
    #Linux
    ./bos-toolbox command -o HOSTS "rm /etc/bos_mgmt_id && /etc/init.d/bosminer restart"

***************
疑难解答
***************

**1. 检查矿机上是否为Braiins OS+固件21.04或更新版**

  - 图形界面：矿机网页后台底部
  - 命令行界面：SSH欢迎界面会显示 

**解决方案：** 如果矿机上的Braiins OS+固件为旧版，您需要先更新

**2. 检查矿机和矿场ID是否绑定成功**

图形界面：

  - 状态（Status) -> 总览(Overview） -> 矿机（Miner）
  - 检查*BOS Management ID*项中是否有正确的矿场ID
  - 如找不到上述内容，矿机和矿场ID就是没绑定好

命令行界面：

  - `cat /etc/bos_mgmt_id`
  - 该命令会输出矿场ID

**解决方案**: 如矿场ID未设置或有误，请重新设置

**3. 重启您的设备**

还是绑定不成功？请重启矿机试试。

  - 图形界面： 系统（System） -> 重启（Reboot） -> 进行重启（Perform Reboot）
  - 命令行界面: `reboot`

**4. 联系客服**

如以上办法都没用，您可以 `创建一份客服工单 <https://help.slushpool.com/zh-CN/support/tickets/new>`_ 。

为方便了解问题，请在工单中包含以下信息：

  - **硬件ID** （状态（System) -> 总览（Overview））
  - **系统日志** （状态（System） -> 系统日志（System Log））
