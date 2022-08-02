
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

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
 
  * 使用BOS工具箱 （:ref:`bosbox_install`）
  * 使用网页端后台方式安装包 （:ref:`web_package_install`）
  * 使用SD卡刷 （:ref:`sd_install`）
  * 使用SD卡刷和矿机工具（Miner tool） （:ref:`miner_nand_install`）
  * 使用SSH脚本（:ref:`ssh_package_install`）

 * 蚂蚁矿机S9固件远程SSH锁解锁
 
  * 使用BOS工具箱 (:ref:`bosbox_unlock`)  
  
 * 升级Braiins OS+
 
  * 使用BOS工具箱 （:ref:`bosbox_update`）
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
 
  * 使用BOS工具箱 （:ref:`bosbox_uninstall`）
  * 使用SSH脚本 （:ref:`ssh_package_uninstall`）
  
 * 开启/关闭预先发行版（Nightly Version）推送

  * 使用矿机工具（Miner tool） (:ref:`miner_nightly`)

 * 开启/关闭自动更新

  * 使用矿机工具（Miner tool） (:ref:`miner_autoupgrade`)

 * 在矿机上运行自定义Shell命令

  * 使用BOS工具箱 (:ref:`bosbox_command`)
  
   * 使用BOS工具箱扫描网络识别矿机

  * 使用BOS工具箱 (:ref:`bosbox_scan`)

 * 使用BOS工具箱监听来自矿机的广播

  * 使用BOS工具箱 (:ref:`bosbox_listen`)


.. _bosbox:

***************
BOS工具箱
***************

BOS工具箱能让用户轻松安装，卸载，升级，检测以及配置Braiins OS+，并在矿机上运行自定义命令。它还有批量模式，让您对矿场的管理更得心应手。我们推荐您使用批量模式管理矿机。 

=====
如何使用
=====

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，然后在文件内按需输入矿机IP地址， **一个IP地址一行！** 保存文本文件后，再将文件后缀从".txt"改为".csv"。批量表格和BOS工具箱需在同一路径下（同一文件夹中）。 
  * 再按下面相应部分的步骤进行操作

=======================================
BOS工具箱的特性及优缺点
=======================================

  + 远程安装Braiins OS+，在安装过程中自动破解S9矿机上的官固锁
  + 远程升级Braiins OS+
  + 远程卸载Braiins OS+ 
  + 远程配置Braiins OS+
  + 在矿机上运行自定义命令
  + 扫描网络中的矿机
  + 安装Braiins OS+时默认自动转移原厂固件中的配置（也可以设置不转移）
  + 卸载Braiins OS+时默认自动转移现有配置到原厂固件（也可以设置不转移）
  + 可自定义进程的参数
  + 安装Braiins OS+后默认自动开启矿机自动调整功能（默认功率限制1420瓦）
  + 批量模式让管理大量矿机也能得心应手
  + 使用简单，容易上手
  - 尚不支持破解SSH远程功能有官固锁的X17系列矿机

.. _bosbox_install:

======================================
使用BOS工具箱安装Braiins OS+
======================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，然后在文件内按需输入矿机IP地址， **一个IP地址一行！** 保存文本文件后，再将文件后缀从".txt"改为".csv"。批量表格和BOS工具箱需在同一路径下（同一文件夹中）。 
  * 下载BOS+工具箱后，双击（Windows上）或使用命令行 ``./bos-toolbox`` （Linux上）打开工具箱。
  * 在 **安装（install）** 部分，选择刚刚创建的csv批量表格文件确定要操作的 **矿机名** 范围，然后点击 **启动** 。

您可以使用下方的 **参数** 调整安装进程：

