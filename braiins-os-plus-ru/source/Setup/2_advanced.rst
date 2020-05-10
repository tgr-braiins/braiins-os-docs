#######################
Расширенное руководство
#######################

.. contents::
	:local:
	:depth: 1

*************
Разветвление
*************

Существует множество инструментов, пакетов и скриптов, которые можно использовать для Braiins OS. Для лучшей навигации используйте следующее:

 * Установка Braiins OS+
 
  * Используя BOS+ Toolbox (:ref:`bosbox_install`)
  * Используя веб-пакет (:ref:`web_package_install`)
  * Используя SD карту (:ref:`sd_install`)
  * Используя SD карту и miner tool (:ref:`miner_nand_install`)
  * Используя SSH скрипты (:ref:`ssh_package_install`)
  
 * Обновление Braiins OS+
 
  * Используя BOS+ Toolbox (:ref:`bosbox_update`)
  * Используя OPKG (:ref:`opkg_update`)
  * Используя пакет sysupgrade (:ref:`sysupgrade_switch_braiinsplus`)
  * Используя bos2bos скрипт (:ref:`bos2bos`)
  
 * Переход на Braiins OS (версия без автонастройки)
 
  * Используя пакет sysupgrade (:ref:`sysupgrade_switch_braiinsos`)
  * Используя bos2bos скрипт (:ref:`bos2bos`)
  
 * Переход на Braiins OS+ (версия с автонастройкой)
 
  * Используя OPKG (:ref:`opkg_switch_to_braiinsplus`)
  * Используя пакет sysupgrade (:ref:`sysupgrade_switch_braiinsplus`)
  * Используя bos2bos скрипт (:ref:`bos2bos`)
  
 * Сброс к исходной версии Braiins OS (версия, которая была впервые установлена на устройстве) - возврат к заводским настройкам
 
  * Используя OPKG (:ref:`opkg_factory_reset`)
  * Используя SD карту (:ref:`sd_factory_reset`)
  * Используя "miner" tool (:ref:`miner_factory_reset`)
  * Используя bos2bos скрипт (:ref:`bos2bos`)
  
 * Деинсталляция Braiins OS+
 
  * Используя BOS+ Toolbox (:ref:`bosbox_uninstall`)
  * Используя SSH скриптs (:ref:`ssh_package_uninstall`)

.. _bosbox:

***************
BOS+ Toolbox
***************

BOS+ Toolbox - это новый инструмент, который позволяет пользователю легко устанавливать, удалять, обновлять, обнаруживать и настраивать  Braiins OS+. Это также позволяет делать это в пакетном режиме, что упрощает управление большим количеством устройств. Это рекомендуемый способ управления вашими устройствами.

===========
Применение
===========

  * Скачайте **BOS+ Toolbox** с нашего `веб-сайта <https://braiins-os.com/>`_.
  * Создайте новый текстовый файл, измените ".txt" окончание на ".csv" и вставьте IP-адреса, на которых вы хотите выполнить команды. Поместите этот файл в каталог, где находится BOS+ Toolbox. **Используйте только один IP-адрес в строке!**
  * Следуйте разделам ниже

=========================================
Особенности, плюсы и минусы этого метода:
=========================================

  + дистанционная установка Braiins OS+
  + дистанционное обновление Braiins OS+
  + дистанционное удаление Braiins OS+
  + дистанционная конфигурация Braiins OS+
  + сканирование сети на наличие устройств
  + переносит всю конфигурацию по умолчанию (можно настроить) при установке Braiins OS+
  + переносит конфигурацию сети по умолчанию (можно настроить) при удалении Braiins OS+
  + параметры доступны для настройки процесса
  + настраивает ограничения мощности по умолчанию (1420W) для автонастройки при установке Braiins OS+
  + пакетный режим доступен для управления несколькими устройствами одновременно
  + простота использования
  
  - не работает на майнере с заблокированным SSH

.. _bosbox_install:

