
.. _manager:

###################
Braiins OS+ Manager
###################

.. contents::
  :local:
  :depth: 1

Braiins OS+ Manager es una plataforma basada en la nube que le permite configurar sus dispositivos de minería corriendo el firmware Braiins OS+ de forma remota y también recibe continuamente datos sobre su rendimiento.

Los datos se están enviando a través del protocolo Stratum V2 y usan el mismo canal utilizado para recaudar la comisión de desarrollo, para no agobiar su red con otra conexión.

El principal objeto dentro de Braiins OS+ Manager es un grupo de dispositivos denominado Farm (Granja). Cada Granja tiene su Farm ID. Esta es una frase que debe colocar a sus dispositivos Braiins OS+ si desea conectarlos a la Granja. Una vez conectados, los dispositivos enviarán sus datos de rendimiento al Braiins OS+ Manager cada 120 segundos.

Cada Granja tiene su configuración. Cuando actualiza y guarda la nueva configuración, esta será propagada al dispositivo en lo que Manager reciba la siguiente carga con los datos de rendimiento desde el dispositivo. Ya que la misma configuración es aplicada a todos los dispositivos dentro de una Granja, **recomendamos encarecidamente que cree granjas separadas por tipo de dispositivo** o simplemente agrupar los dispositivos (incluso del mismo tipo) si desea configurarlos de manera diferente.

***********
Inscribirse
***********

Para usar Braiins OS+ Manager, simplemente `inscríbase aquí <https://manager.braiins.com/#/register>`_.

Luego de introducir su dirección de correo, le estaremos enviando un correo de confirmación. Después de seguir el enlace del correo, se le pedirá que escoja una contraseña y configurar su autenticación de dos pasos (2FA).

****************
Crear una Granja
****************

Cuando haya ingresado, empiece creando una Granja:

1. Abra el diálogo de creación de Granja haciendo clic al símbolo '+'.
2. Escoja un nobre que desee usar para su granja. Puede cambiar luego el nombre.
3. Ingrese las credenciales de minería. Podrá cambiar luego las credenciales y tambien añadir otros pools.
4. La Farm ID para su granja ha sido creada.

La Farm ID es una frase que debe poner en sus dispositivos Braiins OS+ que desee conectar a la Granja.

*************************
Connect Devices to a Farm
*************************

In order to connect a device to your Braiins OS+ Manager Farm, you need to:

  - run Braiins OS+ 21.04 or later running on the selected devices, 
  - set the Farm ID (bos_mgmt_id) on the selected devices.

Those steps can be done using BOS Toolbox with the following steps.
**Important note:** Download the latest version of BOS Toolbox `from here <https://braiins.com/os/plus/download>`_, before using the commands bellow.

**Setting Farm ID during Braiins OS+ installation**

If your devices don’t run Braiins OS+ yet, you can install the Braiins OS+ and set the Farm ID in one simple step by using BOS Toolbox’s install command with `--bos-mgmt-id` argument.
Replace the “HOSTS” placeholder with an IP address or with a text-file containing multiple IPs (one per line, for batch installation). Replace “FARM_ID” with your Farm ID.
   
::

    #Windows
    bos-toolbox.bat install --bos-mgmt-id FARM_ID HOSTS

    #Linux
    ./bos-toolbox install --bos-mgmt-id FARM_ID HOSTS

**Update existing Braiins OS+ installation and set Farm ID**

If your devices are already running Braiins OS+, use the following command to update them to the latest Braiins OS+ version and set Farm ID on them:

::

    #Windows
    bos-toolbox.bat update --bos-mgmt-id FARM_ID HOSTS

    #Linux
    ./bos-toolbox update --bos-mgmt-id FARM_ID HOSTS

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
