###########
快速指南
###########

.. contents::
  :local:
  :depth: 2

*******************
安装Braiins OS+
*******************

============================================
在矿机上是2019年之前版本的原厂固件的情况下
============================================

**单台矿机安装**

您从矿机的网页端后台就能轻松升级，请参照以下步骤：

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **网页后台方式安装包**。
  * 登陆您矿机的网页端后台，点击*System（系统） -> Upgrade（升级）*。
  * 上传您下载的安装包，并刷入固件映像。

Braiins OS+将会被安装到您的矿机上。网络配置（例如静态IP地址）和矿池以及用户设置，将会自动转移到新安装的Braiins OS+上，矿机自动调整功能也将自动开启。 

**多台矿机安装**

使用BOS+工具箱，您可以轻松地在多个矿机上配置Braiins OS+，请参照以下步骤：

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载**BOS+工具箱**。
  * 创建一个文本文件，命名文件名为"listOfMiners"，并将文件后缀从".txt"改为".csv"。在文件内输入您想执行操作的矿机的IP地址，一个IP地址一行！然后将文件和BOS+工具箱放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）
  * 用放置矿机地址文件和BOS+工具性的实际路径（文件夹地址），替换下方命令中的*FILE_PATH_TO_BOS+_TOOLBOX*。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    在 **Windows** 上的命令提示行请用： ::

      bos-plus-toolbox.exe install --batch listOfMiners.csv

    在 **Linux** 上的Terminal控制终端请用： ::
      
      ./bos-plus-toolbox install --batch listOfMiners.csv		

    **请注意：** *当在Linux系统中使用BOS+工具箱时，您需要先使用以下命令让BOS+工具箱变得可执行（一次就够）：* ::
  
      chmod u+x ./bos-plus-toolbox  

Braiins OS+将会被安装到您的矿机上。网络配置（例如静态IP地址）和矿池以及用户设置，将会自动转移到新安装的Braiins OS+上，矿机自动调整功能也将自动开启。 

关于此过程的更多信息，请详见 :ref:`bosbox` 和 :ref:`bosbox_install` 这两个部分的内容。

==================================================
在矿机上是2019年之后版本的原厂固件的情况下
==================================================

如果您的矿机上的原厂固件是2019年或之后的，您只能通过SD卡刷的方法来安装Braiins OS。因为从2019年起的原厂固件为了防止第三方固件的使用，封锁了SSH连接并在网页端后台升级刷固件时要求验证签名。

通过SD卡刷方式安装Braiins OS+，请参照以下步骤：

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

关于此过程的更多信息，请详见 :ref:`sd` 和 :ref:`sd_install` 这两个部分的内容。

******************
更新Braiins OS+
******************

**单台矿机更新**

The firmware periodically checks for availability of a new version. In
case of a new version being available a blue **Upgrade** button appears in the web interface, on
the right side of the top bar. Proceed to click on the button and
confirm to start the upgrade.

Alternatively, you can update the repository information manually by
clicking the *Update lists* button in the System > Software menu. In
case the button is missing, try to refresh the page. To trigger the
upgrade process, type ``firmware`` into the *Download and install
package* field and press *OK*.

**多台矿机更新**

Updating Braiins OS+ on multiple devices at once can easily be done using the **BOS+ Toolbox**. In order to do so, follow the steps bellow:

  * Download the **BOS+ Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file, change the ".txt" ending to ".csv" and insert the IP addresses on which you want execute the commands. Put that file in the directory where the BOS+ Toolbox is located.
  * Once you have downloaded BOS+ Toolbox, open your command-line interpreter (e.g. CMD for Windows, Terminal for Ubuntu, etc.) 
  * Replace the *FILE_PATH_TO_BOS+_TOOLBOX* placeholder in the command below with the actual file path where you saved the BOS+ Toolbox. Then switch to that file path by running the command: ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * Now replace the *listOfMiners.csv* placeholder with your file name in the command below and run the appropriate command for your operating system:

    For **Windows** command terminal: ::

      bos-plus-toolbox.exe update --batch listOfMiners.csv

    For **Linux** command terminal: ::
      
      ./bos-plus-toolbox update --batch listOfMiners.csv

    **Note:** *when using BOS+ Toolbox for Linux, you need to make it executable with the following command (this has to be done only once):* ::
  
      chmod u+x ./bos-plus-toolbox 