====================================  ====================================  ============================================================
GUI选项                                命令行参数                             描述
====================================  ====================================  ============================================================
密码                                  -p PASSWORD, --password PASSWORD      矿机密码
Farm-ID                               --bos-mgmt-id [BOS_MGMT_ID]	          设置Braiins OS+管家标识符
电源功率限值                           --psu-power-limit [PSU_POWER_LIMIT]   设置（以瓦为单位）的电源功率限值
配置矿池                               --pool-user [POOL_USER]               配置默认矿池SLush Pool矿池的用户名和矿工名
最新稳定发布版                         *不适用* - 默认                        推荐默认选项
开源版                                 --open-source         		 用于安装开源版固件 (不能与**预先发布版**和**自定义下载地址**参数同时使用)
预先发布版                             --nightly             		用于安装预先发布版固件 (不能与**开源版**和**自定义下载地址**参数同时使用)
自定义固件下载地址                     --feeds-url [FEEDS_URL]		     自定义固件下载URL链接地址 (不能与**开源版**和**预先发布版**参数同时使用)
固件版本                              --fw-version [FW_VERSION]	         选择具体固件版本
不开启自动升级                         --no-auto-upgrade                     不开启自动升级（不推荐）
不保留现有矿池配置                     --no-keep-pools                       不保留（转移）矿机的原矿池配置
保留现有网络配置                       *不适用* - 默认                       保留矿机的原网络配置，默认选择（推荐）
不保留现有网络配置                     --no-keep-network                     不保留（转移）矿机的原网络配置（在使用DHCP自动分配IP的情况下）
不保留现有矿机名                       --no-keep-hostname                    不保留（转移）矿机的原主机名（Hostname）并根据矿机MAC地址生成一个新的
保留现有矿机名                         --keep-hostname                       保留矿机用户名
升级后                                --post-upgrade [POST_UPGRADE]         指定stage3.sh脚本文件目录
*不适用* - 尚不支持                    -h, --help                            显示帮助信息并退出
*不适用* - 尚不支持                    --backup                              在安装前备份矿机（NAND内置储存上的原固件和配置）
*不适用* - 尚不支持                    --no-nand-backup                      不备份矿机NAND内置储存（仍备份配置）
*不适用* - 尚不支持                    --no-wait                             直到系统完全更新或重启完毕不等待
*不适用* - 尚不支持                    --dry-run                             执行所有的更新步骤但不实际进行更新
====================================  ====================================  ============================================================


**安装命令和参数使用示例如下：**

::

  bos-toolbox.bat install --psu-power-limit 1200 --password root listOfMiners.csv

解释：上方的命令和参数，会自动破解蚂蚁矿机S9上的官固固件锁，并将Braiins OS+安装到矿机IP地址批量表（命名为listOfMiners.csv的文件）中列出的矿机上，并设置列表中所有矿机的输入功率限制为1200瓦。当矿机要求输入SSH密码时，命令将自动输入 *admin* 这个密码。

.. _bosbox_update:

=====================================
使用BOS工具箱升级Braiins OS+
=====================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，然后在文件内按需输入矿机IP地址， **一个IP地址一行！** 保存文本文件后，再将文件后缀从".txt"改为".csv"。批量表格和BOS工具箱需在同一路径下（同一文件夹中）。 
  * 下载BOS+工具箱后，双击（Windows上）或使用命令行 ``./bos-toolbox`` （Linux上）打开工具箱。
  * 在 **更新（update）** 部分，选择刚刚创建的csv批量表格文件确定要操作的 **矿机名** 范围，然后点击 **启动** 。

您可以使用下方的 **参数** 调整更新进程：

====================================  ====================================  ============================================================
GUI选项                               参数                                   描述
====================================  ====================================  ============================================================
升级包名               	            PACKAGE_NAME                         固件升级包名（选择升级到哪个固件版本）
密码                                  -p PASSWORD, --password PASSWORD      矿机密码
Farm-ID                               --bos-mgmt-id [BOS_MGMT_ID]           设定Braiins OS+管家标识符
忽略无响应矿机                         -i, --ignore                          忽略错误
*不适用* - 尚不支持                    --h, --help                           显示帮助信息并退出
====================================  ====================================  ============================================================