===================================================
Установка Braiins OS+ с помощью BOS+ Toolbox
===================================================

  * Скачайте **BOS+ Toolbox** с нашего `веб-сайта <https://braiins-os.com/>`_.
  * Создайте новый текстовый файл, измените ".txt" окончание на ".csv" и вставьте IP-адреса, на которых вы хотите выполнить команды. Поместите этот файл в каталог, где находится BOS+ Toolbox. Используйте только один IP-адрес в строке!
  * После того, как вы загрузили BOS+ Toolbox, откройте командную строку (например, CMD для Windows, Terminal для Ubuntu и т.д.)
  * Замените *FILE_PATH_TO_BOS+_TOOLBOX* заполнитель в приведенной ниже команде с фактическим путем к файлу, в котором вы сохранили BOS+ Toolbox. Затем переключитесь на путь к файлу, выполнив команду: ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * Теперь замените *listOfMiners.csv* заполнитель с вашим именем файла в команде ниже и выполните соответствующую команду для вашей операционной системы:

    Для командной строки **Windows**: ::

      bos-plus-toolbox.exe install ARGUMENTS HOSTNAME
    
    Для командной строки **Linux**: ::
      
      ./bos-plus-toolbox install ARGUMENTS HOSTNAME

    **Примечание:** *при использовании BOS+ Toolbox для Linux вам нужно сделать его исполняемым с помощью следующей команды (это нужно сделать только один раз):* ::
  
      chmod u+x ./bos-plus-toolbox

Вы можете использовать следующие **аргументы**, чтобы настроить процесс:

**Важная заметка:** 
При установке Braiins OS+ на **одно устройство**, используйте аргумент *HOSTNAME* (IP-адрес).
При установке Braiins OS+ на **несколько устройств**, **НЕ** используйте аргумент HOSTNAME, вместо этого, используйте аргумент *--batch BATCH*.

====================================  ===============================================================================
Аргументы                             Описание
====================================  ===============================================================================
-h, --help                            показать это справочное сообщение и выйти
--batch BATCH                         путь к файлу со списком хостов (IP-адресов) для установки
--backup                              сделать резервную копию майнера перед обновлением
--no-nand-backup                      пропустить полное резервное копирование NAND (конфигурация все еще копируется)
--pool-user [POOL_USER]               установить имя пользователя и воркера для пула по умолчанию
--psu-power-limit [PSU_POWER_LIMIT]   установить предел мощности блока питания (в ваттах)
--no-keep-network                     не сохранять конфигурацию сети майнера (использование DHCP)
--no-keep-pools                       не сохранять конфигурацию пула
--no-keep-hostname                    не сохраняйте имя хоста и генерировать новое на основе MAC
--keep-hostname                       заставить оставлять любое имя хоста
--no-wait                             не ждать, пока система полностью обновится
--dry-run                             сделать все шаги обновления без фактического обновления
--post-upgrade [POST_UPGRADE]         путь к каталогу с stage3.sh скриптом
--install-password INSTALL_PASSWORD   ssh пароль для установки
====================================  ===============================================================================

**Пример:**

::

  bos-toolbox.exe install --batch listOfMiners.csv --psu-power-limit 1200 --install-password admin

Эта команда установит Braiins OS+ на майнеры, указанные в файле *listOfMiners.csv*, и установит ограничение мощности 1200 для всех из них. Команда также автоматически вставит пароль SSH *admin*, когда майнер запросит его.

.. _bosbox_update:

==============================================
Обновление Braiins OS+ с помощью BOS+ Toolbox
==============================================

  * Скачайте **BOS+ Toolbox** с нашего `веб-сайта <https://braiins-os.com/plus/download/>`_.
  * Создайте новый текстовый файл, измените ".txt" окончание на ".csv" и вставьте IP-адреса, на которых вы хотите выполнить команды. Поместите этот файл в каталог, где находится BOS+ Toolbox.
  * После того, как вы загрузили BOS+ Toolbox, откройте командную строку (например, CMD для Windows, Terminal для Ubuntu и т.д.)
  * Замените *FILE_PATH_TO_BOS+_TOOLBOX* заполнитель в приведенной ниже команде с фактическим путем к файлу, в котором вы сохранили BOS+ Toolbox. Затем переключитесь на путь к файлу, выполнив команду: ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * Теперь замените *listOfMiners.csv* заполнитель с вашим именем файла в команде ниже и выполните соответствующую команду для вашей операционной системы:

    Для командной строки **Windows**: ::

      bos-plus-toolbox.exe update ARGUMENTS HOSTNAME

   Для командной строки **Linux**: ::
      
      ./bos-plus-toolbox update ARGUMENTS HOSTNAME

     **Примечание:** *при использовании BOS+ Toolbox для Linux вам нужно сделать его исполняемым с помощью следующей команды (это нужно сделать только один раз):* ::
  
      chmod u+x ./bos-plus-toolbox

Вы можете использовать следующие **аргументы**, чтобы настроить процесс:

**Важная заметка:** 
При установке Braiins OS+ на **одно устройство**, используйте аргумент *HOSTNAME* (IP-адрес).
При установке Braiins OS+ на **несколько устройств**, **НЕ** используйте аргумент HOSTNAME, вместо этого, используйте аргумент *--batch BATCH*.

