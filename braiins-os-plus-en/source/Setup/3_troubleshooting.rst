
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

###############
Troubleshooting
###############

*****************************************************
Recovering Bricked (unbootable) Devices Using SD Card
*****************************************************

If anything goes wrong and your device seems unbootable, you can use the
previously created SD card image to recover the original firmware from the
manufacturer by using the following commands:

.. code:: bash

   cd braiins-os_am1-s9_ssh_VERSION
   source .env/bin/activate
   python3 restore2factory.py backup/2ce9c4aab53c-2018-09-19/ your-miner-hostname-or-ip

After the script finishes, wait a few minutes and adjust the jumper to
boot from NAND (internal memory).

******************
BOSminer operation
******************

BOSminer can be controlled using the command line or via the web page.

In order to start or stop BOSminer, use the following commands:

::

	/etc/init.d/bosminer stop
	/etc/init.d/bosminer start

Alternatively, BOSminer can be controlled in `System -> Startup` page and it is restarted every
time the user clicks on the `Save & Apply` button in the `Miner -> Configuration` page.