**更新命令和参数使用示例如下：**

::

  bos-toolbox.bat update listOfMiners.csv firmware

解释：上方的命令和参数，会在有新固件更新可用的情况下，对在矿机IP地址批量表（命名为listOfMiners.csv的文件）中列出矿机上的Braiins OS+进行更新。

.. _bosbox_uninstall:

========================================
使用BOS工具箱卸载Braiins OS+
========================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，然后在文件内按需输入矿机IP地址， **一个IP地址一行！** 保存文本文件后，再将文件后缀从".txt"改为".csv"（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）* 中可以进行查询，或用Braiins OS+管家或BTCTool批量扫描导出）。批量表格和BOS工具箱需在同一路径下（同一文件夹中）。 
  * 下载BOS+工具箱后，双击（Windows上）或使用命令行 ``./bos-toolbox`` （Linux上）打开工具箱。
  * 在 **卸载（uninstall）** 部分，选择刚刚创建的csv批量表格文件确定要操作的 **矿机名** 范围，然后点击 **启动** 。
  
您可以使用下方的 **参数** 调整卸载进程：

====================================  ====================================  ============================================================
GUI选项                               参数                                   描述
====================================  ====================================  ============================================================
密码                                  -p PASSWORD, --password PASSWORD      矿机密码
默认原厂固件                           *不适用* - 默认                        默认设置
自定义固件下载地址                      --feeds-url [FEEDS_URL]		    自定义固件下载URL链接地址
*不适用* - 尚不支持                    --nand-restore			       使用之前的备份对整个NAND进行恢复
*不适用* - 尚不支持                    BACKUP_PATH                           指定从一个目录或一个tgz文件恢复备份的路径
*不适用* - 尚不支持                    --h, --help                           显示帮助信息并退出
====================================  ====================================  ============================================================

**卸载命令和参数使用示例如下：**

::

  bos-toolbox.bat uninstall listOfMiners.csv

解释：上方的命令和参数，会卸载在矿机IP地址批量表（命名为listOfMiners.csv的文件）中列出矿机上的Braiins OS+，并重装原厂固件。

**注意：**
卸装Braiins OS+后恢复的原厂固件不适合挖矿！在开始挖矿前，请按您的矿机型号升级到原厂固件的较新版本。

**重要提示：** 
*BACKUP_PATH* 参数为可选参数。仅与 *--nand-restore* 参数一起使用。通常 **不建议** 恢复之前备份的固件。

.. _bosbox_configure:

===========================================
使用BOS工具箱配置Braiins OS+
===========================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，然后在文件内按需输入矿机IP地址， **一个IP地址一行！** 保存文本文件后，再将文件后缀从".txt"改为".csv"（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）* 中可以进行查询，或用Braiins OS+管家或BTCTool批量扫描导出）。批量表格和BOS工具箱需在同一路径下（同一文件夹中）。  
  * 下载BOS+工具箱后，双击（Windows上）或使用命令行 ``./bos-toolbox`` （Linux上）打开工具箱。
  * 在 **配置（config）** 部分，选择刚刚创建的csv批量表格文件确定要操作的 **矿机名** 范围，然后点击 **启动** 。

您必须 **至少选择使用** 下方的四个 **操作** 之一对配置进程进行调整：

====================================  ============================================================
参数                                   描述
====================================  ============================================================
load                                  加载矿机的目前配置到一个CSV文件中

save                                  保存CSV文件中的矿机设定到矿机（但尚未应用设定）

apply                                 应用之前从CSV文件复制（保存）到矿机上的设定
                                      
save_apply                            保存并应用之前从CSV文件复制（保存）到矿机上的设定
====================================  ============================================================

您可以使用下方的 **参数** 调整配置进程：

