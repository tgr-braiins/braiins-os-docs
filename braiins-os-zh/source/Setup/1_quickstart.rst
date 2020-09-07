###########
快速指南
###########

.. contents::
  :local:
  :depth: 2

*******************
安装Braiins OS
*******************

============================================
矿机原厂固件是2019年之前的版本
============================================

**单台矿机安装**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/PjCZIVkTuLI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

您从矿机的网页端后台就能轻松升级，请参照以下步骤：

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **网页后台方式安装包**。
  * 登陆您矿机的网页端后台，点击*System（系统） -> Upgrade（升级）*。
  * 上传您下载的安装包，并刷入固件映像。

Braiins OS将会被安装到您的矿机上。网络配置（例如静态IP地址）和矿池以及用户配置，将会自动转移到新安装的Braiins OS上，矿机自动调整功能也将自动开启。 

**批量安装**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/0RTKiqwJ4to" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

使用 **BOS工具箱** ，您可以轻松地多台矿机上批量安装Braiins OS，请参照以下步骤：

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址，一个IP地址一行！保存文本文件后，再将文件后缀从".txt"改为".csv"。并确定此文件和BOS工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS工具性的实际路径（文件夹地址），替换下方命令中的 *FILE_PATH_TO_BOS_TOOLBOX* 。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-toolbox.exe install --batch listOfMiners.csv

    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-toolbox install --batch listOfMiners.csv		

    **注：** *当在Linux系统中使用BOS工具箱时，您需要先使用以下命令让BOS工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-toolbox  

Braiins OS将会被安装到您的矿机上。网络配置（例如静态IP地址）和矿池以及用户配置，将会自动转移到新安装的Braiins OS上，矿机自动调整功能也将自动开启。 

关于此过程的更多信息，请详见文档的 :ref:`bosbox` 和 :ref:`bosbox_install` 这两个部分的内容。

==================================================
矿机原厂固件是2019年之后的版本
==================================================

如果您的矿机上的原厂固件是2019年或之后的，您只能通过SD卡刷的方法来安装Braiins OS。因为从2019年起的原厂固件为了防止第三方固件的使用，封锁了SSH连接并在网页端后台升级刷固件时要求验证签名。

通过SD卡刷方式安装Braiins OS，请参照以下步骤：

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

关于此过程的更多信息，请详见文档的 :ref:`sd` 和 :ref:`sd_install` 这两个部分的内容。

******************
更新Braiins OS
******************

**单台矿机更新**

固件每隔一段时间就会检查是否有新版本更新可用。如有可用的新版本，在矿机网页端后台里的右上角会出现一个蓝色的 **Upgrade（更新）** 按钮。点击按钮即可开始执行更新。

或者您也可以通过在矿机网页端后台中的System（系统） > Software（软件）目录中手动点击 *Update lists（更新列表）* 获取更新库信息进行更新。如果您没找到更新按钮的话，请尝试刷新网页。在 *Download and install package（下载和安装包）* 项中，输入 ``firmware`` 并点击 *OK* 触发更新。 

**批量更新**

使用 **BOS工具箱** ，您可以轻松地批量更新多台矿机上Braiins OS，请参照以下步骤：

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址，一个IP地址一行！保存文本文件后，再将文件后缀从".txt"改为".csv"。并确定此文件和BOS工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS工具性的实际路径（文件夹地址），替换下方命令中的*FILE_PATH_TO_BOS_TOOLBOX*。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-toolbox.exe update --batch listOfMiners.csv

    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-toolbox update --batch listOfMiners.csv

    **注：** *当在Linux系统中使用BOS工具箱时，您需要先使用以下命令让BOS工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-toolbox 

此命令将为 *listOfMiners.csv* 矿机列表中的矿机检查更新，如有更新可用则将为列表中的矿机更新固件。

关于此过程的更多信息，请详见文档的 :ref:`bosbox` 和 :ref:`bosbox_update` 这两个部分的内容。  

*********************
卸载Braiins OS
*********************

**单台矿机卸载**

使用 **BOS工具箱**，您可以轻松地卸载单台矿机上安装的Braiins OS，请参照以下步骤：

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **BOS工具箱** 。
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS工具性的实际路径（文件夹地址），替换下方命令中的*FILE_PATH_TO_BOS_TOOLBOX*。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS_TOOLBOX

  * 用您矿机的IP地址（或主机名）替换下方命令中的 *IP_ADDRESS*，并根据您的操作系统，运行以下相应的命令：
  
    在 **Windows** 上的命令提示行请用： ::

      bos-toolbox.exe uninstall IP_ADDRESS

     在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-toolbox uninstall IP_ADDRESS
      
    **注：** *当在Linux系统中使用BOS工具箱时，您需要先使用以下命令让BOS工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-toolbox 

此命令会让矿机回滚到没有锁死SSH版本的原厂固件，方便您远程控制矿机。

**批量卸载**

使用 **BOS工具箱** ，您可以轻松地批量卸载多台矿机上安装的Braiins OS，请参照以下步骤：

  * 在我们 `官网 <https://zh.braiins-os.com/open-source/download>`_ 上下载 **BOS工具箱** 。
  * 创建一个txt文本文件，并将文件命名为"listOfMiners"，然后在文件内输入您想执行操作的矿机的IP地址，一个IP地址一行！（矿机的IP地址在矿机网页端界面中的 *Status（状态）-> Overview（总览）中可以进行查询）。保存文本文件后，再将文件后缀从".txt"改为".csv"。确定此文件和BOS工具箱都放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）。
  * 用放置矿机地址文件和BOS工具性的实际路径（文件夹地址），替换下方命令中的*FILE_PATH_TO_BOS_TOOLBOX*。执行命令，切换到路径。 ::
  
      cd FILE_PATH_TO_BOS_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-toolbox.exe uninstall --batch listOfMiners.csv

    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-toolbox uninstall --batch listOfMiners.csv
      
    **注：** *当在Linux系统中使用BOS工具箱时，您需要先使用以下命令让BOS工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-toolbox 

此命令会让矿机回滚到没有锁死SSH版本的原厂固件，方便您远程控制矿机。

关于此过程的更多信息，请详见文档的 :ref:`bosbox` 和 :ref:`bosbox_uninstall` 这两个部分的内容。  

*********************
配置Braiins OS
*********************

**配置单台矿机**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/PjCZIVkTuLI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

您可以使用矿机的 **网页端后台** 或直接使用矿机上的 **/etc/bosminer.toml** 这个配置文件，对单台矿机上的Braiins OS进行配置（详情请见文档的 :ref:`configuration` 部分）。

**配置多台矿机**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/4jQCu6yuXUA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

使用 **BOS工具箱** ，您可以轻松地批量配置多台矿机上安装的Braiins OS，请参照文档 :ref:`bosbox_configure`部分的步骤进行配置。
