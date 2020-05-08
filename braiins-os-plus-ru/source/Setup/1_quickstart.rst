#############
Быстрый старт
#############

.. contents::
  :local:
  :depth: 2

**********************
установка Braiins OS+
**********************

============================================
Соковая прошивка, выпущенной до 2019 года
============================================

**Установка на одного устройство**

Вы можете легко установить Braiins OS+ через веб-интерфейс. Для этого выполните следующие действия:

  * Скачайте **ВЕБ-ПАКЕТ** с нашего `веб-сайта <https://braiins-os.com/plus/download/>`_.
  * Зайди на свой майнер и зайди в раздел *System -> Upgrade*.
  * Загрузите загруженный пакет и прошейте образ.

Braiins OS+ будет установлен на майнер. Конфигурация сети (например, статический IP-адрес), а также пул и пользовательские настройки будут автоматически перенесены в Braiins OS+, и автонастройка будет включена.

**Установка на несколько устройств**

Установка Braiins OS+ может быть легко выполнена с помощью BOS+ Toolbox. Для этого выполните следующие действия:

  * Скачайте **BOS+ Toolbox** с нашего `веб-сайта <https://braiins-os.com/plus/download/>`_.
  * Создайте новый текстовый файл, измените ".txt" окончание на ".csv" и вставьте IP-адреса, на которых вы хотите выполнить команды. Поместите этот файл в каталог, где находится BOS+ Toolbox. Используйте только один IP-адрес в строке!
  * После того, как вы загрузили BOS+ Toolbox, откройте командную строку (например, CMD для Windows, Terminal для Ubuntu и т.д.)
  * Замените *FILE_PATH_TO_BOS+_TOOLBOX* заполнитель в приведенной ниже команде с фактическим путем к файлу, в котором вы сохранили BOS+ Toolbox. Затем переключитесь на путь к файлу, выполнив команду: ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * Теперь замените *listOfMiners.csv* заполнитель с вашим именем файла в команде ниже и выполните соответствующую команду для вашей операционной системы:

    Для командной строки **Windows**: ::

      bos-plus-toolbox.exe install --batch listOfMiners.csv

    Для командной строки **Linux**: ::
      
      ./bos-plus-toolbox install --batch listOfMiners.csv		

    **Note:** *when using BOS+ Toolbox for Linux, you need to make it executable with the following command (this has to be done only once):* ::
  
      chmod u+x ./bos-plus-toolbox  

Braiins OS+ will be installed on the miner. The network configuration (e.g. Static IP address) and the pool and user settings will be automatically migrated to Braiins OS+ and autotuning will be turned on.

For more information about this process, and for more options visit the sections :ref:`bosbox` and :ref:`bosbox_install`.

==================================================
Running stock firmware released in 2019 or later
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
Uninstall Braiins OS+
*********************

**Single device uninstallation**

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

**Multiple device uninstallation**

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
Configure Braiins OS+
*********************

**Single device configuration**

You can configure Braiins OS+ on single device using the **web interface** of the miner or directly in the configuration file located in **/etc/bosminer.toml** (for more information, visit the :ref:`configuration` section).

**Multiple device configuration**

You can easily configure Braiins OS+ on multiple devices using the **BOS+ Toolbox**. In order to do so, follow the steps in the section :ref:`bosbox_configure`.