This command will look for an update for the miners that are specified in the *listOfMiners.csv* and update them if there is a new version of firmware.

For more information about this process, and for more options visit the sections :ref:`bosbox` and :ref:`bosbox_update`.   

*********************
卸载Braiins OS+
*********************

**单台矿机卸载**

You can easily uninstall Braiins OS+ on a single device using the **BOS+ Toolbox**. In order to do so, follow the steps bellow:

  * Download the **BOS+ Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Once you've downloaded the BOS+ Toolbox, open your command-line interpreter (e.g. CMD for Windows, Terminal for Ubuntu, etc.)
  * Replace the *FILE_PATH_TO_BOS+_TOOLBOX* placeholder in the command below with the actual file path where you saved the BOS+ Toolbox. Then switch to that file path by running the command: ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * Now replace the *IP_ADDRESS* placeholder with your miner's IP address (or host name) in the command below and run the appropriate command for your operating system:

    For **Windows** command terminal: ::

      bos-plus-toolbox.exe uninstall IP_ADDRESS

    For **Linux** command terminal: ::
      
      ./bos-plus-toolbox uninstall IP_ADDRESS
      
    **Note:** *when using BOS+ Toolbox for Linux, you need to make it executable with the following command (this has to be done only once):* ::
  
      chmod u+x ./bos-plus-toolbox 

This will revert back to stock firmware. It will automatically install an older version where the SSH was not locked, so you can access your miner remotely.

**多台矿机卸载**

You can easily uninstall Braiins OS+ on multiple devices using the **BOS+ Toolbox**. In order to do so, follow the steps below:

  * Download the **BOS+ Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file in your text editor and insert the IP addresses on which you want execute the commands. Each IP address should be separated by a comma. (Note that you can find the IP address in the Braiins OS+ web interface by going to *Status -> Overview*.) Then save the file in the same directory as you saved the BOS+ Toolbox and change the ".txt" ending to ".csv". 
  * Once you have downloaded BOS+ Toolbox and saved the .csv file, open your command-line interpreter (e.g. CMD for Windows, Terminal for Ubuntu, etc.).
  * Replace the *FILE_PATH_TO_BOS+_TOOLBOX* placeholder in the command below with the actual file path where you saved the BOS+ Toolbox. Then switch to that file path by running the command: ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * Now replace the *listOfMiners.csv* placeholder with your file name in the command below and run the appropriate command for your operating system:

    For **Windows** command terminal: ::

      bos-plus-toolbox.exe uninstall --batch listOfMiners.csv

    For **Linux** command terminal: ::
      
      ./bos-plus-toolbox uninstall --batch listOfMiners.csv
      
    **Note:** *when using BOS+ Toolbox for Linux, you need to make it executable with the following command (this has to be done only once):* ::
  
      chmod u+x ./bos-plus-toolbox 

This will revert back to stock firmware. It will automatically install an older version where the SSH was not locked, so you can access your miner remotely.

For more information about this process, and for more options visit the sections :ref:`bosbox` and :ref:`bosbox_uninstall`.

*********************
配置Braiins OS+
*********************

**单台矿机配置**

You can configure Braiins OS+ on single device using the **web interface** of the miner or directly in the configuration file located in **/etc/bosminer.toml** (for more information, visit the :ref:`configuration` section).

**多台矿机配置**

You can easily configure Braiins OS+ on multiple devices using the **BOS+ Toolbox**. In order to do so, follow the steps in the section :ref:`bosbox_configure`.