====================================  ============================================================
Аргументы                             Описание
====================================  ============================================================
--h, --help                           показать это справочное сообщение и выйти
--batch BATCH                         путь к файлу со списком хостов для установки
-p PASSWORD, --password PASSWORD      пароль администратора
-i, --ignore                          не останавливаться на ошибках
====================================  ============================================================


**Пример:**

::

  bos-toolbox.exe update --batch listOfMiners.csv

Эта команда будет искать обновление для майнеров, указанных в *listOfMiners.csv*, и обновлять их, если появится новая версия прошивки.

.. _bosbox_uninstall:

================================================
Деинсталляция Braiins OS+ с помощью BOS+ Toolbox
================================================

  * Скачайте **BOS+ Toolbox** с нашего `веб-сайта <https://braiins-os.com/plus/download/>`_.
  * Создайте новый текстовый файл в своем текстовом редакторе и вставьте IP-адреса, на которых вы хотите выполнить команды. Каждый IP-адрес должен быть разделен запятой. (Обратите внимание, что вы можете найти IP-адрес в веб-интерфейсе Braiins OS+, перейдя в *Status -> Overview*.) Затем сохраните файл в том же каталоге, в котором вы сохранили BOS+ Toolbox, и измените ".txt" окончание на ".csv".
  * После того, как вы загрузили BOS+ Toolbox и сохранили .csv фаил, откройте командную строку (например, CMD для Windows, Terminal для Ubuntu и т.д.)
  * Замените *FILE_PATH_TO_BOS+_TOOLBOX* заполнитель в приведенной ниже команде с фактическим путем к файлу, в котором вы сохранили BOS+ Toolbox. Затем переключитесь на путь к файлу, выполнив команду: ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * Теперь замените *listOfMiners.csv* заполнитель с вашим именем файла в команде ниже и выполните соответствующую команду для вашей операционной системы:

     Для командной строки **Windows**: ::

      bos-plus-toolbox.exe uninstall ARGUMENTS HOSTNAME

    Для командной строки **Linux**: ::
      
      ./bos-plus-toolbox uninstall ARGUMENTS HOSTNAME
      
    **Примечание:** *при использовании BOS+ Toolbox для Linux вам нужно сделать его исполняемым с помощью следующей команды (это нужно сделать только один раз):* ::
  
      chmod u+x ./bos-plus-toolbox

Вы можете использовать следующие **аргументы**, чтобы настроить процесс:

**Важная заметка:** 
При установке Braiins OS+ на **одно устройство**, используйте аргумент *HOSTNAME* (IP-адрес).
При установке Braiins OS+ на **несколько устройств**, **НЕ** используйте аргумент HOSTNAME, вместо этого, используйте аргумент *--batch BATCH*.

====================================  ============================================================
Аргументы                             Описание
====================================  ============================================================
-h, --help                            показать это справочное сообщение и выйти
--batch BATCH                         путь к файлу со списком хостов для установки
--factory-image FACTORY_IMAGE         путь/URL к исходному образу обновления прошивки (дефолт:
                                      Antminer-S9-all-201812051512-autofreq-user-Update2UBI-
                                      NF.tar.gz)
====================================  ============================================================

**Пример:**

::

  bos-toolbox.exe uninstall --batch listOfMiners.csv

Эта команда удалит Braiins OS+ из майнеров, указанных в файле *listOfMiners.csv*, и установит стандартную прошивку по умолчанию. (Antminer-S9-all-201812051512-autofreq-user-Update2UBI-NF.tar.gz).

.. _bosbox_configure:

==============================================
Настройка  Braiins OS+ с помощью BOS+ Toolbox
==============================================

  * Скачайте **BOS+ Toolbox** с нашего `веб-сайта <https://braiins-os.com/plus/download/>`_.
  * Создайте новый текстовый файл в своем текстовом редакторе и вставьте IP-адреса, на которых вы хотите выполнить команды. Каждый IP-адрес должен быть разделен запятой. (Обратите внимание, что вы можете найти IP-адрес в веб-интерфейсе Braiins OS+, перейдя в *Status -> Overview*.) Затем сохраните файл в том же каталоге, в котором вы сохранили BOS+ Toolbox, и измените ".txt" окончание на ".csv".
  * После того, как вы загрузили BOS+ Toolbox и сохранили .csv фаил, откройте командную строку (например, CMD для Windows, Terminal для Ubuntu и т.д.)
  * Замените *FILE_PATH_TO_BOS+_TOOLBOX* заполнитель в приведенной ниже команде с фактическим путем к файлу, в котором вы сохранили BOS+ Toolbox. Затем переключитесь на путь к файлу, выполнив команду: ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  *Теперь замените *listOfMiners.csv* заполнитель с вашим именем файла в команде ниже и выполните соответствующую команду для вашей операционной системы:


    Для командной строки **Windows**: ::

      bos-plus-toolbox.exe config ARGUMENTS ACTION TABLE

     Для командной строки **Linux**: ::
      
      ./bos-plus-toolbox config ARGUMENTS ACTION TABLE
      
    **Примечание:** *при использовании BOS+ Toolbox для Linux вам нужно сделать его исполняемым с помощью следующей команды (это нужно сделать только один раз):* ::
  
      chmod u+x ./bos-plus-toolbox

