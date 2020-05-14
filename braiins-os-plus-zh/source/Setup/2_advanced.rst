##############
高级指南
##############

.. contents::
	:local:
	:depth: 1

********
工具及使用场景一览
********

有很多工具、安装包以及脚本都可以用于管理Braiins OS+。请使用下方的树状列表查找最适合您的情况：

 * 安装Braiins OS+
 
  * 使用BOS+工具箱 （:ref:`bosbox_install`）
  * 使用网页端后台方式安装包 （:ref:`web_package_install`）
  * 使用SD卡刷 （:ref:`sd_install`）
  * 使用SD卡刷和矿机工具（Miner tool） （:ref:`miner_nand_install`）
  * 使用SSH脚本（:ref:`ssh_package_install`）
  
 * 升级Braiins OS+
 
  * 使用BOS+工具箱 （:ref:`bosbox_update`）
  * 使用OPKG批量包 （:ref:`opkg_update`）
  * 使用SYSUPGRADE（系统升级）包 （:ref:`sysupgrade_switch_braiinsplus`）
  * 使用BOS2BOS（从Braiins OS升级到Braiins OS其他版本的）脚本 （:ref:`bos2bos`）
  
 * 降级到Braiins OS社区版（不带自动调整功能）
 
  * 使用SYSUPGRADE（系统升级）包 （:ref:`sysupgrade_switch_braiinsos`）
  * 使用BOS2BOS（从Braiins OS升级到Braiins OS其他版本的）脚本 （:ref:`bos2bos`）
  
 * 升级到Braiins OS+（带自动调整功能）
 
  * 使用OPKG批量包 （:ref:`opkg_switch_to_braiinsplus`）
  * 使用SYSUPGRADE（系统升级）包 （:ref:`sysupgrade_switch_braiinsplus`）
  * 使用BOS2BOS（从Braiins OS升级到Braiins OS的其他版本）脚本 （:ref:`bos2bos`）
  
 * 重置到Braiins OS初始版本（矿机首次安装Braiins OS的版本） - 恢复出厂设置
 
  * 使用OPKG批量包 （:ref:`opkg_factory_reset`）
  * 使用SD卡刷 （:ref:`sd_factory_reset`）
  * 使用矿机工具（Miner tool） （:ref:`miner_factory_reset`）
  * 使用BOS2BOS（从Braiins OS升级到Braiins OS其他版本的）脚本（:ref:`bos2bos`）
  
 * 卸载Braiins OS+
 
  * 使用BOS+工具箱 （:ref:`bosbox_uninstall`）
  * 使用SSH脚本 （:ref:`ssh_package_uninstall`）

.. _bosbox:

***************
BOS+工具箱
***************

BOS+工具箱能让用户轻松安装，卸载，升级，检测以及配置Braiins OS+。它还有批量模式，让您对矿场的管理更得心应手。我们推荐您使用批量模式管理矿机。 

=====
如何使用
=====

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS+工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址， **一个IP地址一行** ！保存文本文件后，再将文件后缀从".txt"改为".csv"。并确定此文件和BOS+工具箱都放在同一路径下（同一文件夹中）。 
  * 再按下面相应部分的步骤进行操作

=======================================
BOS+工具箱的特性及优缺点
=======================================

  + 远程安装Braiins OS+
  + 远程升级Braiins OS+
  + 远程卸载Braiins OS+ 
  + 远程配置Braiins OS+
  + 扫描网络中的矿机
  + 安装Braiins OS+时默认自动转移原厂固件中的配置（也可以设置不转移）
  + 卸载Braiins OS+时默认自动转移现有配置到原厂固件（也可以设置不转移）
  + 可自定义进程的参数
  + 安装Braiins OS+后默认自动开启矿机自动调整功能（默认功率限制1420瓦）
  + 批量模式让管理大量矿机也能得心应手
  + 使用简单，容易上手
  
  - 不支持SSH功能被锁住的矿机

.. _bosbox_install:

======================================
使用BOS+工具箱安装Braiins OS+
======================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS+工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址， **一个IP地址一行** ！保存文本文件后，再将文件后缀从".txt"改为".csv"。并确定此文件和BOS+工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS+工具性的实际路径（文件夹地址），替换下方命令中的 *FILE_PATH_TO_BOS+_TOOLBOX* 。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-plus-toolbox.exe install ARGUMENTS HOSTNAME
    
    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-plus-toolbox install ARGUMENTS HOSTNAME

    **请注意：** *当在Linux系统中使用BOS+工具箱时，您需要先使用以下命令让BOS+工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-plus-toolbox

