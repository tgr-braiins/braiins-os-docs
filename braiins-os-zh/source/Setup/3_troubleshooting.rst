
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

########
故障排除
########

********************************
使用SD卡恢复死机（无法启动的）矿机
********************************

如果您的矿机出现任何问题，且无法开机。您可以通过之前创建的SD卡映像恢复矿机的原厂固件。请您使用以下命令：

.. code:: bash

   cd braiins-os_am1-s9_ssh_VERSION
   source .env/bin/activate
   python3 restore2factory.py backup/2ce9c4aab53c-2018-09-19/ your-miner-hostname-or-ip

脚本完成后，请等待几分钟再调整跳线从NAND（内存）开机。

*********************
BOSminer 挖矿软件的操作
*********************

BOSminer可以通过命令行或在网页上进行控制。

要想开启或关闭BOSminer，请使用以下的命令：

::

	/etc/init.d/bosmniner stop
	/etc/init.d/bosmniner start

此外，BOSminer也可以在System（系统）-> Startup（启动）页面中控制，用户每次点击Miner（矿机） -> Configuration（配置）页内的Save & Apply（保存并应用）按钮，BOSminer都会重新启动。