Вы можете использовать следующие **аргументы**, чтобы настроить процесс:

====================================  ============================================================
Аргументы                             Описание
====================================  ============================================================
-h, --help                            показать это справочное сообщение и выйти
-u USER, --user USER                  Имя пользователя администратора
-p PASSWORD, --password PASSWORD      Пароль администратора или "prompt"
-c, --check                           пробный прогон sans 
-i, --ignore                          не останавливаться на ошибках
====================================  ============================================================

Вам **необходимо использовать одно** из следующих **действий** чтобы отрегулировать процесс:

====================================  ============================================================
Аргументы                             Описание
====================================  ============================================================
load                                  загрузить текущую конфигурацию майнеров (указанную в 
                                      файле CSV) и вставить их в файл CSV
save                                  сохранить настройки из файла CSV для майнеров 
                                      (без применения)
apply                                 применить настройки, которые были скопированы из файла CSV к 
                                      майнерам
save_apply                            сохранить и применить настройки из файла CSV к майнерам
====================================  ============================================================

**Пример:**

::

  bos-toolbox.exe config --user root load listOfMiners.csv
  
  #отредактируйте файл CSV с помощью редактора электронных таблиц (например: Office Excel, LibreOffice Calc, etc.)
  
  bos-toolbox.exe config --user root save_apply listOfMiners.csv

Первая команда загрузит конфигурацию майнеров, указанную в *listOfMiners.csv* (используя логин *root*) и сохранит ее в CSV-файле. Теперь вы можете открыть файл и редактировать то, что вам нужно. После редактирования файла вторая команда скопирует настройки обратно в майнеры и применит их.

.. _bosbox_scan:

===============================================================
Сканирование сети для выявления майнеров с помощью BOS+ Toolbox
===============================================================

  *Скачайте **BOS+ Toolbox** с нашего `веб-сайта <https://braiins-os.com/plus/download/>`_.
  * Создайте новый текстовый файл в своем текстовом редакторе и вставьте IP-адреса, на которых вы хотите выполнить команды. Каждый IP-адрес должен быть разделен запятой. (Обратите внимание, что вы можете найти IP-адрес в веб-интерфейсе Braiins OS+, перейдя в *Status -> Overview*.) Затем сохраните файл в том же каталоге, в котором вы сохранили BOS+ Toolbox, и измените ".txt" окончание на ".csv".
  * После того, как вы загрузили BOS+ Toolbox и сохранили .csv фаил, откройте командную строку (например, CMD для Windows, Terminal для Ubuntu и т.д.)
  * Замените *FILE_PATH_TO_BOS+_TOOLBOX* заполнитель в приведенной ниже команде с фактическим путем к файлу, в котором вы сохранили BOS+ Toolbox. Затем переключитесь на путь к файлу, выполнив команду: ::

      cd FILE_PATH_TO_BOS+_TOOLBOX

  * Теперь замените *listOfMiners.csv* заполнитель с вашим именем файла в команде ниже и выполните соответствующую команду для вашей операционной системы:


    Для командной строки **Windows**: ::

      bos-plus-toolbox.exe discover ARGUMENTS

     Для командной строки **Linux**: ::
      
      ./bos-plus-toolbox discover ARGUMENTS
      
    **Примечание:** *при использовании BOS+ Toolbox для Linux вам нужно сделать его исполняемым с помощью следующей команды (это нужно сделать только один раз):* ::
  
      chmod u+x ./bos-plus-toolbox

Вы можете использовать следующие **аргументы**, чтобы настроить процесс:

====================================  ============================================================
Аргументы                             Описание
====================================  ============================================================
-h, --help                            показать это справочное сообщение и выйти
====================================  ============================================================

Вам **необходимо использовать одно** из следующих **действий** чтобы отрегулировать процесс:

====================================  ============================================================
Аргументы                             Описание
====================================  ============================================================
scan                                  активно сканировать предоставленный диапазон адресов
listen                                прослушивание входящей трансляции с устройств
                                      (при нажатии кнопки отчета IP)