您可以使用下方的 **参数** 调整安装进程：

**重点：** 
当您在 **单台矿机** 上安装Braiins OS+时，需要使用 *HOSTNAME* 这个参数 （IP地址）。
当您在多台矿机上 **批量** 安装Braiins OS+时，请 **不要** 使用HOSTNAME这个参数，而是使用 *--batch BATCH* 这个参数。

====================================  ============================================================
参数                                   描述
====================================  ============================================================
-h, --help                            显示帮助信息并退出
--batch BATCH                         指定"listOfMiners.csv"（矿机主机IP地址列表）文件
--backup                              在进行升级前备份矿机
--no-nand-backup                      跳过对矿机内置储存NAND的备份（仍备份矿机配置）
--pool-user [POOL_USER]               为默认矿池设置用户名（Username）和矿工名（Workername）
--psu-power-limit [PSU_POWER_LIMIT]   设置（以瓦为单位）的电源功率限制
--no-keep-network                     不保留（转移）矿机的原网络配置（在使用DHCP自动分配IP的情况下）
--no-keep-pools                       不保留（转移）矿机的原矿池配置
--no-keep-hostname                    不保留（转移）矿机的原主机名（Hostname）并根据矿机MAC地址生成一个新的 
--keep-hostname                       强制保留（转移）矿机的原主机名
--no-wait                             直到系统完全更新完毕不等待
--dry-run                             执行所有的更新步骤但不实际进行更新
--post-upgrade [POST_UPGRADE]         指定stage3.sh脚本文件目录
--install-password INSTALL_PASSWORD   设置安装的SSH密码
====================================  ============================================================

**安装命令和参数使用示例如下：**

::

  bos-toolbox.exe install --batch listOfMiners.csv --psu-power-limit 1200 --install-password admin

解释：上方的命令和参数，会将Braiins OS+安装到在 *listOfMiners.csv* （矿机IP地址列表）中列出的矿机上，并设置列表中所有矿机的输入功率限制为1200瓦。当矿机要求输入SSH密码时，命令将自动输入 *admin* 这个密码。

.. _bosbox_update:

=====================================
使用BOS+工具箱升级Braiins OS+
=====================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS+工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址， **一个IP地址一行** ！保存文本文件后，再将文件后缀从".txt"改为".csv"。并确定此文件和BOS+工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS+工具性的实际路径（文件夹地址），替换下方命令中的 *FILE_PATH_TO_BOS+_TOOLBOX* 。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-plus-toolbox.exe update ARGUMENTS HOSTNAME

    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-plus-toolbox update ARGUMENTS HOSTNAME

    **请注意：** *当在Linux系统中使用BOS+工具箱时，您需要先使用以下命令让BOS+工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-plus-toolbox

您可以使用下方的 **参数** 调整更新进程：

**重点：** 
当您在 **单台矿机** 上安装Braiins OS+时，需要使用 *HOSTNAME* 这个参数 （IP地址）。
当您在多台矿机上 **批量** 安装Braiins OS+时，请 **不要** 使用HOSTNAME这个参数，而是使用 *--batch BATCH* 这个参数。

====================================  ============================================================
参数                                   描述
====================================  ============================================================
--h, --help                           显示帮助信息并退出
--batch BATCH                         指定"listOfMiners.csv"（矿机主机IP地址列表）文件
-p PASSWORD, --password PASSWORD      矿机密码
-i, --ignore                          忽略错误
====================================  ============================================================


**更新命令和参数使用示例如下：**

::

  bos-toolbox.exe update --batch listOfMiners.csv

解释：上方的命令和参数，会在有新固件更新可用的情况下，对在 *listOfMiners.csv* （矿机IP地址列表）中列出矿机上的Braiins OS+进行更新。

.. _bosbox_uninstall:

========================================
使用BOS+工具箱卸载Braiins OS+
========================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS+工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址，一个IP地址一行！（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）中可以进行查询）。保存文本文件后，再将文件后缀从".txt"改为".csv"。确定此文件和BOS+工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS+工具性的实际路径（文件夹地址），替换下方命令中的*FILE_PATH_TO_BOS+_TOOLBOX*。执行命令，切换到路径。 ::
  
      cd FILE_PATH_TO_BOS+_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-plus-toolbox.exe uninstall ARGUMENTS HOSTNAME

     在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-plus-toolbox uninstall ARGUMENTS HOSTNAME
      
    **请注意：** *当在Linux系统中使用BOS+工具箱时，您需要先使用以下命令让BOS+工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-plus-toolbox