====================================  ====================================  ============================================================
GUI选项                               参数                                   描述
====================================  ====================================  ============================================================
用户名                                -u USER, --user USER                  矿机登录名
密码                                  -p PASSWORD, --password PASSWORD      矿机登录密码
更改密码                              --change-password		        允许更改密码 (对于*listOfMiners.csv*中的密码)
忽略错误                              -i, --ignore                          忽略错误
*不适用* - 尚不支持                    -h, --help                            显示帮助信息并退出
*不适用* - 尚不支持                    -c, --check                           不写入的试运行检查
====================================  ====================================  ============================================================

**配置命令和参数使用示例如下：**

::

  bos-toolbox.bat config --user root load listOfMiners.csv
  
  #把矿机上的配置加载到一个CSV文件中后，之后通过表格软件编辑配置（如MS Office Excel，LibreOffice Calc等)
  
  bos-toolbox.bat config --user root root -p admin -P save_apply listOfMiners.csv

解释：上方的第一个命令和参数，会（使用 *root* 这个后台用户名）提取在矿机IP地址批量表（命名为listOfMiners.csv的文件）中列出矿机的配置，并将这些配置保存到一个CSV文件中。然后您可以打开并编辑这个CSV文件，调整矿机的配置。您改动好之后，就可以用上方的第二个命令和参数，将配置复制（保存）到矿机上，更改密码为新设置的密码，并应用新配置。

.. _bosbox_scan:

======================================================
使用BOS工具箱扫描网络并发现矿机
======================================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，然后在文件内按需输入矿机IP地址， **一个IP地址一行！** 保存文本文件后，再将文件后缀从".txt"改为".csv"（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）* 中可以进行查询，或用Braiins OS+管家或BTCTool批量扫描导出）。批量表格和BOS工具箱需在同一路径下（同一文件夹中）。  
  * 下载BOS+工具箱后，双击（Windows上）或使用命令行 ``./bos-toolbox`` （Linux上）打开工具箱。
  * 在 **扫描（scan）** 部分，选择刚刚创建的csv批量表格文件确定要操作的 **矿机名** 范围，然后点击 **启动** 。
  

您可以使用下方的 **参数** 调整网络扫描和矿机发现进程：

====================================  ====================================  ============================================================
GUI选项                                参数                                  描述
====================================  ====================================  ============================================================
密码                                  -p PASSWORD, --password PASSWORD      矿机密码
保存结果                              -o OUTPUT, --output OUTPUT            保存IP地址监测结果到文件
详情                                  -v, --verbose                         报告网络错误
*不适用* - 尚不支持                    -h, --help                            显示帮助信息并退出
*不适用* - 尚不支持                    -j JOBS, --jobs JOBS                  网络扫描并行工作数
====================================  ====================================  ============================================================


**网络扫描和矿机发现命令和参数使用示例如下：**

::

  #扫描从10.10.10.0到10.10.10.255的网络范围
  bos-toolbox.bat scan 10.10.10.0/24

  #扫描从10.10.0.0到10.10.255.255的网络范围
  bos-toolbox.bat scan 10.10.0.0/16

  #扫描从10.10.0.0到10.255.255.255的网络范围
  bos-toolbox.bat scan 10.0.0.0/8

.. _bosbox_listen:

================================================
使用BOS工具箱监听来自矿机的广播
================================================

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，然后在文件内按需输入矿机IP地址， **一个IP地址一行！** 保存文本文件后，再将文件后缀从".txt"改为".csv"（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）* 中可以进行查询，或用Braiins OS+管家或BTCTool批量扫描导出）。批量表格和BOS工具箱需在同一路径下（同一文件夹中）。  
  * 下载BOS+工具箱后，双击（Windows上）或使用命令行 ``./bos-toolbox`` （Linux上）打开工具箱。
  * 在 **监听（listen）** 部分，选择刚刚创建的csv批量表格文件确定要操作的 **矿机名** 范围，然后点击 **启动** 。
    
  您可以使用下方的 **参数** 调整监听的进程：
  