====================================  ============================================================

**Пример:**

::

  bos-toolbox.exe discover scan 10.10.10.0/24

Эта команда будет сканировать сеть, в диапазоне 10.10.10.0 - 10.10.10.255 и выведет список найденных майнеров с их IP-адресами.

.. _web_package:

***********
Веб-пакет
***********

Веб-пакет можно использовать для переключения со стоковой прошивки, выпущенной до 2019 года. Он также должен работать на других прошивках, основанных на стоковой версии. Этот пакет нельзя использовать на стоковой прошивке, выпущенной в 2019 году и позже, из-за проверки подписи, которая была реализована. Проверка подписи предотвращает использование иных, чем оригинальные стоковые прошивки.

==========
Применение
==========

  * Скачайте **Веб-пакет** с нашего `веб-сайта <https://braiins-os.com/>`_.
  * Следуйте разделам ниже

=========================================
Особенности, плюсы и минусы этого метода:
=========================================

  + заменяет стоковую прошивку на Braiins OS+ без использования дополнительных инструментов
  + переносит конфигурацию сети
  + переносит пул URL, имена пользователей и пароли
  + настраивает ограничения мощности по умолчанию (1420W) для автонастройки
  
  - не может использоваться на стоковой прошивке, выпущенной в 2019 году и позже
  - невозможно настроить установку (например, он всегда будет переносить настройки сети)
  - нет пакетного режима (для массовой установки), если вы не создаете свои собственные скрипты

.. _web_package_install:

===========================================
Установите Braiins OS+ с помощью веб-пакета
===========================================

  * Скачайте **Веб-пакет** с нашего `веб-сайта <https://braiins-os.com/>`_.
  * Войдите на свой майнер и перейдите в раздел *System -> Upgrade*.
  * Загрузите загруженный пакет и прошейте образ.

.. _sd:

***************
Образ SD карты
***************

Если вы используете стандартную прошивку, выпущенную в 2019 году и позже, единственный способ установить Braiins OS+ - это вставить SD-карту с прошивкой Braiins OS+. В 2019 году SSH-соединение было заблокировано, и проверка подписи в веб-интерфейсе предотвращает использование других программных прошивок.

==========
Применение
==========

  * Скачайте **Образ SD карты** с нашего `веб-сайта <https://braiins-os.com/>`_.
  * Следуйте разделам ниже

=========================================
Особенности, плюсы и минусы этого метода:
=========================================

  + заменяет SSH заблокированную стоковую прошивку на Braiins OS+
  + использует конфигурацию сети, хранящуюся в NAND (это можно отключить, см. раздел *Настройки сети* ниже)
  + настраивает ограничения мощности по умолчанию (1420W) для автонастройки
  
  - не переносит пул URL, имена пользователей и пароли
  - нет пакетного режима (для массовой установки)

.. _sd_install:

========================================
Установка Braiins OS+ с помощью SD карты
========================================

 * Скачайте Образ SD карты с нашего `веб-сайта `website <https://braiins-os.com/>`_.
 * Перенесите загруженный образ на SD-карту (например, используя `Etcher <https://etcher.io/>`_). *Примечание. Простое копирование на SD-карту не будет работать. SD-карта должна быть перепрошита!*
 * Настройте перемычки для загрузки с SD-карты (вместо памяти NAND), как показано ниже.

  .. |pic1| image:: ../_static/s9-jumpers.png
      :width: 45%
      :alt: S9 Jumpers

  .. |pic2| image:: ../_static/s9-jumpers-board.png
      :width: 45%
      :alt: S9 Jumpers Board

  |pic1|  |pic2|

 * Вставьте SD-карту в устройство, затем запустите устройство.
 * Через некоторое время вы сможете получить доступ к интерфейсу Braiins OS+ через IP-адрес устройства.
 * *[Необязательно]:* Теперь вы можете установить Braiins OS+ на NAND (см. раздел :ref:`sd_nand_install`)

.. _sd_network:

================
Настройки сети
================
 
 По умолчанию используется конфигурация сети, хранящаяся в NAND, при запуске Braiins OS+ с SD-карты. Эта функция может быть отключена, следуя инструкциям ниже:

  * Смонтируйте первый раздел FAT на SD-карте
  * Откройте файл uEnv.txt и вставьте следующий стринг (убедитесь, что в на каждой строке только один стринг)

  ::

    cfg_override=no

