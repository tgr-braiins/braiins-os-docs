
.. _manager:

###################
Braiins OS+ Manager
###################

.. contents::
  :local:
  :depth: 1

Braiins OS+ Manager is a cloud-based platform that allows you to remotely configure your mining devices running the Braiins OS+ firmware as well as continuously receive data about their performance.

The data are being sent over the Stratum V2 protocol and using the same channel that is used for collecting the dev fee, thus not burdening your network with another connection.

The main object in the Braiins OS+ Manager is a group of devices called Farm. Every Farm has its Farm ID. It is a string you have to set on your Braiins OS+ devices if you wish to connect them to the Farm. Once connected, the devices send their performance data to the Braiins OS+ Manager every 120 seconds.

Every Farm has its configuration. When you update and save new configuration, it will be propagated to a device once the Manager receives next performance data payload from the device. Since the same config is applied to all devices in a Farm, **we strongly recommend that you create a separate farm for each device type** or simply a group of devices (even of the same type) you wish to configure differently.

*******
Sign Up
*******

To use Braiins OS+ Manager, simply `signup here <https://manager.braiins.com/#/register>`_.

After you enter your email address, we will send you confirmation email. After following the link in the email, you will be prompted to choose your password and setup two-factor authentication.

*************
Create a Farm
*************

Once you are logged, start with creating a Farm:

1. Open the Farm creation dialogue by clicking on the '+' symbol.
2. Choose a name you wish to use for your farm. You can change the name later.
3. Enter mining credentials. You will be able to change the credentials later as well as add other pools.
4. The Farm ID for your farm has been created.

The Farm ID is a string you have to set on your Braiins OS+ devices you wish to connect to the Farm. 

*************************
Connect Devices to a Farm
*************************

In order to connect a device to your Braiins OS+ Manager Farm, you need to:

  - run Braiins OS+ 21.04 or later running on the selected devices, 
  - set the Farm ID (bos_mgmt_id) on the selected devices.

Those steps can be done using BOS Toolbox with the following steps.
**Important note:** Download the latest version of BOS Toolbox `from here <https://braiins.com/os/plus/download>`_, before using the commands below.

**Install/Update Braiins OS+ and set Farm ID**

If your devices don’t run Braiins OS+ 21.04 (or higher) yet, you can install the latest Braiins OS+ and set the Farm ID in one simple step by using BOS Toolbox’s install command with `--bos-mgmt-id` argument.
Replace the “HOSTS” placeholder with an IP address or with a text-file containing multiple IPs (one per line, for batch installation). Replace “FARM_ID” with your Farm ID.
   
::

    #Windows
    bos-toolbox.bat install --bos-mgmt-id FARM_ID HOSTS

    #Linux
    ./bos-toolbox install --bos-mgmt-id FARM_ID HOSTS

******************
Configure the Farm
******************

**Workername Setup**

There are three different options on how the devices included in a Farm can identify themselves in the Manager device list and on the pool side:

  - Per Device (FARMNAME_IP4) - default - workername consists of the name of the Farm and fourth segment of IP address of a device
  - Per Device (FARMNAME_IP3_IP4) - in addition, the third segment of the IP address is also included
  - Per Device (FARMNAME_IP2_IP3_IP4) - in addition, the second segment of the IP address is also included
  - Single (FARMNAME) - All devices use the same workername (name of the Farm). This means that the hash rate is aggregated to one worker on the pool side.

The workername mode may be changed anytime.

**Mining Configuration**

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