====================================  ====================================  ============================================================
GUI选项                               参数                                   描述
====================================  ====================================  ============================================================
保存结果                              -o OUTPUT, --output OUTPUT             保存IP地址监测结果到文件
格式                                  --format FORMAT                       更改设备信息默认格式; '{IP}'和'{MAC}'标签将被实际数据替换
*不适用* - 尚不支持                    -h, --help                            显示帮助信息并退出
====================================  ====================================  ============================================================

.. _bosbox_command:

================================================
使用BOS工具箱在矿机上运行自定义命令
================================================
  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，然后在文件内按需输入矿机IP地址， **一个IP地址一行！** 保存文本文件后，再将文件后缀从".txt"改为".csv"（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）* 中可以进行查询，或用Braiins OS+管家或BTCTool批量扫描导出）。批量表格和BOS工具箱需在同一路径下（同一文件夹中）。  
  * 下载BOS+工具箱后，双击（Windows上）或使用命令行 ``./bos-toolbox`` （Linux上）打开工具箱。
  * 在 **命令（command）** 部分，选择刚刚创建的csv批量表格文件确定要操作的 **矿机名** 范围，然后点击 **启动** 。
  
  您可以使用下方的 **参数** 调整矿机上运行自定义命令的进程:
  
====================================  ====================================  ============================================================
GUI选项                               参数                                   描述
====================================  ====================================  ============================================================
显示远程输出                           -o, --output                          捕获并输出远程结果
显示矿机名输出                         -O, --output-hostname                 捕获并输出矿机用户名结果
密码                                  -p PASSWORD, --password PASSWORD      矿机密码
*不适用* - 尚不支持                    -h, --help                            显示帮助信息并退出
*不适用* - 尚不支持                    -j JOBS, --jobs JOBS                  并行工作数
*不适用* - 尚不支持                    -a, --auto                            如RPC不可用，则使用SSH
*不适用* - 尚不支持                    -l, --legacy                          使用SSH
*不适用* - 尚不支持                    -L, --no-legacy                       使用RPC
====================================  ====================================  ============================================================

**矿机运行自定义命令的命令和参数使用示例如下：**

::

  #关闭矿机IP地址批量表（命名为list.csv的文件）中列出矿机上的BOSminer, 有效地停止挖矿并将电能消耗降到最低
  bos-toolbox.bat command -o list.csv stop

.. _bosbox_unlock:

============================================
使用BOS工具箱解锁蚂蚁矿机S9上的固件远程SSH锁
============================================

  * 在我们 `官网 <https://zh.braiins.com/os/plus/download/>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，然后在文件内按需输入矿机IP地址， **一个IP地址一行！** 保存文本文件后，再将文件后缀从".txt"改为".csv"（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）* 中可以进行查询，或用Braiins OS+管家或BTCTool批量扫描导出）。批量表格和BOS工具箱需在同一路径下（同一文件夹中）。  
  * 下载BOS+工具箱后，双击（Windows上）或使用命令行 ``./bos-toolbox`` （Linux上）打开工具箱。
  * 在 **解锁（unlock）** 部分，选择刚刚创建的csv批量表格文件确定要操作的 **矿机名** 范围，然后点击 **启动** 。

    您可以使用下方的 **参数** 调整解锁进程：

====================================  ====================================  ============================================================
GUI选项                               参数                                   描述
====================================  ====================================  ============================================================
用户名                                 -u USERNAME, --username USERNAME      矿机登录名
密码                                   -p PASSWORD, --password PASSWORD      矿机登录密码
*不适用* - 尚不支持                     --h, --help                           显示帮助信息并退出
*不适用* - 尚不支持                     --port PORT                           蚂蚁矿机网页后台解锁用端口
*不适用* - 尚不支持                     --ssl                                 是否使用SSL协议
====================================  ====================================  ============================================================


**安装命令和参数使用示例如下：**

::

  bos-toolbox.bat unlock -p root listOfMiners.csv

解释：上方的命令和参数，会解锁在矿机IP地址批量表（命名为listOfMiners.csv的文件）中列出的矿机上的固件远程SSH锁。

.. _web_package:

***********
网页端后台方式安装包
***********

如果您使用的是2019年前的原厂固件，您从矿机的网页端后台，使用Braiins OS+的网页端后台方式安装包，即可用直接升级Braiins OS+。使用的是其他基于原厂固件的第三方固件的情况下也应该是同理的。由于2019年后发布的原厂固件，对网页端后台升级采取了固件签名认证来防止安装第三方固件，所以Braiins OS+的网页端后台方式安装包就无法用于对2019年后发布的原厂固件的升级。

=====
如何使用
=====

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **网页端后台方式安装包** 。
  * 再按下方步骤进行操作。

=======================================
此方式的特性和优缺点：
=======================================

  + 无需额外工具就能直接用Braiins OS+替换调原厂固件
  + 默认自动转移原厂固件的网络配置
  + 默认自动转移原厂固件的矿池URL地址，用户名及密码
  + 默认自动开启矿机自动调整功能（默认功率限制1420瓦）
  
  - 不支持升级2019年及之后发布的原厂固件
  - 不支持配置安装（比如始终会自动转移网络配置）
  - 不支持批量操作（除非您自己写脚本）

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
  * 再按下方步骤进行操作。

=======================================
此方式的特性和优缺点：
=======================================

  + 用Braiins OS+替换锁定SSH的原厂固件
  + 默认使用内置储存NAND中的网络配置 (可禁用, 见下方的 *网络设置* 部分)
  + 默认自动开启矿机自动调整功能（默认功率限制1420瓦）
  
  - 不支持转移之前的矿池URL，用户名及密码
  - 不支持批量操作
  
.. _sd_install:

=================================
使用SD卡方式安装映像安装Braiins OS+
=================================

 * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **SD卡方式安装映像** 。
 * 将下载的映像烧录到SD卡上（例如使用像 `Etcher <https://etcher.io/>`_ 之类的烧录软件）。*请注意：光复制到SD卡上是不够的，必须用软件刷到卡上！*
 * **(只有蚂蚁矿机S9)** 调整跳线，让矿机从SD卡启动（而不是从NAND内存），如下所示。

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