Отключение использования старых сетевых настроек полезно для пользователей, у которых есть проблемы с тем, что майнер не виден в сети (например, статический IP-адрес, используемый в NAND, находится вне зоны действия сети). При этом используется DHCP.

.. _sd_nand_install:

===============
NAND установка
===============

SD-карту можно использовать для замены встроенного программного обеспечения NAND на Braiins OS+. Это можно сделать либо:
  * используя веб-интерфейс - раздел *System -> Install current system to device (NAND)*
  * используя *miner* tool через SSH - следуйте этому разделу руководства :ref:`miner_nand_install`

.. _sd_factory_reset:

=============================================
Braiins OS+ сброс настроек с помощью SD-карты
=============================================

Вы можете сделать сброс до заводских настроек, следуя инструкциям ниже:

  * Смонтируйте первый раздел FAT на SD-карте
  * Откройте файл uEnv.txt и вставьте следующий стринг (убедитесь, что в на каждой строке только один стринг)

  ::

    factory_reset=yes

.. _ssh_package:

********************************
Пакет удаленной установки (SSH)
********************************

С помощью *Пакета удаленной установки (SSH)* вы можете установить или удалить Braiins OS+. Этот метод не рекомендуется, так как требует установки Python. Вместо этого используйте BOS+ Toolbox.

===========
Применение
===========

  * Скачайте **Пакет удаленной установки (SSH)** с нашего `веб-сайта <https://braiins-os.com/>`_.
  * Следуйте разделам ниже

=========================================
Особенности, плюсы и минусы этого метода:
=========================================

  + дистанционная установка Braiins OS+
  + дистанционное удаление Braiins OS+
  + переносит всю конфигурацию по умолчанию (можно настроить) при установке Braiins OS+
  + переносит конфигурацию сети по умолчанию (можно настроить) при удалении Braiins OS+
  + параметры доступны для настройки процесса
  + настраивает ограничения мощности по умолчанию (1420W) для автонастройки при установке Braiins OS+
  
  - нет пакетного режима (для массовой установки), если вы не создаете свои собственные скрипты
  - требует долгой установки
  - не работает на майнере с заблокированным SSH

.. _ssh_package_environment:

===========================
Подготовка окружающей среды
===========================

Во-первых, вам нужно подготовить среду Python. Это состоит из следующих шагов:

* *(Только Windows)* Устонавите *Ubuntu for Windows 10* доступный в Microsoft Store `здесь. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Выполните следующие команды в терминале командной строки:

*(Обратите внимание, что команды совместимы с Ubuntu и Ubuntu для Windows 10. Если вы используете другой дистрибутив Linux или другую ОС, пожалуйста, ознакомьтесь с соответствующей документацией и отредактируйте команды при необходимости.)*

::

  #Обновите репозитории и установите зависимости
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv
  
  #Скачайте и распакуйте пакет прошивки
  wget -c http://feeds.braiins-os.com/20.04/braiins-os_am1-s9_ssh_2020-04-30-1-cbf99510-plus.tar.gz -O - | tar -xz
  
  #Измените каталог на распакованную папку с прошивкой
  cd ./braiins-os_am1-s9_ssh_2020-04-30-1-cbf99510-plus
  
  #Создайте виртуальную среду и активируйте ее
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate
  
  #Установите необходимые пакеты Python
  python3 -m pip install -r requirements.txt

.. _ssh_package_install:

==========================================
Установка Braiins OS+ с помощью SSH-пакета
==========================================

Установка Braiins OS+ с использованием так называемого *Метода SSH* состоит из следующих шагов:

* *(Кастомная прошивка)* Перепрошейте на заводскую прошивкую Этот шаг можно пропустить, если устройство работает на заводской прошивке или на предыдущих версиях Braiins OS. *(Примечание: вполне возможно, что Braiins OS+ может быть установлен непосредственно поверх кастомной прошивки, но, поскольку они отличаются от стоковой версии, может потребоваться сначала прошить стоковую прошивку.)*
* *(Только Windows)* Установите *Ubuntu for Windows 10* оступный в Microsoft Store `здесь. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Подготовьте среду Python, которая описана в разделе :ref:`ssh_package_environment`.
* Выполните следующие команды в терминале командной строки (заменить заполнитель ``IP_ADDRESS`` соответственно) :

*(Обратите внимание, что команды совместимы с Ubuntu и Ubuntu для Windows 10. Если вы используете другой дистрибутив Linux или другую ОС, пожалуйста, ознакомьтесь с соответствующей документацией и отредактируйте команды при необходимости.)*

