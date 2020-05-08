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

**单一矿机安装**

您从矿机网页端后台就能轻松升级。请参照以下步骤：

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载 **网页后台方式安装包**。
  * 登陆您矿机的网页端后台，点击*System（系统） -> Upgrade（升级）*。
  * 上传您下载的安装包，并刷入固件映像。

Braiins OS+就会被安装到您的矿机上。网络配置（如静态IP地址）和矿池以及用户设置，将会自动转移到新安装的Braiins OS+上，矿机自动调整功能也将自动开启。 

**多矿机安装**

使用BOS+工具箱，您可以轻松地在多个矿机上配置Braiins OS+。请参照以下步骤：

  * 在我们 `官网 <https://zh.braiins-os.com/plus/download/>`_ 上下载**BOS+工具箱**。
  * 创建一个文本文件，命名文件为"listOfMiners"并将文件后缀从".txt"改为".csv"。在文件内输入您想执行操作的矿机的IP地址，一个IP地址一行！然后将文件和BOS+工具箱放在同一路径下（同一文件夹中）。 
  * 使用命令行（Windows操作系统的CMD，Ubuntu的Terminal终端等）
  * 用放置矿机地址文件和BOS+工具性的实际路径（文件夹地址），替换下方命令中的*FILE_PATH_TO_BOS+_TOOLBOX*。执行命令，切换到路径。 ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * 然后根据您的操作系统，运行以下相应的命令：

    **Windows** 命令提示行请用： ::

      bos-plus-toolbox.exe install --batch listOfMiners.csv

    **Linux** Terminal控制终端请用： ::
      
      ./bos-plus-toolbox install --batch listOfMiners.csv		

    **请注意：** *当在Linux系统中使用BOS+工具箱时，when using BOS+ Toolbox for Linux, you need to make it executable with the following command (this has to be done only once):* ::
  
      chmod u+x ./bos-plus-toolbox  

Braiins OS+ will be installed on the miner. The network configuration (e.g. Static IP address) and the pool and user settings will be automatically migrated to Braiins OS+ and autotuning will be turned on.

For more information about this process, and for more options visit the sections :ref:`bosbox` and :ref:`bosbox_install`.

==================================================
在矿机上是2019年之后版本的原厂固件的情况下
==================================================

If you are running stock firmware that was released in 2019 and later, the only way to install Braiins OS+ is to insert an SD card with Braiins OS+ flashed on it. In 2019, the SSH connection was locked and the signature verification in the web interface prevents the usage of 3rd party firmwares.

In order to install Braiins OS+ via the SD card method, follow the steps bellow:

 * Download the SD card image from our `website <https://braiins-os.com/plus/download/>`_.
 * Flash the downloaded image on an SD card (e.g. using `Etcher <https://etcher.io/>`_). *Note: Simple copy to SD card will not work. The SD card has to be flashed!*
 * Adjust the jumpers to boot from SD card (instead of NAND memory), as shown below.

  .. |pic1| image:: ../_static/s9-jumpers.png
      :width: 45%
      :alt: S9 Jumpers

  .. |pic2| image:: ../_static/s9-jumpers-board.png
      :width: 45%
      :alt: S9 Jumpers Board

  |pic1|  |pic2|

 * Insert the SD card into the device, then start the device.
 * After a moment, you should be able to access the Braiins OS+ interface through the device’s IP address.
 * *[Optional]:* You can now install Braiins OS+ to the internal memory (NAND) following the section :ref:`sd_nand_install`.

For more information about this process, and for more options visit the sections :ref:`sd` and :ref:`sd_install`.

******************
更新Braiins OS+
******************

**单一矿机更新**

The firmware periodically checks for availability of a new version. In
case of a new version being available a blue **Upgrade** button appears in the web interface, on
the right side of the top bar. Proceed to click on the button and
confirm to start the upgrade.

Alternatively, you can update the repository information manually by
clicking the *Update lists* button in the System > Software menu. In
case the button is missing, try to refresh the page. To trigger the
upgrade process, type ``firmware`` into the *Download and install
package* field and press *OK*.

**多矿机更新**

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

**单一矿机卸载**

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

**多矿机卸载**

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

**单一矿机配置**

You can configure Braiins OS+ on single device using the **web interface** of the miner or directly in the configuration file located in **/etc/bosminer.toml** (for more information, visit the :ref:`configuration` section).

**多矿机配置**

You can easily configure Braiins OS+ on multiple devices using the **BOS+ Toolbox**. In order to do so, follow the steps in the section :ref:`bosbox_configure`.