您可以按下方步骤恢复出厂配置：

  * 加载SD卡上的第一个FAT格式的分区
  * 打开uEnv.txt文件并插入下方的参数 (注意空行，一条参数一行）
  ::

    factory_reset=yes

.. _ssh_package:

****************************
远程（SSH）方式安装包
****************************

您可以使用 *远程（SSH）方式安装包* 安装或卸载Braiins OS+。由于此方法需要用到Python设置，我们并不推荐使用此方法。您最好使用BOS工具箱。

=====
如何使用
=====

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **远程（SSH）方式安装包** 。
  * 再按下方步骤进行操作。

=======================================
此方式的特性和优缺点：
=======================================

  + 远程安装Braiins OS+ 
  + 远程卸装Braiins OS+
  + 在安装Braiins OS+时，自动转移原厂固件的完整配置到Braiins OS+上（可自选）
  + 在卸载Braiins OS+时，自动转移Braiins OS+固件的完整配置到原厂固件上（可自选）
  + 可使用参数自定义安装/卸载过程
  + 在安装Braiins OS+后，默认自动开启矿机自动调整功能（默认功率限制1420瓦）
  
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
  #Antminer S9
  wget -c https://feeds.braiins-os.com/20.10/braiins-os_am1-s9_ssh_2020-10-25-0-908ca41d-20.10-plus.tar.gz -O - | tar -xz
  
  #Antminer S17
  wget -c https://feeds.braiins-os.com/20.11/braiins-os_am2-s17_ssh_2020-11-27-0-5eb922d4-20.11-plus.tar.gz -O - | tar -xz

  #更改固件解压文件夹的目录
  #Antminer S9
  cd ./braiins-os_am1-s9_ssh_VERSION
  
  #Antminer S17
  cd ./braiins-os_am2-s17_ssh_VERSION

  #创建一个虚拟环境并启用
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate
  
  #安装所需的Python包
  python3 -m pip install -r requirements.txt

.. _ssh_package_install:

=====================================
使用远程（SSH）方式安装包安装Braiins OS+
=====================================

请按以下步骤使用所谓的“远程（SSH）方式”安装Braiins OS+：

* *（自定义固件）* 刷原厂固件。如果设备已经运行的是原厂固件或旧版本的Braiins OS，则此步可以跳过。 *（请注意：取决于固件版本，Braiins OS+有可能可以直接从自定义固件升级，可能最开始需要刷原厂固件。）*
* *（仅在Windows上作这一步）* 从 `微软商店 <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_ 下载安装 *“Ubuntu for Windows 10“* 。
* 准备Python环境，请详见 :ref:`ssh_package_environment` 部分。
* 在命令行终端中，运行以下命令（按矿机实际IP地址替换命令中的 ``IP_ADDRESS`` ）：

*（请注意，以下命令仅适用于Ubuntu或Ubuntu for Windows 10。如您使用的是另外的Linux发行版或其他操作系统，请查阅相应的技术文档并对以下命令作出相应的更改。）* 

::

  #更改固件解压文件夹的目录（如已不在固件文件夹中）
  #Antminer S9
  cd ./braiins-os_am1-s9_ssh_VERSION
  
  #Antminer S17
  cd ./braiins-os_am2-s17_ssh_VERSION

  #启用虚拟环境（如尚未启用）
  source .env/bin/activate
  
  #运行Braiins OS+安装脚本
  python3 upgrade2bos.py IP_ADDRESS

**注：** *更多关于可用参数的信息说明，可用参数* **--help** *查看。*

.. _ssh_package_uninstall:

=======================================
使用远程（SSH）方式安装包卸载Braiins OS+
=======================================

.. _ssh_package_uninstall_image:

用原厂固件映像的情况下
=============================

您首先需要准备Python环境，请详见 :ref:`ssh_package_environment` 部分的内容。

在蚂蚁矿机S9上，您可以使用厂家官网上的以 ``tar.gz`` 为格式未解压的原厂固件下载地址URL，替换下方命令中的 ``FACTORY_IMAGE`` ，并按矿机实际IP地址替换命令中的 ``IP_ADDRESS`` ， 并运行下方命令。 

（支持的固件映像及其相应的MD5哈希值在Github上的 `platform.py <https://github.com/braiins/braiins/blob/master/braiins-os/upgrade/am1/platform.py>`__ 文件中已列出。）

::

  #Antminer S9
  cd ~/braiins-os_am1-s9_ssh_2020-09-07-1-463cb8d0-20.09-plus && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS
  
  #Antminer S17
  cd ~/braiins-os_am2-s17_ssh_2020-11-27-0-5eb922d4-20.11-plus && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS

**注：** *更多关于可用参数的信息说明，可用参数* **--help** *查看。*

.. _ssh_package_uninstall_backup:

用之前创建的备份的情况下
===============================

您首先需要准备Python环境，请详见 :ref:`ssh_package_environment` 部分的内容。

如果您在之前安装Braiins OS+时创建了原厂固件的备份，您可以通过下方命令恢复这个备份（按备份ID和日期，和矿机实际IP地址替换命令中 ``BACKUP_ID_DATE`` 和 ``IP_ADDRESS`` ）：

::

  #Antminer S9
  cd ~/braiins-os_am1-s9_ssh_2020-09-07-1-463cb8d0-20.09-plus && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS
  
  #Antminer S17
  cd ~/braiins-os_am2-s17_ssh_2020-11-27-0-5eb922d4-20.11-plus && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS

**注： 因为备份创建的要求比较复杂，也没有办法能够检查可能已损坏的备份文件，一般不推荐使用此法卸载Braiins OS+。请在使用过程中自行注意风险，如备份恢复失败，您还可以选择使用通过SD卡方式恢复矿机固件!**

.. _opkg:

****
OPKG包管理器
****

在通过远程SSH连接矿机后，就能使用OPKG包管理器命令。OPKG包管理器命令有很多，对于Braiins OS+，相应的OPKG包管理器命令如下：

::
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

  + 远程升级Braiins OS+
  + 远程从Braiins OS的其他版本切换到Braiins OS+
  + 远程回滚Braiins OS初始版本
  + 无需进行任何配置，默认自动转移矿机原厂配置并继续挖矿（当升级或切换到Braiins OS+时）
  
  - 不支持批量操作（除非您自己写脚本）

.. _opkg_update:

=============================
使用OPKG包管理器升级Braiins OS+
=============================

通过远程SSH方式连接到矿机，并使用以下命令，您就能使用OPKG包管理器轻松升级您安装的Braiins OS+：

::

  opkg update
  opkg install firmware

  #也可以用远程SSH连接您的矿机并运行:
  ssh root@IP_ADDRESS "opkg update && opkg install firmware"

命令自动会转移您矿机的配置，您不需要作任何矿机配置上的改动。

.. _opkg_switch_to_braiinsplus:

====================================================
使用OPKG包管理器从Braiins OS的其他版本切换到Braiins OS+
====================================================

通过远程SSH方式连接到矿机，并使用以下命令，您就能使用OPKG包管理器轻松切换到Braiins OS+：

::

  opkg update
  opkg install bos_plus

  #也可以用远程SSH连接您的矿机并运行:
  ssh root@IP_ADDRESS "opkg update && opkg install bos_plus"

命令自动会转移您矿机的配置，您不需要作任何矿机配置上的改动。默认功率将被限制为1420瓦。

.. _opkg_factory_reset:

====================================
使用OPKG包管理器对Braiins OS+恢复出厂配置
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

  #Antminer S9
  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-09-07-0-e50f2a1b-20.09_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'
  
  #Antminer S17
  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am2-s17/firmware_2020-09-07-0-e50f2a1b-20.09_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

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

  #Antminer S9
  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.com/am1-s9/firmware_2020-09-07-1-463cb8d0-20.09-plus_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'
  
  #Antminer S17
  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.com/am2-s17/firmware_2020-11-27-0-5eb922d4-20.11-plus_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'
  
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

通过远程SSH方式连接到矿机，并使用以下命令，就能用SD卡上的Braiins OS+固件替换矿机内置储存NAND中的固件：

  ::

    miner nand_install


.. _miner_factory_reset:

==============================================
使用矿机工具（Miner tool）对Braiins OS+恢复出厂配置
==============================================

使用以下命令，同样能通过 *矿机工具（Miner tool）* 对矿机上的Braiins OS+恢复出厂配置：

  ::

    miner factory_reset

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
    
.. _miner_nightly:

==============================================
使用矿机工具（Miner tool）开启/关闭预先发行版（Nightly Version）推送
==============================================

预先发行版旨在最快能修复固件的一些关键问题，因此它在发布前不会像正式版那样经过全面测试。使用以下命令，您就可以通过 *矿机工具（Miner tool）* 开启或关闭最新的预先发行版更新推送：

  ::

    #开启预先发行版推送
    miner nightly_feeds on

    #关闭预先发行版推送
    miner nightly_feeds off

.. _miner_autoupgrade:

=============================================
使用矿机工具（Miner tool）开启/关闭自动升级
=============================================

您可以通过开启自动升级这一特性，让矿机固件自动升级到最新的系统版本。这一特性在从 **原厂** 固件切换到Braiins OS+时是默认 **开启** 的，从 **Braiins OS** 或 **Braiins OS+** 的旧版本升级的情况下是默认 **关闭** 的。使用以下命令，您就可以通过 *矿机工具（Miner tool）* 开启/关闭固件自动升级：

  ::

    #开启自动升级
    miner auto_upgrade on

    #关闭自动升级
    miner auto_upgrade off

    