::

    #Измените каталог на распакованную папку с прошивкой (если ее еще нет в папке с прошивкой)
  cd ./braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46
  
  #Активировать виртуальную среду (если она еще не активирована)
  source .env/bin/activate
  
  #Запустите скрипт для установки Braiins OS+
  python3 upgrade2bos.py IP_ADDRESS

**Примечание:** *для получения дополнительной информации об аргументах, которые можно использовать, используйте* **--help** *аргумент.*

.. _ssh_package_uninstall:

==============================================
Lеинсталляция Braiins OS+ с помошью SSH-пакета
==============================================

.. _ssh_package_uninstall_image:

Использование заводского образа прошивки
=========================================

First, you need to prepare the Python environment, which is described in the section :ref:`ssh_package_environment`.

On an Antminer S9, you can flash a factory firmware image
from the manufacturer’s website, with ``FACTORY_IMAGE`` being file path
or URL to the ``tar.gz`` (not extracted!) file. Supported images with
corresponding MD5 hashes are listed in the
`platform.py <https://github.com/braiins/braiins/blob/master/braiins-os/upgrade/am1/platform.py>`__
file.

Run (replace the placeholders ``FACTORY_IMAGE`` and ``IP_ADDRESS`` accordingly):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS

**Note:** *for more information about the arguments that can be used, use the* **--help** *argument.*

.. _ssh_package_uninstall_backup:

Using previously created backup
===============================

First, you need to prepare the Python environment, which is described in the section :ref:`ssh_package_environment`.

If you created a backup of the original firmware during the installation of Braiins OS+, you can restore it by using the following commands (replace the placeholders ``BACKUP_ID_DATE`` and ``IP_ADDRESS`` accordingly):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-1-6b4a0f46 && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS

**Note: This method is not recommended as the backup creation is very finicky. The backup can be corrupted and there is no way to check it. Use at your own risk and make sure, you can access the miner and insert an SD card to it in case the restoration does not finish successfully!**

.. _opkg:

****
OPKG
****

OPKG commands can be used after connecting to the miner via SSH. There are many OPKG commands, but regarding Braiins OS+, you need to use only the following:

  * *opkg update* - updates the package lists. It's recommended to use this command before other OPKG commands.
  * *opkg install PACKAGE_NAME* install the defined package. It's recommended to use *opkg update* to update the package lists before installing packages.
  * *opkg remove PACKAGE_NAME*

Since the firmware change results in a reboot, the following
output is expected:

::

  ...
  Collected errors:
  * opkg_conf_load: Could not lock /var/lock/opkg.lock: Resource temporarily unavailable.
    Saving config files...
    Connection to 10.10.10.1 closed by remote host.
    Connection to 10.10.10.1 closed.

=======================================
Features, PROs and CONs of this method:
=======================================

  + update Braiins OS+ remotely
  + switch to Braiins OS+ from other versions remotely
  + revert to the initial version of Braiins OS remotely
  + migrates the configuration and continue to mine without a need to configure anything (when updating or switching to Braiins OS+)
  
  - no batch-mode (unless you create your own scripts)

.. _opkg_update:

=============================
Update Braiins OS+ using OPKG
=============================

With OPKG you can easily update your current installation of Braiins OS+, by connecting to the miner via SSH and using the following commands:

::

  opkg update
  opkg install firmware

  #you can also connect to the miner and run the commands at the same time
  ssh root@IP_ADDRESS "opkg update && opkg install firmware"

This will migrate the configuration and continue to mine without a need to configure anything.

.. _opkg_switch_to_braiinsplus:

====================================================
Switch to Braiins OS+ from other versions using OPKG
====================================================

With OPKG you can easily switch to Braiins OS+, by connecting to the miner via SSH and using the following commands:

::

  opkg update
  opkg install firmware

  #you can also connect to the miner and run the commands at the same time
  ssh root@IP_ADDRESS "opkg update && opkg install bos_plus"

This will migrate the configuration and continue to mine without a need to configure anything. Default power limit will be set (1420W).

.. _opkg_factory_reset:

====================================
Braiins OS+ factory reset using OPKG
====================================

With OPKG you can easily revert to the initial version of Braiins OS (the version, which was installed for the first time on that device), by connecting to the miner via SSH and using the following commands:

::

  opkg update
  opkg remove firmware

  #you can also connect to the miner and run the commands at the same time
  ssh root@IP_ADDRESS "opkg update && opkg remove firmware"

This will reset the configuration to the state after the first Braiins OS installation.

.. _sysupgrade:

******************
Sysupgrade package
******************

Sysupgrade is used to upgrade the system running on the device. With this method, you can install various versions of Braiins OS or create a backup of the system. Installation of a firmware using *Braiins OS web interface* or using *opkg install firmware* uses this method. It's recommended to use the *Braiins OS web interface* or *opkg install firmware* instead of this method.