您可以使用下方的 **参数** 调整卸载进程：

**重点：** 
当您在 **单台矿机** 上安装Braiins OS+时，需要使用 *HOSTNAME* 这个参数 （IP地址）。
当您在多台矿机上 **批量** 安装Braiins OS+时，请 **不要** 使用HOSTNAME这个参数，而是使用 *--batch BATCH* 这个参数。

====================================  ============================================================
参数                                   描述
====================================  ============================================================
-h, --help                            显示帮助信息并退出
--batch BATCH                         指定"listOfMiners.csv"（矿机主机IP地址列表）文件
--factory-image FACTORY_IMAGE         指定原厂更新固件文件路径或URL地址（默认是：
                                      Antminer-S9-all-201812051512-autofreq-user-Update2UBI-
                                      NF.tar.gz）
====================================  ============================================================

**卸载命令和参数使用示例如下：**

::

  bos-toolbox.exe uninstall --batch listOfMiners.csv

解释：上方的命令和参数，会卸载在 *listOfMiners.csv* （矿机IP地址列表）中列出矿机上的Braiins OS+，并重装原厂固件（Antminer-S9-all-201812051512-autofreq-user-Update2UBI-NF.tar.gz）。

.. _bosbox_configure:

===========================================
使用BOS+工具箱配置Braiins OS+
===========================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS+工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址，一个IP地址一行！（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）中可以进行查询）。保存文本文件后，再将文件后缀从".txt"改为".csv"。确定此文件和BOS+工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS+工具性的实际路径（文件夹地址），替换下方命令中的*FILE_PATH_TO_BOS+_TOOLBOX*。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-plus-toolbox.exe config ARGUMENTS ACTION TABLE

    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-plus-toolbox config ARGUMENTS ACTION TABLE
      
    **请注意：** *当在Linux系统中使用BOS+工具箱时，您需要先使用以下命令让BOS+工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-plus-toolbox

您可以使用下方的 **参数** 调整配置进程：

====================================  ============================================================
参数                                   描述
====================================  ============================================================
-h, --help                            显示帮助信息并退出
-u USER, --user USER                  矿机网页端后台用户名
-p PASSWORD, --password PASSWORD      矿机网页端后台密码
-c, --check                           不写入的试运行检查
-i, --ignore                          忽略错误
====================================  ============================================================

您必须 **至少选择使用** 下方的 **动作** 中的一个来调整配置进程：

====================================  ============================================================
参数                                   描述
====================================  ============================================================
load                                  加载矿机的目前配置到一个CSV文件中

save                                  保存CSV文件中的矿机设定到矿机（但尚未应用设定）

apply                                 应用之前从CSV文件复制（保存）到矿机上的设定
                                      
save_apply                            保存并应用之前从CSV文件复制（保存）到矿机上的设定
====================================  ============================================================

**配置命令和参数使用示例如下：**

::

  bos-toolbox.exe config --user root load listOfMiners.csv
  
  #把矿机上的配置加载到CSV文件中后，可以通过表格软件编辑配置（如MS Office Excel，LibreOffice Calc等)
  
  bos-toolbox.exe config --user root save_apply listOfMiners.csv

解释：上方的第一个命令和参数，会（使用*root*这个后台用户名）提取在 *listOfMiners.csv* （矿机IP地址列表）中列出矿机的配置，并将这些配置保存到一个CSV文件中。然后您可以打开并编辑这个CSV文件，调整矿机的配置。您改动好之后，就可以用上方的第二个命令和参数，将配置复制（保存）到矿机上，并应用新配置。

.. _bosbox_scan:

======================================================
使用BOS+工具箱扫描网络并发现矿机
======================================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS+工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址，一个IP地址一行！（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）中可以进行查询）。保存文本文件后，再将文件后缀从".txt"改为".csv"。确定此文件和BOS+工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS+工具性的实际路径（文件夹地址），替换下方命令中的*FILE_PATH_TO_BOS+_TOOLBOX*。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-plus-toolbox.exe discover ARGUMENTS

    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-plus-toolbox discover ARGUMENTS
      
    **请注意：** *当在Linux系统中使用BOS+工具箱时，您需要先使用以下命令让BOS+工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-plus-toolbox

