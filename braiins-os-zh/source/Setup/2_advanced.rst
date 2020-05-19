##############
高级指南
##############

.. contents::
	:local:
	:depth: 1

********
工具及使用场景一览
********

有很多工具、安装包以及脚本都可以用于管理Braiins OS。请使用下方的树状列表查找最适合您的情况：

 * 安装Braiins OS
 
  * 使用BOS工具箱 （:ref:`bosbox_install`）
  * 使用网页端后台方式安装包 （:ref:`web_package_install`）
  * 使用SD卡刷 （:ref:`sd_install`）
  * 使用SD卡刷和矿机工具（Miner tool） （:ref:`miner_nand_install`）
  * 使用SSH脚本（:ref:`ssh_package_install`）
  
 * 升级Braiins OS
 
  * 使用BOS工具箱 （:ref:`bosbox_update`）
  * 使用OPKG批量包 （:ref:`opkg_update`）
  * 使用SYSUPGRADE（系统升级）包 （:ref:`sysupgrade_switch_braiinsplus`）
  * 使用BOS2BOS（从Braiins OS升级到Braiins OS其他版本的）脚本 （:ref:`bos2bos`）
  
 *  切换到Braiins OS社区版（不带自动调整功能）
 
  * 使用SYSUPGRADE（系统升级）包 （:ref:`sysupgrade_switch_braiinsos`）
  * 使用BOS2BOS（从Braiins OS升级到Braiins OS其他版本的）脚本 （:ref:`bos2bos`）
  
 *  切换到Braiins OS+（带自动调整功能）
 
  * 使用OPKG批量包 （:ref:`opkg_switch_to_braiinsplus`）
  * 使用SYSUPGRADE（系统升级）包 （:ref:`sysupgrade_switch_braiinsplus`）
  * 使用BOS2BOS（从Braiins OS升级到Braiins OS的其他版本）脚本 （:ref:`bos2bos`）
  
 * 重置到Braiins OS初始版本（矿机首次安装Braiins OS的版本） - 恢复出厂设置
 
  * 使用OPKG批量包 （:ref:`opkg_factory_reset`）
  * 使用SD卡刷 （:ref:`sd_factory_reset`）
  * 使用矿机工具（Miner tool） （:ref:`miner_factory_reset`）
  * 使用BOS2BOS（从Braiins OS升级到Braiins OS其他版本的）脚本（:ref:`bos2bos`）
  
 * 卸载Braiins OS
 
  * 使用BOS工具箱 （:ref:`bosbox_uninstall`）
  * 使用SSH脚本 （:ref:`ssh_package_uninstall`）

.. _bosbox:

***************
BOS工具箱
***************

BOS工具箱能让用户轻松安装，卸载，升级，检测以及配置Braiins OS。它还有批量模式，让您对矿场的管理更得心应手。我们推荐您使用批量模式管理矿机。 

=====
如何使用
=====

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址， **一个IP地址一行** ！保存文本文件后，再将文件后缀从".txt"改为".csv"。并确定此文件和BOS工具箱都放在同一路径下（同一文件夹中）。 
  * 再按下面相应部分的步骤进行操作

=======================================
BOS工具箱的特性及优缺点
=======================================

  + 远程安装Braiins OS
  + 远程升级Braiins OS
  + 远程卸载Braiins OS 
  + 远程配置Braiins OS
  + 扫描网络中的矿机
  + 安装Braiins OS时默认自动转移原厂固件中的配置（也可以设置不转移）
  + 卸载Braiins OS时默认自动转移现有配置到原厂固件（也可以设置不转移）
  + 可自定义进程的参数
  + 批量模式让管理大量矿机也能得心应手
  + 使用简单，容易上手
  
  - 不支持SSH功能被锁住的矿机

.. _bosbox_install:

======================================
使用BOS工具箱安装Braiins OS
======================================

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址， **一个IP地址一行** ！保存文本文件后，再将文件后缀从".txt"改为".csv"。并确定此文件和BOS工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS工具性的实际路径（文件夹地址），替换下方命令中的 *FILE_PATH_TO_BOS_TOOLBOX* 。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-toolbox.exe install ARGUMENTS HOSTNAME
    
    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-toolbox install ARGUMENTS HOSTNAME

    **请注意：** *当在Linux系统中使用BOS工具箱时，您需要先使用以下命令让BOS工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-toolbox