=====
Usage
=====

In order to use sysupgrade, you need to connect to the miner via SSH. The syntax is the following:

::

  sysupgrade [parameters] <image file or URL>

The most important parameters are **--help** (to display the help) and **-F** to force the installation. It's not recommended to use this method (besides the way, it is described bellow), unless you really know, what you are doing.

=======================================
Features, PROs and CONs of this method:
=======================================

  + installs various version of Braiins OS, while connected to the miner
  + migrates the configuration
  + parameters are available to customize the process
  
  - no batch-mode (unless you create your own scripts)
  - cannot switch to an older version of Braiins OS (released before 2020)

.. _sysupgrade_switch_braiinsos:

==============================================================================
Switch to Braiins OS (without autotuning) from other versions using Sysupgrade
==============================================================================

In order to upgrade from older version of Braiins OS or downgrade from Braiins OS+, use the following command (replace the placeholder ``IP_ADDRESS`` accordingly):

::

  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-04-30-0-259943b5_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

This command contains the following commands: 

  * **ssh** - to connect to the miner
  * **wget** - used for downloading files, in this case the firmware package
  * **sysupgrade** - to actually flash the downloaded firmware package

.. _sysupgrade_switch_braiinsplus:

==========================================================
Switch to Braiins OS+ from other versions using Sysupgrade
==========================================================

In order to upgrade from older version of Braiins OS, use the following command (replace the placeholder ``IP_ADDRESS`` accordingly):

::

  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar http://feeds.braiins-os.com/am1-s9/firmware_2020-04-30-1-cbf99510-plus_arm_cortex-a9_neon.tar && sysupgrade /tmp/firmware.tar'

This command contains the following commands: 

  * **ssh** - to connect to the miner
  * **wget** - used for downloading files, in this case the firmware package
  * **sysupgrade** - to actually flash the downloaded firmware package

Note: It's recommended to use the *BOS+ Toolbox*, *Braiins OS web interface* or *opkg install bos_plus* instead of this method.

.. _bos2bos:

**************
Bos2Bos script
**************

**Bos2Bos script is not recommended to use, unless you experience problems with the installation using the other methods.** This method works, only if Braiins OS is already running on the device.

=======================================
Features, PROs and CONs of this method:
=======================================

  + installs any version of Braiins OS remotely
  + install a clean version of Braiins OS
  + parameters are available to customize the process
  
  - no batch-mode (unless you create your own scripts)

=====
Usage
=====

Usage of the Bos2Bos script requires the following setup:

* *(Only Windows)* Install *Ubuntu for Windows 10* available from the Microsoft Store `here. <https://www.microsoft.com/en-us/store/p/ubuntu/9nblggh4msv6>`_
* Run the following commands in your command line terminal:

*(Note that the commands are compatible with Ubuntu and Ubuntu for Windows 10. If you are using a different distribution of Linux or a different OS, please check the corresponding documentation and edit the commands as necessary.)*

::
  
  #Update the repositories and install dependencies
  sudo apt update && sudo apt install python3 python3-virtualenv virtualenv
  
  # clone repository
  git clone https://github.com/braiins/braiins-os.git
  
  #change the directory
  cd ./braiins-os/braiins-os/

  #Create a virtual environment and activate it
  virtualenv --python=/usr/bin/python3 .env && source .env/bin/activate
  
  #Install the required Python packages
  python3 -m pip install -r requirements.txt

After you succesfully finish the setup, you can use the following commands:

::

  #activate the virtual environment
  source .env/bin/activate

  #basic usage is the following
  python3 bos2bos.py FIRMWARE_URL IP_ADDRESS

  #the description of all available parameters can be displayed using the following command
  python3 bos2bos.py -h

**********
Miner tool
**********

.. _miner_nand_install:

=======================================
SD to NAND install using the Miner tool
=======================================

The SD card can be used to replace the firmware running on NAND with Braiins OS+. This can be done by connecting to the miner via SSH and usage of the following command:

  ::

    miner nand_install


.. _miner_factory_reset:

==============================================
Braiins OS+ factory reset using the Miner tool
==============================================

Factory reset can also be done using the *Miner tool*. Use the following command to do so:

  ::

    miner nand_install

.. _miner_detect:

========================================
Detect device with LEDs using Miner tool
========================================

You can find a device by turning on LED blinking, using the *Miner tool*. Use the following command to do so:

  ::

    #turn on LED blinking
    miner fault_light on

    #turn off LED blinking
    miner fault_light off
