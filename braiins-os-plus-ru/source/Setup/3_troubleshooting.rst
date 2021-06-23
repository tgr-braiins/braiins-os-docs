
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

####################
Устранение неполадок
####################

********************************************************************
Восстановление Bricked (не загружаемых) устройств с помощью SD-карты
********************************************************************

Если что-то идет не так и ваше устройство кажется не запустимым, вы можете использовать ранее созданный образ SD-карты, чтобы восстановить оригинальную прошивку от производителя, используя следующие команды:

.. code:: bash

   cd braiins-os_am1-s9_ssh_VERSION
   source .env/bin/activate
   python3 restore2factory.py backup/2ce9c4aab53c-2018-09-19/ your-miner-hostname-or-ip

После завершения сценария подождите несколько минут и настройте перемычку для загрузки из NAND (внутренней памяти).

***************
Работа BOSminer
***************

BOSminer можно управлять с помощью командной строки или через веб-страницу.

Чтобы запустить или остановить BOSminer, используйте следующие команды:

::
	
	/etc/init.d/bosmniner stop
	/etc/init.d/bosmniner start

Альтернативно, BOSminer можно настроить на `System -> Startup` странице и он перезапускается каждый раз, когда пользователь нажимает на `Save & Apply` кнопку на `Miner -> Configuration` странице.

