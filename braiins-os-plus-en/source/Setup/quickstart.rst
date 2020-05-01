###########
Quick Start
###########

.. contents::
  :local:
  :depth: 2

*******************
Install Braiins OS+
*******************

============================================
Running stock firmware released before 2019
============================================

**Single device installation**

You can easily install Braiins OS+ via the web interface upgrade process. In order to do so, follow the steps bellow:

  * Download the **Web Package** from our `website <https://braiins-os.com/>`_.
  * Login on your miner and go to the section *System -> Upgrade*.
  * Upload the downloaded package and flash the image.

Braiins OS+ will be installed on the miner. The network configuration (e.g. Static IP address) and the pool and user settings will be automatically migrated to Braiins OS+ and autotuning will be turned on.

**Multiple-device installation**

Installation of Braiins OS+ can easily be done using the Braiins OS+ Box (TODO:edit name). In order to do so, follow the steps bellow:

  * Download Braiins OS+ Box (TODO:edit name) from our `website <https://braiins-os.com/>`_. and open it.
  * In the Braiins OS+ Box (TODO:edit name), type the following command (replace the IP_ADDRESS placeholder accordingly) (TODO: specify how to list multiple IP addresses)

  ::

    install IP_ADDRESS

Braiins OS+ will be installed on the miner. The network configuration (e.g. Static IP address) and the pool and user settings will be automatically migrated to Braiins OS+ and autotuning will be turned on.

For more information about this process, and for more options visit the section TODO LINK AND NAME OF UPGRADE2BOS SECTION.

==================================================
Running stock firmware released in 2019 or later
==================================================

If you are running stock firmware that was released in 2019 and later, the only way to install Braiins OS+ is to insert an SD card with Braiins OS+ flashed on it. In 2019, the SSH connection was locked and the signature verification in the web interface prevents the usage of 3rd party firmwares.

In order to install Braiins OS+ via the SD card method, follow the steps bellow:
 * Download the SD card image from our `website <https://braiins-os.com/>`_.
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
 * *[Optional]:* You can now install Braiins OS+ to the internal memory (NAND) following the steps described here. TODO LINK

For more information about this process, and for more options visit the section TODO LINK AND NAME OF sd SECTION.

******************
Update Braiins OS+
******************

**Single device update**

The firmware periodically checks for availability of a new version. In
case of a new version being available a blue **Upgrade** button appears in the web interface, on
the right side of the top bar. Proceed to click on the button and
confirm to start the upgrade.

Alternatively, you can update the repository information manually by
clicking the *Update lists* button in the System > Software menu. In
case the button is missing, try to refresh the page. To trigger the
upgrade process, type ``firmware`` into the *Download and install
package* field and press *OK*.

For more information about this process, and for more options visit the section TODO LINK AND NAME OF web package SECTION.

**Multiple-device update**

Updating Braiins OS+ on multiple devices at once can easily be done using the Braiins OS+ Box (TODO:edit name). In order to do so, follow the steps bellow:

  * Download Braiins OS+ Box (TODO:edit name) from our `website <https://braiins-os.com/>`_. and open it.
  * In the Braiins OS+ Box (TODO:edit name), type the following command (replace the IP_ADDRESS placeholder accordingly) (TODO how to specify multiple IPs)

  ::

    update IP_ADDRESS

This will check for a new version and update Braiins OS+ if possible. 

For more information about this process, and for more options visit the section TODO LINK AND NAME OF opkg SECTION.   

*********************
Uninstall Braiins OS+
*********************

Uninstallation of Braiins OS+ can easily be done using the Braiins OS+ Box (TODO:edit name). In order to do so, follow the steps bellow:

  * Download Braiins OS+ Box (TODO:edit name) from our `website <https://braiins-os.com/>`_. and open it.
  * In the Braiins OS+ Box (TODO:edit name), type the following command (replace the IP_ADDRESS placeholder accordingly)

  ::

    uninstall IP_ADDRESS

This will revert back to stock firmware. It will automatically install an older version, where the SSH was not locked, so you can access your miner remotely.

For more information about this process, and for more options visit the section TODO LINK AND NAME OF restore2factory SECTION.