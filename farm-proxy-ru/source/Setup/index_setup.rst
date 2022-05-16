
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

#####
Setup
#####

.. toctree::
   :maxdepth: 3
   :glob:

   *

*********
Установка
*********

Braiins Farm Proxy имеет собственный общедоступный `репозиторий Github <https://github.com/braiins/farm-proxy>`_ и представляет собой стек из 4 контейнеров Docker. Вы можете легко установить их с помощью терминала Linux оборудования, на котором будет размещаться прокси.

В начале требуется установить пару пререквизитов:

**Linux**

 * Следуйте `инструкциям по установке докера <https://docs.docker.com/engine/install/ubuntu/>`_
 * Выполните `дополнительные шаги по установке <https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user>`_
 * Выполните `установку docker-compose <https://docs.docker.com/compose/install/>`_

**RPi**

  * Следуйте одному из многих руководств - например https://jfrog.com/connect/post/install-docker-compose-on-raspberry-pi/

**Проверка установленных компонентов**

::

      docker version
      docker-compose --version
      git --version

**Скачать репозиторий Braiins Farm Proxy**

::

      sudo apt update
      sudo apt install git
      git clone https://github.com/braiins/farm-proxy.git

Стек Docker состоит из следующих контейнеров:
 * *farm-proxy*: контейнер с бинарным файлом Braiins Farm Proxy,
 * *grafana*: контейнер с приложением Grafana,
 * *nodeexporter*: контейнер с экспортером данных об оборудовании и ОС для БД Prometheus,
 * *prometheus*: контейнер с базой данных Prometheus.

Отдельный бинарный файл Braiins Farm Proxy можно загрузить с общедоступного Github `здесь <https://github.com/braiins/farm-proxy/releases>`_.

******
Запуск
******

Когда операция майнинга с использованием Braiins Farm Proxy настроена оператором фермы, прокси может быть запущен (сама настройка подробно описана в следующем тексте). Запустите команду в терминале Linux ``docker-compose up -d``. Чтобы увидеть, все ли контейнеры Docker запущены, нажмите команду ``docker ps`` чтобы перечислить и проверить статус контейнеров.

**********
Перезапуск
**********

Если Braiins Farm Proxy требуется перезагрузка, выполните команду ``docker restart farm-proxy``. Команда перезапустит только контейнер Farm Proxy. Перезапуск Farm Proxy необходим в случае каких-либо изменений в конфигурационном файле TOML. Если вы хотите перезапустить другой контейнер Docker, это можно сделать аналогичным образом, заменив «farm-proxy» на имя контейнера, который необходимо перезапустить.

*********
Остановка
*********

Иногда оператор майнинга может захотеть остановить Braiins Farm Proxy. Это можно сделать в терминале Linux с помощью команды ``docker stop farm-proxy``. Это нужно в случае каких-либо изменений в файле *docker-compose.yml*. Чтобы снова запустить прокси, выполните команду ``docker-compose up -d farm-proxy``. Если вы хотите остановить другой контейнер Docker, это можно сделать аналогичным образом..

**********
Обновление
**********

Рекомендуется следить за репозиторием Braiins Farm Proxy Github, чтобы получать уведомления, в случае доступности новой версии. Чтобы перейти на более новую версию, просто запустите команду ``git pull origin master`` в терминале Linux. Если вы внесли какие-либо изменения в файлы конфигурации, вы можете сохранить измененные файлы или стешнуть их с помощью команды ``git stash`` перед обновлением.

********
Удаление
********

Чтобы полностью удалить ферму-прокси и встроенный мониторинг из вашей системы, вам необходимо выполнить следующие шаги:

1. Остановить прокси фермы и готовый мониторинг: ``docker-compose down``,
2. Удалить контейнеры: ``docker container prune``,
3. Удалить образы: ``docker image prune -a``,
4. Удалить сохраненные данные мониторинга: ``docker volume prune``.
