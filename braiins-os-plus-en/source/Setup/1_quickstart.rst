###########
Quick Start
###########

.. contents::
  :local:
  :depth: 2

.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

*******************
Install Braiins OS+
*******************

============================================
Running stock firmware released before 2019
============================================

**GUI BOS+ Toolbox installation**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ya171-K_s4U" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

**Single device installation**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/PjCZIVkTuLI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

You can easily install Braiins OS+ via the web interface upgrade process. In order to do so, follow the steps bellow:

  * Download the **Web Package** from our `website <https://braiins-os.com/plus/download/>`_.
  * Login on your miner and go to the section *System -> Upgrade*.
  * Upload the downloaded package and flash the image.

Braiins OS+ will be installed on the miner. The network configuration (e.g. Static IP address) and the pool and user settings will be automatically migrated to Braiins OS+ and autotuning will be turned on.

**Multiple-device installation**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/0RTKiqwJ4to" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Installation of Braiins OS+ can easily be done using the BOS Toolbox. In order to do so, follow the steps bellow:

  * Download **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file, change the ".txt" ending to ".csv" and insert the IP addresses on which you want execute the commands. Use only one IP address per line!
  * Once you have downloaded BOS Toolbox, run it by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Install** section, fill the **Miner(s)** option by selecting the created text file and press **Start**.

Braiins OS+ will be installed on the miner. The network configuration (e.g. Static IP address) and the pool and user settings will be automatically migrated to Braiins OS+ and autotuning will be turned on.

For more information about this process, and for more options visit the sections :ref:`bosbox` and :ref:`bosbox_install`.

Braiins OS+ is also supported by BTC Tools - batch management tool for miners. BTC Tools for Windows/Linux can be downloaded here `here <https://btccom.zendesk.com/hc/en-us/articles/360020105012>`_. On the same page, documenation on how to use BTC Tools is available.

==================================================
Running stock firmware released in 2019 or later
==================================================

**(Antminer S9 only) Unlock SSH and install using BOS Toolbox**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ya171-K_s4U" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

In 2019, the SSH connection was locked and the signature verification in the web interface prevents the usage of 3rd party firmwares. In order to install Braiins OS+ on machines with locked SSH, follow the steps bellow:

  * Download **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file, change the ".txt" ending to ".csv" and insert the IP addresses on which you want execute the commands. Put that file in the directory where the BOS Toolbox is located. Use only one IP address per line!
  * Once you have downloaded BOS Toolbox, run it by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Install** section, fill the **Miner(s)** option by selecting the created text file, fill the **Password** field with your password and press **Start**.

Braiins OS will be installed on the miner. The network configuration (e.g. Static IP address) and the pool and user settings will be automatically migrated to Braiins OS.

**SD method**

If you are running stock firmware that was released in 2019 and later, the only way to install Braiins OS+ is to insert an SD card with Braiins OS+ flashed on it. In 2019, the SSH connection was locked and the signature verification in the web interface prevents the usage of 3rd party firmwares.

In order to install Braiins OS+ via the SD card method, follow the steps bellow:

 * Download the SD card image from our `website <https://braiins-os.com/plus/download/>`_.
 * Flash the downloaded image on an SD card (e.g. using `Etcher <https://etcher.io/>`_). *Note: Simple copy to SD card will not work. The SD card has to be flashed!*
 * **(Antminer S9 only)** Adjust the jumpers to boot from SD card (instead of NAND memory), as shown below.

  .. |pic1| image:: ../_static/s9-jumpers.png
      :width: 45%
      :alt: S9 Jumpers

  .. |pic2| image:: ../_static/s9-jumpers-board.png
      :width: 45%
      :alt: S9 Jumpers Board

  |pic1|  |pic2|

 * Insert the SD card into the device, then start the device.
 * If the auto-install version of SD image was used, the system will be automatically installed to the internal memory (NAND). The installation is completed, once both LEDs start to blink at the same time. After the installation completes, remove the SD card to boot Braiins OS+ from the NAND.
 * After a moment, you should be able to access the Braiins OS+ interface through the device’s IP address.

For more information about this process, and for more options visit the sections :ref:`sd` and :ref:`sd_install`.

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

**Multiple device update**

Updating Braiins OS+ on multiple devices at once can easily be done using the **BOS Toolbox**. In order to do so, follow the steps bellow:

  * Download the **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file, change the ".txt" ending to ".csv" and insert the IP addresses on which you want execute the commands. Use only one IP address per line! Put that file in the directory where the BOS Toolbox is located.
  * Once you have downloaded BOS Toolbox, run it by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Update** section, fill the **Miner(s)** option by selecting the created text file and press **Start**.

This command will look for an update for the miners that are specified in the created text file and update them if there is a new version of firmware.

For more information about this process, and for more options visit the sections :ref:`bosbox` and :ref:`bosbox_update`.   

*********************
Uninstall Braiins OS+
*********************

**Single device uninstallation**

You can easily uninstall Braiins OS+ on a single device using the **BOS Toolbox**. In order to do so, follow the steps bellow:

  * Download the **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Once you have downloaded BOS Toolbox, run it by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Uninstall** section, fill the **Miner(s)** option with the IP address of the miner and press **Start**.

This will revert back to stock firmware. It will automatically install an older version where the SSH was not locked, so you can access your miner remotely.

**Warning:** The stock firmware that's installed when you uninstall Braiins OS+ is not suitable for mining! Upgrade to a newer version of stock firmware for your specific hardware model before you start mining.

**Multiple device uninstallation**

You can easily uninstall Braiins OS+ on multiple devices using the **BOS Toolbox**. In order to do so, follow the steps below:

  * Download the **BOS Toolbox** from our `website <https://braiins-os.com/plus/download/>`_.
  * Create a new text file in your text editor and insert the IP addresses on which you want execute the commands. Use only one IP address per line! (Note that you can find the IP address in the Braiins OS+ web interface by going to *Status -> Overview*.) Then save the file in the same directory as you saved the BOS Toolbox and change the ".txt" ending to ".csv". 
  * Once you have downloaded BOS Toolbox, run it by double-clicking (Windows) or by running ``./bos-toolbox`` in command-line interpreter (Linux).
  * In the **Uninstall** section, fill the **Miner(s)** option by selecting the created text file and press **Start**.

This will revert back to stock firmware. It will automatically install an older version where the SSH was not locked, so you can access your miner remotely.

**Warning:** The stock firmware that's installed when you uninstall Braiins OS+ is not suitable for mining! Upgrade to a newer version of stock firmware for your specific hardware model before you start mining.

For more information about this process, and for more options visit the sections :ref:`bosbox` and :ref:`bosbox_uninstall`.

*********************
Configure Braiins OS+
*********************

**Single device configuration**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/PjCZIVkTuLI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

You can configure Braiins OS+ on single device using the **web interface** of the miner or directly in the configuration file located in **/etc/bosminer.toml** (for more information, visit the :ref:`configuration` section).

**Multiple device configuration**

.. raw:: html

  <iframe width="560" height="315" src="https://www.youtube.com/embed/4jQCu6yuXUA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

You can easily configure Braiins OS+ on multiple devices using the **BOS Toolbox**. In order to do so, follow the steps in the section :ref:`bosbox_configure`.
