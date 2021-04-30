
.. _manager:

###################
Braiins OS+国家
###################

.. contents::
  :local:
  :depth: 1

Braiins OS+国家是基于云端的管理平台让您远程设置您运行Braiins OS+固件的矿机，以及不断地受到有关其性能的数据。

数据是通过阶层Stratum V2协议发送的，并使用与用于收开发费同样的信道，从而不会增加网络上的负载。

Braiins OS+管家的重要单位是一群矿机叫做矿场。每个矿场有自己的ID。ID由您在想连接的Braiins OS+设备上所定的参数组成.一旦连接，设备每个120秒钟向Braiins OS+管家发送其性能数据。

每个矿场有其配置。当您更新并保存到新配置时，一旦管家从设备收到下一次性能数据负载，配置将被传播到设备。由于一个配置用于矿场所有设备上，**我们强烈推荐您为每种矿机创建一个单独的矿场**或者为希望以不同配置的一组设备（甚至都是同一类型）创建一个单独的矿场。

*******
注册 
*******

为使用Braiins OS+国家，`点击此处注册 <https://manager.braiins.com/#/register>`_.

输入电子邮件地址后，我们将给您发送确认电子邮件。 单击电子邮件中的链接后，系统将提示您选择密码并设置两布验证。

*************
创建矿场
*************

登录后，首先创建矿场：

1. 点击'+'符号打开矿场创建对话框。
2. 选择您想使用的矿场名字。以后可以改变它。
3. 输入挖矿登录消息。您以后可以更改登录消息以及添加其他矿池。
4. 您矿场的ID已创建。

矿场ID由您想在连接到矿场的Braiins OS+设备要设置的参数组成。

*************************
将设备连接到矿场
*************************

为将设备连接到您的Braiins OS+矿机矿场，您需要：

  - 在所选的设备上运行Braiins OS+股价21.04或者更新的版本  
  - 在所选的设备上设置矿场ID（bos_mgmt_id）

按照以下的步骤可以用BOS工具箱来执行则和谐操作。T
**重要提醒:** 在使用以下的命令之前，`从这里<https://manager.braiins.com/#/register>`_下载BOS工具箱的最新版本。

**在Braiins OS+安装中设置矿场ID**

如果您的设备尚未使用Braiins OS+，您可以安装Braiins OS+并在一个简单的步骤设置矿场ID使用BOS工具箱的安装命令跟`--bos-mgmt-id`参数。
用IP地址或者包含多个IP地址的文本文件（一个IP一行，为批量安装）替换“HOSTS”。用您矿场ID替换“FARM_ID”。
   
::

    #Windows
    bos-toolbox.bat install --bos-mgmt-id FARM_ID HOSTS

    #Linux
    ./bos-toolbox install --bos-mgmt-id FARM_ID HOSTS

**更新已存在的Braiins OS+安装并设置矿场**

如果您设备上已经运行Braiins OS+，使用以下的命令将它们更新到Braiins OS+最版本并其上设置矿场ID：

::

    #Windows
    bos-toolbox.bat update --bos-mgmt-id FARM_ID HOSTS

    #Linux
    ./bos-toolbox update --bos-mgmt-id FARM_ID HOSTS

******************
配置矿场
******************

**矿工名设置**

有关矿场中包含的设备如何在管家设备聊表和矿池端中表示自己，有三种不同的选项：

  - Per Device (FARMNAME_IP4) - 默认 - 矿工名由矿场名字和设备IP地址第四个部分组成 
  - Per Device (FARMNAME_IP3_IP4) - 此外，也添加了IP地址的第三部分
  - Per Device (FARMNAME_IP2_IP3_IP4) - 此外，也添加了IP地址的第二部分
  - Single (FARMNAME) - 所有设备都使用同样矿工名（矿场的名字）。这意味着所有算力汇聚到矿池端的一个矿工。

矿工名可以随时改变

**挖矿配置**

The mining configuration available in the “Configuration” tab includes a sub-set of `general Braiins OS\+ configuration <https://docs.braiins.com/os/plus-en/Configuration/index_configuration.html>`_ available on individual devices. For example, options for individual hash chains are not available here since it only makes sense from an individual device perspective. Other than that, all the important options to configure tuning, target temperatures or dynamic power scaling are present.

The configuration requires you to input credentials for at least one pool (which is done during the farm creation process). Other configuration fields are optional. If you don't provide any value, each Device in a Farm will simply use its default. It is behavior equivalent to leaving the config of a single Braiins OS+ device empty.

Once you click on the Save button, the new configuration is propagated to the devices included in the Farm almost immediately - typically within one second.

**Local Changes**

Local changes (on the miner) are always overwritten by the the Manager. If you wish to take control of the device, disconnect it from the Farm first.

******************************
Disconnect Devices from a Farm
******************************

If you wish to disconnect the devices from the Farm and configure them individually, you can do it by simply removing the bos_mgmt_id file from selected devices. For multiple devices, this can be done using BOS Toolbox as follows:

::

    #Windows
    bos-toolbox.bat command -o HOSTS "rm /etc/bos_mgmt_id && /etc/init.d/bosminer restart"
    
    #Linux
    ./bos-toolbox command -o HOSTS "rm /etc/bos_mgmt_id && /etc/init.d/bosminer restart"

***************
Troubleshooting
***************

**1. Check if the device runs Braiins OS+ 21.04 or later**

  - Using GUI: the version is displayed in the footer
  - Using CLI: the version is displayed on the SSH welcome screen

**Fix:** if your run older Braiins OS+ version, update your devices first

**2. Check if the Farm ID has been correctly configured**

Using GUI:

  - go to Status -> Overview -> Miner
  - Check if the correct Farm ID is present in the *BOS Management ID* field.
  - If the field is not present at all, no Farm ID is configured on the device.

Using CLI:

  - `cat /etc/bos_mgmt_id`
  - the command should return the Farm ID

**Fix**: if the ID is not present or is incorrect, try to set it again

**3. Reboot your device**

Still doesn’t work? Reboot your device.

  - Using GUI: System -> Reboot -> Perform Reboot
  - Using CLI: `reboot`

**4. Contact the support team**

If nothing mentioned above has helped, `submit a support ticket <https://help.slushpool.com/en/support/tickets/new>`_. 

For effective troubleshooting, include the following information:

  - **Hardware ID** (Status -> Overview)
  - **System Log** (Status -> System Log)