您可以使用下方的 **参数** 调整安装进程：

**重点：** 
当您在 **单台矿机** 上安装Braiins OS时，需要使用 *HOSTNAME* 这个参数 （IP地址）。
当您在多台矿机上 **批量** 安装Braiins OS时，请 **不要** 使用HOSTNAME这个参数，而是使用 *--batch BATCH* 这个参数。

====================================  ============================================================
参数                                   描述
====================================  ============================================================
-h, --help                            显示帮助信息并退出
--batch BATCH                         指定"listOfMiners.csv"（矿机主机IP地址列表）文件
--backup                              在进行升级前备份矿机
--no-nand-backup                      跳过对矿机内置储存NAND的备份（仍备份矿机配置）
--pool-user [POOL_USER]               为默认矿池设置用户名（Username）和矿工名（Workername）
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

  bos-toolbox.exe install --batch listOfMiners.csv --install-password admin

解释：上方的命令和参数，会将Braiins OS安装到在 *listOfMiners.csv* （矿机IP地址列表）中列出的矿机上。当矿机要求输入SSH密码时，命令将自动输入 *admin* 这个密码。

.. _bosbox_update:

=====================================
使用BOS工具箱升级Braiins OS
=====================================

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址， **一个IP地址一行** ！保存文本文件后，再将文件后缀从".txt"改为".csv"。并确定此文件和BOS工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS工具性的实际路径（文件夹地址），替换下方命令中的 *FILE_PATH_TO_BOS_TOOLBOX* 。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-toolbox.exe update ARGUMENTS HOSTNAME

    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-toolbox update ARGUMENTS HOSTNAME

    **请注意：** *当在Linux系统中使用BOS工具箱时，您需要先使用以下命令让BOS工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-toolbox

您可以使用下方的 **参数** 调整更新进程：

**重点：** 
当您在 **单台矿机** 上安装Braiins OS时，需要使用 *HOSTNAME* 这个参数 （IP地址）。
当您在多台矿机上 **批量** 安装Braiins OS时，请 **不要** 使用HOSTNAME这个参数，而是使用 *--batch BATCH* 这个参数。

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

解释：上方的命令和参数，会在有新固件更新可用的情况下，对在 *listOfMiners.csv* （矿机IP地址列表）中列出矿机上的Braiins OS进行更新。

.. _bosbox_uninstall:

========================================
使用BOS工具箱卸载Braiins OS
========================================

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址，一个IP地址一行！（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）中可以进行查询）。保存文本文件后，再将文件后缀从".txt"改为".csv"。确定此文件和BOS工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS工具性的实际路径（文件夹地址），替换下方命令中的*FILE_PATH_TO_BOS_TOOLBOX*。执行命令，切换到路径。 ::
  
      cd FILE_PATH_TO_BOS_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-toolbox.exe uninstall ARGUMENTS HOSTNAME

     在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-toolbox uninstall ARGUMENTS HOSTNAME
      
    **请注意：** *当在Linux系统中使用BOS工具箱时，您需要先使用以下命令让BOS工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-toolbox

您可以使用下方的 **参数** 调整卸载进程：

**重点：** 
当您在 **单台矿机** 上安装Braiins OS时，需要使用 *HOSTNAME* 这个参数 （IP地址）。
当您在多台矿机上 **批量** 安装Braiins OS时，请 **不要** 使用HOSTNAME这个参数，而是使用 *--batch BATCH* 这个参数。

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

解释：上方的命令和参数，会卸载在 *listOfMiners.csv* （矿机IP地址列表）中列出矿机上的Braiins OS，并重装原厂固件（Antminer-S9-all-201812051512-autofreq-user-Update2UBI-NF.tar.gz）。

.. _bosbox_configure:

===========================================
使用BOS工具箱配置Braiins OS
===========================================

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址，一个IP地址一行！（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）中可以进行查询）。保存文本文件后，再将文件后缀从".txt"改为".csv"。确定此文件和BOS工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS工具性的实际路径（文件夹地址），替换下方命令中的*FILE_PATH_TO_BOS_TOOLBOX*。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-toolbox.exe config ARGUMENTS ACTION TABLE

    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-toolbox config ARGUMENTS ACTION TABLE
      
    **请注意：** *当在Linux系统中使用BOS工具箱时，您需要先使用以下命令让BOS工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-toolbox

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
使用BOS工具箱扫描网络并发现矿机
======================================================

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址，一个IP地址一行！（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）中可以进行查询）。保存文本文件后，再将文件后缀从".txt"改为".csv"。确定此文件和BOS工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS工具性的实际路径（文件夹地址），替换下方命令中的*FILE_PATH_TO_BOS_TOOLBOX*。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-toolbox.exe discover ARGUMENTS

    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-toolbox discover ARGUMENTS
      
    **请注意：** *当在Linux系统中使用BOS工具箱时，您需要先使用以下命令让BOS工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-toolbox

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

