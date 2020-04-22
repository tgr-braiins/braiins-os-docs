####
安装
####

.. contents::
	:local:
	:depth: 1

*********
安装前准备
*********

本文档是您想在矿机上安装Braiins OS+的快速指南。有两种方法能够测试并使用Braiins OS+

1. **从带有Braiins OS映像文件的SD卡启动**，这样能有效地保留矿机上内置的原厂固件。假如您遇到任何问题，您很轻松就能从矿机内置储存启动原有固件。我们建议您在一开始用这个安全的方法。

2. **永久性地刷新固件**，让Braiins OS完全并有效地替换掉您矿机上的原厂固件。用这个方法安装后，如您想要恢复矿机的原厂固件，只能通过回滚您在安装时创建的原厂固件备份来恢复。
     
基于上述原因，我们强烈推荐 **只在支持从SD卡启动的矿机上安装Braiins OS**。

要想开始使用Braiins OS+和BOSminer+，您会需要：

* 支持Braiins OS的ASIC矿机
* 下载Braiins OS+最新版本
* 安装Braiins OS+
* 设置Braiins OS+并开始挖矿

*注：本指南中使用到的命令，是基于说明目的编写。您可能需要根据实际情况，调整命令中的文件路径和名称。*

************
安装/升级指南
************

下方是针对各种安装/升级情景的指南导航

* **原厂固件 -> Braiins OS+（最新版）** - 按照 :ref:`sd_card_method` 或 :ref:`remote_ssh_method` 的步骤进行。 
* **Braiins OS社区版（旧版）-> Braiins OS+（最新版）** - 按照 :ref:`upgrade_community_bos_plus` 的步骤进行。
* **Braiins OS社区版（旧版）-> Braiins OS社区版（最新版）** 按照 :ref:`downgrade_bos_plus_community` 的步骤进行。
* **Braiins OS社区版（旧版）-> Braiins OS+（最新版）** 按照 :ref:`upgrade_community_bos_plus` 的步骤进行。
* **Braiins OS+（最新版）-> Braiins OS社区版（最新版）** 按照 :ref:`downgrade_bos_plus_community` 的步骤进行。
* **Braiins OS+ -> 原厂固件** - 按照 :ref:`downgrade_bos_stock` 的步骤进行。

.. _sd_card_method:

*********
SD卡方式
*********

* 从我们 `官网 <https://zh.braiins-os.com/>`_ 上下载最新发布的SD卡映像。您可以用我们的公钥签名验证映像文件。`点此下载  <https://slushpool.com/media/download/braiins-os.gpg.pub>`_ 我们的公钥签名。
* 将下载的映像烧录到SD卡上（例如使用像 `Etcher <https://etcher.io/>`_ 之类的烧录软件）
* 调整跳线，让矿机从SD卡启动（而不是从NAND内存），如下所示。

.. |pic1| image:: ../_static/s9-jumpers.png
	    :width: 45%
	    :alt: S9 跳线

.. |pic2| image:: ../_static/s9-jumpers-board.png
	    :width: 45%
	    :alt: S9 跳线版
	    
|pic1|  |pic2|

* 将SD卡插到矿机上，开机。
* 过一会，您就应该能通过设备的IP地址进到Braiins OS+界面。

**在多个矿机上使用单个SD卡**

最近一次使用的MAC地址会存储在SD卡的覆盖分区 (Overlay Partition)上，以便检查SD卡是否插入到同一台矿机。
如果当前的MAC地址与上一次不同，网络和系统配置将被重置为默认，且 ``/etc/miner_hwid`` 文件将会被删除。

如果在NAND上存储有Braiins OS固件，HW_ID(硬件ID)则由NAND决定
如果NAND发生损坏，或它储存的是原厂固件，``/etc/miner_hwid`` 文件将会被使用（如果存在），
否则就会产生一个新的HW_ID，并直到下一次开机，新的HW_ID都会被保存到 ``/etc/miner_hwid`` 里。


将Braiins OS+ 从SD卡烧录到矿机内置储存（NAND）中
=============================================

您也可以在SD卡上运行Braiins OS+的同时，将Braiins OS+烧录到矿机内置储存（NAND）中。
如需将Braiins OS+永久烧录到NAND中，请通过SSH连接矿机并运行以下命令：


::

  miner nand_install

.. _remote_ssh_method:

***********
远程SSH方式
***********

使用*SSH方式*安装Braiins OS+，请按以下步骤：

 * *（自定义固件（Custom Firmware））* 烧录原厂固件到矿机（如果矿机上已经运行的是原厂固件，或旧版的Braiins OS则这一步可以跳过）

 * *（如是在Windows上）* 请安装Ubuntu for Windows 10 ，在 `微软商店 <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_  里可以下载。

 * 在命令行终端中运行以下的命令 （按需替换占位符  ``IP_ADDRESS`` 中的内容）：

(请注意，下方命令兼容Ubuntu和Ubuntu for Windows 10。如果您使用的是Linux的其他发行版或者别的操作系统，请您查看相应的文档并按照实际情况更改命令）。

::

  # 准备运行环境并下载固件（这一步如果已经做过了则可跳过）
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv
  wget -c https://feeds.braiins-os.com/20.03/braiins-os-plus_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46.tar.gz -O - | tar -xz && cd ./braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate && python3 -m pip install -r requirements.txt && deactivate
  
  # 在矿机上安装Braiins OS+
  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 upgrade2bos.py IP_ADDRESS

****************
安装/升级多个设备
****************
如果您需要在多个设备上进行安装或升级，可以使用我们的配置电子表格，它为不同的用例生成命令。


电子表格在 `这里 <https://docs.google.com/spreadsheets/d/1H3Zn1zSm6-6atWTzcU0aO63zvFzANgc8mcOFtRaw42E>`_ 可以下载