您可以使用下方的 **参数** 调整网络扫描和矿机发现进程：

====================================  ============================================================
参数                                   描述
====================================  ============================================================
-h, --help                            显示帮助信息并退出
====================================  ============================================================

您必须 **至少选择使用** 下方的 **参数** 中的一个来调整网络扫描和矿机发现进程：

====================================  ============================================================
参数                                   描述
====================================  ============================================================
scan                                  主动扫描提供的IP地址范围
listen                                监听矿机识别广播（当按下IP report键时）
====================================  ============================================================

**网络扫描和矿机发现命令和参数使用示例如下：**

::

  bos-toolbox.exe discover scan 10.10.10.0/24

解释：上方的命令和参数，会扫描从10.10.10.0到10.10.10.255这个范围的IP地址，并列出找到的矿机及其相应的IP地址。

.. _web_package:

***********
网页端后台方式安装包
***********

如果您使用的是2019年前的原厂固件，您从矿机的网页端后台，使用Braiins OS+的网页端后台方式安装包，即可用直接升级Braiins OS+。使用的是其他基于原厂固件的第三方固件的情况下也应该是同理的。由于2019年后发布的原厂固件，对网页端后台升级采取了固件签名认证来防止安装第三方固件，所以Braiins OS+的网页端后台方式安装包就无法用于对2019年后发布的原厂固件的升级。

=====
如何使用
=====

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **网页端后台方式安装包** 。
  * 再按下方步骤进行操作

=======================================
此安装方式的特性和优缺点：
=======================================

  + 无需额外工具就能直接用Braiins OS+替换调原厂固件
  + 默认自动转移网络配置
  + 默认自动转移矿池URL地址，用户名及密码
  + 默认自动开启矿机自动调整功能（默认功率限制1420瓦）
  
  - 不支持升级2019年及之后发布的原厂固件
  - 不支持配置安装（比如始终会自动转移网络配置）
  - 不支持批量安装（除非您自己写脚本）

.. _web_package_install:

=====================================
使用网页端后台方式安装包安装Braiins OS+
=====================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **网页端后台方式安装包** 。
  * 登陆您矿机的网页端后台，点击 *System（系统） -> Upgrade（升级）*。
  * 上传您下载的安装包，并刷入固件映像。

.. _sd:

*************
SD卡方式安装映像
*************

如果您使用的是2019年前的原厂固件，您只能通过SD卡刷的方法来安装Braiins OS。因为从2019年起的原厂固件为了防止第三方固件的使用，封锁了SSH连接并在网页端后台升级刷固件时要求验证签名。

=====
如何使用
=====

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **SD卡方式安装映像** 。
  * 再按下方步骤进行操作

=======================================
此安装方式的特性和优缺点：
=======================================

  + 用Braiins OS+替换锁定SSH的原厂固件
  + 默认使用内置储存NAND中的网络配置 (可禁用, 见下方的 *网络设置* 部分)
  + 默认自动开启矿机自动调整功能（默认功率限制1420瓦）
  
  - 不支持转移之前的矿池URL，用户名及密码
  - 不支持批量安装
  
.. _sd_install:

=================================
使用SD卡方式安装映像安装Braiins OS+
=================================

 * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **SD卡方式安装映像** 。
 * 将下载的映像烧录到SD卡上（例如使用像 `Etcher <https://etcher.io/>`_ 之类的烧录软件）。*请注意：光复制到SD卡上是不够的，必须用软件刷到卡上！*
 * 调整跳线，让矿机从SD卡启动（而不是从NAND内存），如下所示。

  .. |pic1| image:: ../_static/s9-jumpers.png
      :width: 45%
      :alt: S9 跳线

  .. |pic2| image:: ../_static/s9-jumpers-board.png
      :width: 45%
      :alt: S9 跳线板

  |pic1|  |pic2|

 * 将SD卡插到矿机上，开机。
 * 过一会，您就应该能通过设备的IP地址进到Braiins OS+界面了。
 * *[可选操作]：* 您也可以将Braiins OS+从SD卡刷到内置储存（NAND）上。具体请详见 :ref:`sd_nand_install`这一部分的内容。

.. _sd_network:

================
网络配置
================
 
 默认情况下，当从SD卡启动Braiins OS+时，将使用内置储存NAND上的网络配置置。此特性可以按照以下步骤禁用：

  * 加载SD卡上的第一个FAT格式的分区
  * 打开uEnv.txt文件并插入下方的参数 (注意空行，一条参数一行）

  ::

    cfg_override=no

对在网络中找不到（无法发现）矿机的矿工来说，禁用原始网络配置会很有帮助（比如NAND中原静态IP地址不在局域网地址范围内）。禁用之后，则使用DHCP动态IP地址。

.. _sd_nand_install:

============
将固件刷到矿机内置储存NAND上
============

您也可以将SD卡上的Braiins OS+刷到矿机内置储存NAND上。有两种方式可选：
  * 在您矿机的网页端后台中，点击 *System（系统） -> Install current system to device (NAND)（安装当前系统到矿机（NAND）上）*
  * 或通过SSH，使用 *miner（矿机）* 工具 —— 请按指南中 ref:`miner_nand_install` 的部分进行

.. _sd_factory_reset:

=======================================
使用SD卡方式安装映像对Braiins OS+恢复出厂配置
=======================================

您可以按下方步骤恢复出厂配置：You can do a factory reset, by following the steps bellow:

  * 加载SD卡上的第一个FAT格式的分区
  * 打开uEnv.txt文件并插入下方的参数 (注意空行，一条参数一行）
  ::

    factory_reset=yes

.. _ssh_package:

****************************
远程（SSH）方式安装包
****************************

您可以使用 *远程（SSH）方式安装包* 安装或卸载Braiins OS+。由于此方法需要用到Python设置，我们并不推荐使用此方法。您最好使用BOS+工具箱。

=====
如何使用
=====

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **远程（SSH）方式安装包** 。
  * 再按下方步骤进行操作

=======================================
此安装方式的特性和优缺点：
=======================================

  + 远程安装Braiins OS+ 
  + 远程卸装Braiins OS+
  + 在安装Braiins OS+时，自动转移原厂固件的完整配置到Braiins OS+上（可自选）
  + 在卸载Braiins OS+时，自动转移Braiins OS+固件的完整配置到原厂固件上（可自选）
  + 可自定义安装/卸载过程的参数
  + 在安装Braiins OS+后，t默认自动开启矿机自动调整功能（默认功率限制1420瓦）
  
  - 不支持批量安装/卸载（除非您自己写脚本）
  - 配置过程耗时
  - 不支持SSH远程功能被锁住的矿机

.. _ssh_package_environment:

=========================
准备环境
=========================

首先，您需要准备Python环境。请按下方步骤操作：

* *（仅在Windows上作这一步）* 从 `微软商店 <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_ 下载安装 *“Ubuntu for Windows 10“* 。
* 在命令行终端中运行以下命令：

*(Note that the commands are compatible with Ubuntu and Ubuntu for Windows 10. If you are using a different distribution of Linux or a different OS, please check the corresponding documentation and edit the commands as necessary.)*

::

  #Update the repositories and install dependencies
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv
  
  #Download and extract the firmware package
  wget -c http://feeds.braiins-os.com/20.04/braiins-os_am1-s9_ssh_2020-04-30-1-cbf99510-plus.tar.gz -O - | tar -xz
  
  #Change the directory to the unpacked firmware folder
  cd ./braiins-os_am1-s9_ssh_2020-04-30-1-cbf99510-plus
  
  #Create a virtual environment and activate it
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate
  
  #Install the required Python packages
  python3 -m pip install -r requirements.txt

.. _ssh_package_install:

=====================================
Install Braiins OS+ using SSH package
=====================================

Installation of Braiins OS+ using the so-called *SSH Method* consists of the following steps:

* *(Custom Firmware)* Flash stock firmware. This step can be skipped if the device is running on stock firmware or on a previous versions of Braiins OS. *(Note: It is possible, that Braiins OS+ can be installed directly over a custom firmware, but as they differ from the stock version, it might be necessary to flash stock firmware first.)*
* *(Only Windows)* Install *Ubuntu for Windows 10* available from the Microsoft Store `here. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Prepare the Python environment, which is described in the section :ref:`ssh_package_environment`.
* Run the following commands in your command line terminal (replace the placeholder ``IP_ADDRESS`` accordingly) :

*(Note that the commands are compatible with Ubuntu and Ubuntu for Windows 10. If you are using a different distribution of Linux or a different OS, please check the corresponding documentation and edit the commands as necessary.)*

::

    #Change the directory to the unpacked firmware folder (if not already in the firmware folder)
  cd ./braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46
  
  #Activate the virtual environment (if it is not already activated)
  source .env/bin/activate
  
  #Run the script to install Braiins OS+
  python3 upgrade2bos.py IP_ADDRESS

**Note:** *for more information about the arguments that can be used, use the* **--help** *argument.*

.. _ssh_package_uninstall:

=======================================
Uninstall Braiins OS+ using SSH package
=======================================

.. _ssh_package_uninstall_image:

Using factory firmware image
=============================

First, you need to prepare the Python environment, which is described in the section :ref:`ssh_package_environment`.

On an Antminer S9, you can flash a factory firmware image
from the manufacturer’s website, with ``FACTORY_IMAGE`` being file path
or URL to the ``tar.gz`` (not extracted!) file. Supported images with
corresponding MD5 hashes are listed in the
`platform.py <https://github.com/braiins/braiins/blob/master/braiins-os/upgrade/am1/platform.py>`__
file.

Run (replace the placeholders ``FACTORY_IMAGE`` and ``IP_ADDRESS`` accordingly):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS

**Note:** *for more information about the arguments that can be used, use the* **--help** *argument.*

.. _ssh_package_uninstall_backup:

Using previously created backup
===============================

First, you need to prepare the Python environment, which is described in the section :ref:`ssh_package_environment`.

If you created a backup of the original firmware during the installation of Braiins OS+, you can restore it by using the following commands (replace the placeholders ``BACKUP_ID_DATE`` and ``IP_ADDRESS`` accordingly):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS

**Note: This method is not recommended as the backup creation is very finicky. The backup can be corrupted and there is no way to check it. Use at your own risk and make sure, you can access the miner and insert an SD card to it in case the restoration does not finish successfully!**

.. _opkg:

****
OPKG
****

OPKG commands can be used after connecting to the miner via SSH. There are many OPKG commands, but regarding Braiins OS+, you need to use only the following:

  * *opkg update* - updates the package lists. It's recommended to use this command before other OPKG commands.
  * *opkg install PACKAGE_NAME* install the defined package. It's recommended to use *opkg update* to update the package lists before installing packages.
  * *opkg remove PACKAGE_NAME*

Since the firmware change results in a reboot, the following
output is expected:

::

  ...
  Collected errors:
  * opkg_conf_load: Could not lock /var/lock/opkg.lock: Resource temporarily unavailable.
    Saving config files...
    Connection to 10.10.10.1 closed by remote host.
    Connection to 10.10.10.1 closed.

=======================================
Features, PROs and CONs of this method:
=======================================

  + update Braiins OS+ remotely
  + switch to Braiins OS+ from other versions remotely
  + revert to the initial version of Braiins OS remotely
  + migrates the configuration and continue to mine without a need to configure anything (when updating or switching to Braiins OS+)
  
  - no batch-mode (unless you create your own scripts)

.. _opkg_update:

=============================
Update Braiins OS+ using OPKG
=============================

With OPKG you can easily update your current installation of Braiins OS+, by connecting to the miner via SSH and using the following commands:

::

  opkg update
  opkg install firmware

  #you can also connect to the miner and run the commands at the same time
  ssh root@IP_ADDRESS "opkg update && opkg install firmware"

This will migrate the configuration and continue to mine without a need to configure anything.

.. _opkg_switch_to_braiinsplus:

====================================================
Switch to Braiins OS+ from other versions using OPKG
====================================================

With OPKG you can easily switch to Braiins OS+, by connecting to the miner via SSH and using the following commands:

::

  opkg update
  opkg install firmware

  #you can also connect to the miner and run the commands at the same time
  ssh root@IP_ADDRESS "opkg update && opkg install bos_plus"

This will migrate the configuration and continue to mine without a need to configure anything. Default power limit will be set (1420W).

.. _opkg_factory_reset:

====================================
Braiins OS+ factory reset using OPKG
====================================

With OPKG you can easily revert to the initial version of Braiins OS (the version, which was installed for the first time on that device), by connecting to the miner via SSH and using the following commands:

::

  opkg update
  opkg remove firmware

  #you can also connect to the miner and run the commands at the same time
  ssh root@IP_ADDRESS "opkg update && opkg remove firmware"

This will reset the configuration to the state after the first Braiins OS installation.

.. _sysupgrade:

******************
Sysupgrade package
******************

Sysupgrade is used to upgrade the system running on the device. With this method, you can install various versions of Braiins OS or create a backup of the system. Installation of a firmware using *Braiins OS web interface* or using *opkg install firmware* uses this method. It's recommended to use the *Braiins OS web interface* or *opkg install firmware* instead of this method.

=====
如何使用
=====

In order to use sysupgrade, you need to connect to the miner via SSH. The syntax is the following:

::

  sysupgrade [parameters] <image file or URL>

The most important parameters are **--help** (to display the help) and **-F** to force the installation. It's not recommended to use this method (besides the way, it is described bellow), unless you really know, what you are doing.

=======================================
Features, PROs and CONs of this method:
=======================================

  + installs various version of Braiins OS, while connected to the miner
  + migrates the configuration
  + parameters are available to customize the process
  
  - no batch-mode (unless you create your own scripts)
  - cannot switch to an older version of Braiins OS (released before 2020)

.. _sysupgrade_switch_braiinsos:

==============================================================================
Switch to Braiins OS (without autotuning) from other versions using Sysupgrade
==============================================================================

In order to upgrade from older version of Braiins OS or downgrade from Braiins OS+, use the following command (replace the placeholder ``IP_ADDRESS`` accordingly):

::

  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-04-30-0-259943b5_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

This command contains the following commands: 

  * **ssh** - to connect to the miner
  * **wget** - used for downloading files, in this case the firmware package
  * **sysupgrade** - to actually flash the downloaded firmware package

.. _sysupgrade_switch_braiinsplus:

==========================================================
Switch to Braiins OS+ from other versions using Sysupgrade
==========================================================

In order to upgrade from older version of Braiins OS, use the following command (replace the placeholder ``IP_ADDRESS`` accordingly):

::

  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar http://feeds.braiins-os.com/am1-s9/firmware_2020-04-30-1-cbf99510-plus_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

This command contains the following commands: 

  * **ssh** - to connect to the miner
  * **wget** - used for downloading files, in this case the firmware package
  * **sysupgrade** - to actually flash the downloaded firmware package

Note: It's recommended to use the *BOS+ Toolbox*, *Braiins OS web interface* or *opkg install bos_plus* instead of this method.

.. _bos2bos:

**************
Bos2Bos script
**************

**Bos2Bos script is not recommended to use, unless you experience problems with the installation using the other methods.** This method works, only if Braiins OS is already running on the device.

=======================================
Features, PROs and CONs of this method:
=======================================

  + installs any version of Braiins OS remotely
  + install a clean version of Braiins OS
  + parameters are available to customize the process
  
  - no batch-mode (unless you create your own scripts)

=====
如何使用
=====

Usage of the Bos2Bos script requires the following setup:

* *(Only Windows)* Install *Ubuntu for Windows 10* available from the Microsoft Store `here. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Run the following commands in your command line terminal:

*(Note that the commands are compatible with Ubuntu and Ubuntu for Windows 10. If you are using a different distribution of Linux or a different OS, please check the corresponding documentation and edit the commands as necessary.)*

::
  
  #Update the repositories and install dependencies
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv
  
  # clone repository
  git clone https://github.com/braiins/braiins-os.git
  
  #change the directory
  cd ./braiins-os/braiins-os/

  #Create a virtual environment and activate it
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate
  
  #Install the required Python packages
  python3 -m pip install -r requirements.txt

After you succesfully finish the setup, you can use the following commands:

::

  #activate the virtual environment
  source .env/bin/activate

  #basic usage is the following
  python3 bos2bos.py FIRMWARE_URL IP_ADDRESS

  #the description of all available parameters can be displayed using the following command
  python3 bos2bos.py -h

**********
Miner tool
**********

.. _miner_nand_install:

=======================================
SD to NAND install using the Miner tool
=======================================

The SD card can be used to replace the firmware running on NAND with Braiins OS+. This can be done by connecting to the miner via SSH and usage of the following command:

  ::

    miner nand_install


.. _miner_factory_reset:

==============================================
Braiins OS+ factory reset using the Miner tool
==============================================

Factory reset can also be done using the *Miner tool*. Use the following command to do so:

  ::

    miner nand_install

.. _miner_detect:

========================================
Detect device with LEDs using Miner tool
========================================

You can find a device by turning on LED blinking, using the *Miner tool*. Use the following command to do so:

  ::

    #turn on LED blinking
    miner fault_light on

    #turn off LED blinking
    miner fault_light off