如果您使用的是2019年前的原厂固件，您从矿机的网页端后台，使用Braiins OS的网页端后台方式安装包，即可用直接升级Braiins OS。使用的是其他基于原厂固件的第三方固件的情况下也应该是同理的。由于2019年后发布的原厂固件，对网页端后台升级采取了固件签名认证来防止安装第三方固件，所以Braiins OS的网页端后台方式安装包就无法用于对2019年后发布的原厂固件的升级。

=====
如何使用
=====

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **网页端后台方式安装包** 。
  * 再按下方步骤进行操作.

=======================================
此方式的特性和优缺点：
=======================================

  + 无需额外工具就能直接用Braiins OS替换调原厂固件
  + 默认自动转移原厂固件的网络配置
  + 默认自动转移原厂固件的矿池URL地址，用户名及密码
  
  - 不支持升级2019年及之后发布的原厂固件
  - 不支持配置安装（比如始终会自动转移网络配置）
  - 不支持批量操作（除非您自己写脚本）

.. _web_package_install:

=====================================
使用网页端后台方式安装包安装Braiins OS
=====================================

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **网页端后台方式安装包** 。
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

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **SD卡方式安装映像** 。
  * 再按下方步骤进行操作。

=======================================
此方式的特性和优缺点：
=======================================

  + 用Braiins OS替换锁定SSH的原厂固件
  + 默认使用内置储存NAND中的网络配置 (可禁用, 见下方的 *网络设置* 部分)
  
  - 不支持转移之前的矿池URL，用户名及密码
  - 不支持批量操作
  
.. _sd_install:

=================================
使用SD卡方式安装映像安装Braiins OS
=================================

 * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **SD卡方式安装映像** 。
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
 * 过一会，您就应该能通过设备的IP地址进到Braiins OS界面了。
 * *[可选操作]：* 您也可以将Braiins OS从SD卡刷到内置储存（NAND）上。具体请详见 :ref:`sd_nand_install`这一部分的内容。

.. _sd_network:

================
网络配置
================
 
 默认情况下，当从SD卡启动Braiins OS时，将使用内置储存NAND上的网络配置置。此特性可以按照以下步骤禁用：

  * 加载SD卡上的第一个FAT格式的分区
  * 打开uEnv.txt文件并插入下方的参数 (注意空行，一条参数一行）

  ::

    cfg_override=no

对在网络中找不到（无法发现）矿机的矿工来说，禁用原始网络配置会很有帮助（比如NAND中原静态IP地址不在局域网地址范围内）。禁用之后，则使用DHCP动态IP地址。

.. _sd_nand_install:

============
将固件刷到矿机内置储存NAND上
============

您也可以将SD卡上的Braiins OS刷到矿机内置储存NAND上。有两种方式可选：
  * 在您矿机的网页端后台中，点击 *System（系统） -> Install current system to device (NAND)（安装当前系统到矿机（NAND）上）*
  * 或通过SSH，使用 *miner（矿机）* 工具 —— 请按指南中 ref:`miner_nand_install` 的部分进行

.. _sd_factory_reset:

=======================================
使用SD卡方式安装映像对Braiins OS恢复出厂配置
=======================================

您可以按下方步骤恢复出厂配置：

  * 加载SD卡上的第一个FAT格式的分区
  * 打开uEnv.txt文件并插入下方的参数 (注意空行，一条参数一行）
  ::

    factory_reset=yes

.. _ssh_package:

****************************
远程（SSH）方式安装包
****************************

您可以使用 *远程（SSH）方式安装包* 安装或卸载Braiins OS。由于此方法需要用到Python设置，我们并不推荐使用此方法。您最好使用BOS工具箱。

=====
如何使用
=====

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **远程（SSH）方式安装包** 。
  * 再按下方步骤进行操作。

=======================================
此方式的特性和优缺点：
=======================================

  + 远程安装Braiins OS 
  + 远程卸装Braiins OS
  + 在安装Braiins OS时，自动转移原厂固件的完整配置到Braiins OS上（可自选）
  + 在卸载Braiins OS时，自动转移Braiins OS固件的完整配置到原厂固件上（可自选）
  + 可使用参数自定义安装/卸载过程
  
  - 不支持批量操作（除非您自己写脚本）
  - 配置过程耗时
  - 不支持SSH远程功能被锁住的矿机

.. _ssh_package_environment:

=========================
准备环境
=========================

首先，您需要准备Python环境。请按下方步骤操作：

* *（仅在Windows上作这一步）* 从 `微软商店 <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_ 下载安装 *“Ubuntu for Windows 10“* 。
* 在命令行终端中运行以下命令：

*（请注意，以下命令仅适用于Ubuntu或Ubuntu for Windows 10。如您使用的是另外的Linux发行版或其他操作系统，请查阅相应的技术文档并对以下命令作出相应的更改。）* 

::

  #更新库和安装发布环境(Dependencies)
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv
  
  #下载和解压固件包
  wget -c http://feeds.braiins-os.com/20.04/braiins-os_am1-s9_ssh_2020-04-30-1-cbf99510-plus.tar.gz -O - | tar -xz
  
  #更改固件解压文件夹的目录
  cd ./braiins-os_am1-s9_ssh_2020-04-30-1-cbf99510-plus
  
  #创建一个虚拟环境并启用
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate
  
  #安装所需的Python包
  python3 -m pip install -r requirements.txt

.. _ssh_package_install:

=====================================
使用远程（SSH）方式安装包安装Braiins OS
=====================================

请按以下步骤使用所谓的“远程（SSH）方式”安装Braiins OS：

* *（自定义固件）* 刷原厂固件。如果设备已经运行的是原厂固件或旧版本的Braiins OS，则此步可以跳过。 *（请注意：取决于固件版本，Braiins OS有可能可以直接从自定义固件升级，可能最开始需要刷原厂固件。）*
* *（仅在Windows上作这一步）* 从 `微软商店 <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_ 下载安装 *“Ubuntu for Windows 10“* 。
* 准备Python环境，请详见 :ref:`ssh_package_environment` 部分。
* 在命令行终端中，运行以下命令（按矿机实际IP地址替换命令中的 ``IP_ADDRESS`` ）：

*（请注意，以下命令仅适用于Ubuntu或Ubuntu for Windows 10。如您使用的是另外的Linux发行版或其他操作系统，请查阅相应的技术文档并对以下命令作出相应的更改。）* 

::

  #更改固件解压文件夹的目录（如已不在固件文件夹中）
  cd ./braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46
  
  #启用虚拟环境（如尚未启用）
  source .env/bin/activate
  
  #运行Braiins OS安装脚本
  python3 upgrade2bos.py IP_ADDRESS

**注：** *更多关于可用参数的信息说明，可用参数* **--help** *查看。*

.. _ssh_package_uninstall:

=======================================
使用远程（SSH）方式安装包卸载Braiins OS
=======================================

.. _ssh_package_uninstall_image:

用原厂固件映像的情况下
=============================

您首先需要准备Python环境，请详见 :ref:`ssh_package_environment` 部分的内容。

在蚂蚁矿机S9上，您可以使用厂家官网上的以 ``tar.gz`` 为格式未解压的原厂固件下载地址URL，替换下方命令中的 ``FACTORY_IMAGE`` ，并按矿机实际IP地址替换命令中的 ``IP_ADDRESS`` ， 并运行下方命令。 

（支持的固件映像及其相应的MD5哈希值在Github上的 `platform.py <https://github.com/braiins/braiins/blob/master/braiins-os/upgrade/am1/platform.py>`__ 文件中已列出。）

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS

**注：** *更多关于可用参数的信息说明，可用参数* **--help** *查看。*

.. _ssh_package_uninstall_backup:

用之前创建的备份的情况下
===============================

您首先需要准备Python环境，请详见 :ref:`ssh_package_environment` 部分的内容。

如果您在之前安装Braiins OS时创建了原厂固件的备份，您可以通过下方命令恢复这个备份（按备份ID和日期，和矿机实际IP地址替换命令中 ``BACKUP_ID_DATE`` 和 ``IP_ADDRESS`` ）：

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS

**注： 因为备份创建的要求比较复杂，也没有办法能够检查可能已损坏的备份文件，一般不推荐使用此法卸载Braiins OS。请在使用过程中自行注意风险，如备份恢复失败，您还可以选择使用通过SD卡方式恢复矿机固件!**

.. _opkg:

****
OPKG包管理器
****

在通过远程SSH连接矿机后，就能使用OPKG包管理器命令。OPKG包管理器命令有很多，对于Braiins OS，相应的OPKG包管理器命令如下：

  * *opkg update* - 更新包列表。推荐在使用其他OPKG包管理器命令前使用此命令。
  * *opkg install PACKAGE_NAME* - 安装定义名称的包。推荐在使用此命令安装前使用 *opkg update* 命令更新包列表。i
  * *opkg remove PACKAGE_NAME* - 移除定义名称的包。

由于固件的改变会导致重启，以下的输出将会出现：

::

  ...
  Collected errors:
  * opkg_conf_load: Could not lock /var/lock/opkg.lock: Resource temporarily unavailable.
    Saving config files...
    Connection to 10.10.10.1 closed by remote host.
    Connection to 10.10.10.1 closed.

=======================================
此方式的特性和优缺点：
=======================================

  + 远程升级Braiins OS
  + 远程从Braiins OS的其他版本切换到Braiins OS
  + 远程回滚Braiins OS初始版本
  + 无需进行任何配置，默认自动转移矿机原厂配置并继续挖矿（当升级或切换到Braiins OS时）
  
  - 不支持批量操作（除非您自己写脚本）

.. _opkg_update:

=============================
使用OPKG包管理器升级Braiins OS 
=============================

通过远程SSH方式连接到矿机，并使用以下命令，您就能使用OPKG包管理器轻松升级您安装的Braiins OS：

::

  opkg update
  opkg install firmware

  #也可以用远程SSH连接您的矿机并运行:
  ssh root@IP_ADDRESS "opkg update && opkg install firmware"

命令自动会转移您矿机的配置，您不需要作任何矿机配置上的改动。

.. _opkg_switch_to_braiinsplus:

====================================================
使用OPKG包管理器从Braiins OS的其他版本切换到（带自动调整功能的）Braiins OS+
====================================================

通过远程SSH方式连接到矿机，并使用以下命令，您就能使用OPKG包管理器轻松切换到Braiins OS+：

::

  opkg update
  opkg install bos_plus

  #也可以用远程SSH连接您的矿机并运行:
  ssh root@IP_ADDRESS "opkg update && opkg install bos_plus"

命令自动会转移您矿机的配置，您不需要作任何矿机配置上的改动。

.. _opkg_factory_reset:

====================================
使用OPKG包管理器对Braiins OS恢复出厂配置
====================================

通过远程SSH方式连接到矿机，并使用以下命令，您就能使用OPKG包管理器轻松回滚到初始（在矿机上首次安装)的Braiins OS版本:

::

  opkg update
  opkg remove firmware

  #也可以用远程SSH连接您的矿机并运行:
  ssh root@IP_ADDRESS "opkg update && opkg remove firmware"

命令会将配置重置到初次安装Braiins OS后的状态。

.. _sysupgrade:

******************
系统升级（Sysupgrade）包 
******************

系统升级（Sysupgrade）可以用于矿机上运行的系统。您可以使用此方式安装Braiins OS的各种版本，或创建系统备份。使用 *Braiins OS网页端后台* 或 *opkg安装固件* 安装固件就是使用了系统升级（Sysupgrade）包的方式。建议直接使用 *Braiins OS网页端后台* 或 *opkg安装固件* 而不是此方式进行安装。  

=====
如何使用
=====

您需要通过远程SSH连接到矿机来使用系统升级（Sysupgrade）。句法如下：

::

  sysupgrade [parameters] <image file or URL>

最重要的参数是 **--help** （显示帮助信息）和 **-F** （强制安装）。**除非是在您对此方式相当熟悉的情况下，通常不建议使用这种方式进行固件安装。**

=======================================
此方式的特性和优缺点：
=======================================

  + 当连接到矿机时，在矿机上安装各种版本的Braiins OS
  + 默认自动转移原厂固件的配置
  + 可使用参数自定义过程
  
  - 不支持批量操作（除非您自己写脚本）
  - 不支持切换到较旧版本的Braiins OS（2020年以前发布的）

.. _sysupgrade_switch_braiinsos:

==============================================================================
使用系统升级（Sysupgrade）从Braiins OS的其他版本切换到（不带自动调整功能的）Braiins OS
==============================================================================

请使用下方命令（并根据实际IP地址替换 ``IP_ADDRESS`` ），对旧版本的Braiins OS进行升级，或从Braiins OS+降级到Braiins OS：

::

  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-04-30-0-259943b5_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

此命令包含以下的命令：

  * **ssh** - 远程SSH连接到矿机
  * **wget** - 下载文件，这里具体指下载固件包
  * **sysupgrade** - 将下载的固件包刷到矿机上

.. _sysupgrade_switch_braiinsplus:

==========================================================
使用系统升级（Sysupgrade）从Braiins OS的其他版本切换到Braiins OS+
==========================================================

使用下方命令（并根据实际IP地址替换 ``IP_ADDRESS`` ），从旧版本的Braiins OS升级到从Braiins OS+：

::

  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar http://feeds.braiins-os.com/am1-s9/firmware_2020-04-30-1-cbf99510-plus_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

This command contains the following commands: 

此命令包含以下的命令：

  * **ssh** - 远程SSH连接到矿机
  * **wget** - 下载文件，这里具体指下载固件包
  * **sysupgrade** - 将下载的固件包刷到矿机上

注意：推荐使用 *BOS工具箱* ， *Braiins OS网页端后台* 或 *opkg install bos_plus命令* 而不是使用此方法升级。 

.. _bos2bos:

**************
BOS到BOS（Bos2Bos）脚本 
**************
除非是在您对此方式相当熟悉的情况下，通常不建议使用BOS到BOS（Bos2Bos）脚本这种方式进行固件安装
**除非是在您在使用其他方法安装遇到问题的情况下，通常不建议使用BOS到BOS（Bos2Bos）脚本。** 只有矿机上已安装有Braiins OS，此方法才会是有效的。

=======================================
此方式的特性和优缺点：
=======================================

  + 远程安装Braiins OS的任意版本
  + 安装纯净版Braiins OS
  + 可使用参数自定义过程
  
  - 不支持批量操作（除非您自己写脚本）

=====
如何使用
=====

使用BOS到BOS（Bos2Bos）脚本需要的环境设置如下：

* *（仅在Windows上作这一步）* 从 `微软商店 <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_ 下载安装 *“Ubuntu for Windows 10“* 。
* 在命令行终端中运行以下命令：

*（请注意，以下命令仅适用于Ubuntu或Ubuntu for Windows 10。如您使用的是另外的Linux发行版或其他操作系统，请查阅相应的技术文档并对以下命令作出相应的更改。）* 

::
  
  #更新库和安装发布环境(Dependencies)
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv
  
  #克隆库
  git clone https://github.com/braiins/braiins-os.git
  
  #更改目录
  cd ./braiins-os/braiins-os/

  #创建一个虚拟环境并启用
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate
  
  #安装所需的Python包
  python3 -m pip install -r requirements.txt

在您完成环境设置后，您可以使用以下命令：

::

  #启用虚拟环境
  source .env/bin/activate

  #下方是基本用法
  python3 bos2bos.py FIRMWARE_URL IP_ADDRESS

  #下方是可用于显示所有可用参数的帮助信息的命令
  python3 bos2bos.py -h

**********
矿机工具（Miner tool）
**********

.. _miner_nand_install:

=======================================
使用矿机工具（Miner tool）将SD卡上的固件安装到矿机内置储存NAND上
=======================================

通过远程SSH方式连接到矿机，并使用以下命令，就能用SD卡上的Braiins OS固件替换矿机内置储存NAND中的固件：

  ::

    miner nand_install


.. _miner_factory_reset:

==============================================
使用矿机工具（Miner tool）对Braiins OS恢复出厂配置
==============================================

使用以下命令，同样能通过 *矿机工具（Miner tool）* 对矿机上的Braiins OS恢复出厂配置：

  ::

    miner nand_install

.. _miner_detect:

========================================
使用矿机工具（Miner tool）通过LED灯找出矿机
========================================

使用以下命令，您就可以通过 *矿机工具（Miner tool）* 以让矿机LED闪烁的方式找出矿机：

  ::

    #开启LED闪烁
    miner fault_light on

    #关闭LED闪烁
    miner fault_light off
