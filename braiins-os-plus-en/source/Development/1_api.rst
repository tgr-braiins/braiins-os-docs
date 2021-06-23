
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

############
BOSminer API
############

.. contents::
  :local:
  :depth: 2

*****
Usage
*****

Basic subset of the upstream cgminer API as well as several
new commands are implemented.

To test API, you can use `nc` in the following way *(replace the *IP_ADDRESS* placeholder with your miner's IP address)*:

**Single command**

::

  echo '{"command":"pools"}' | nc IP_ADDRESS 4028

In case you want to format JSON string, you can use `jq`:

::

  echo '{"command":"pools"}' | nc IP_ADDRESS 4028 | jq .

**Multiple commands**

::

  echo '{"command":"version+config"}' | nc IP_ADDRESS 4028 | jq .


**Command with parameter**

::

  echo '{"command":"asc","parameter":0}' | nc IP_ADDRESS 4028 | jq .

******************************
List of all supported commands
******************************

=============================
Commands from the CGminer API
=============================

We provide only brief description of the commands below. If interested in additional
details, please read the `official documentation. <https://github.com/ckolivas/cgminer/blob/master/API-README>`_

 * **asccount** - The details of a single ASC number N in the same format and details as for DEVS
 * **asc** *|N* - The number of ASCs
 * **config** - Some miner configuration information
 * **devdetails** - Report per chain configuration
 * **devs** - Each available ASC with their details
 * **edevs** *[|old]* - The same as devs, except it ignores blacklisted devices and zombie devices (unless the "old" parameter is used)
 * **pools** - Report pool configuration and statistics
 * **summary** - The status summary of the miner
 * **stats** - Each device or pool that has 1 or more getworks with a list of stats regarding getwork times
 * **version** - Print API and BOSminer version
 * **estats** - the same as stats, except it ignores blacklisted devices and zombie devices
 * **check** - checks, whether the API command exists and is accessible
 * **coin** - coin mining information
 * **lcd** - short status summary of the miner
 * **switchpool** *|N* - switches the selected pool to the highest priority
 * **enablepool** *|N* - enables the selected pool
 * **disablepool** *|N* - disables the selected pool
 * **addpool** *|URL,USR,PASS* - adds pool
 * **removepool** *|N* - removes pool

**Note**: the commands *switchpool*, *enablepool*, *disablepool*, *addpool* and *removepool* are not fully implemented in Braiins OS. The outcome of these commands is reset after restart and they do not activate the pools. This is a known issue and is being fixed.

============
New commands
============

 * **fans** - Report fans statistics
 * **tempctrl** - Report temperature control configuration
 * **temps** - Report temperature data
 * **tunerstatus** - Report tuning statistics
 * **pause** - Immediately pause mining and stop power consumption, prepare for resume
 * **resume** - Resume mining after miner has been paused

********
Examples
********

**fans**
Report fans statistics

.. raw:: html

   <details>
   <summary><a>Output</a></summary>

.. code-block:: json

   {
     "STATUS": [
       {
         "STATUS": "S",
         "When": 1595938455,
         "Code": 202,
         "Msg": "4 Fan(s)",
         "Description": "BOSminer+ 0.2.0-ea64aec8e"
       }
     ],
  	 "FANS": [
       {
         "FAN": 0,
         "ID": 0,
         "RPM": 5340,
         "Speed": 100
       },
       {
         "FAN": 1,
         "ID": 1,
         "RPM": 4620,
         "Speed": 100
       },
       {
         "FAN": 2,
         "ID": 2,
         "RPM": 0,
         "Speed": 100
       },
       {
         "FAN": 3,
         "ID": 3,
         "RPM": 0,
         "Speed": 100
       }
     ],
     "id": 1
   }

.. raw:: html

   <p></p>
   </details>


**tempctrl**
Report temperature control configuration

.. raw:: html

   <details>
   <summary><a>Output</a></summary>

.. code-block:: json

    {
	  "STATUS": [
	    {
	      "STATUS": "S",
	      "When": 1595938464,
	      "Code": 200,
	      "Msg": "Temperature control",
	      "Description": "BOSminer+ 0.2.0-ea64aec8e"
	    }
	  ],
	  "TEMPCTRL": [
	    {
	      "Dangerous": 110,
	      "Hot": 100,
	      "Mode": "Automatic",
	      "Target": 89
	    }
	  ],
	  "id": 1
	}

.. raw:: html

   <p></p>
   </details>


**temps**
Report temperature data

.. raw:: html

   <details>
   <summary><a>Output</a></summary>

.. code-block:: json

	{
	  "STATUS": [
	    {
	      "STATUS": "S",
	      "When": 1595938484,
	      "Code": 201,
	      "Msg": "3 Temp(s)",
	      "Description": "BOSminer+ 0.2.0-ea64aec8e"
	    }
	  ],
	  "TEMPS": [
	    {
	      "Board": 81.875,
	      "Chip": 104.625,
	      "ID": 6,
	      "TEMP": 0
	    },
	    {
	      "Board": 85.875,
	      "Chip": 108.9375,
	      "ID": 7,
	      "TEMP": 1
	    },
	    {
	      "Board": 84.4375,
	      "Chip": 105.4375,
	      "ID": 8,
	      "TEMP": 2
	    }
	  ],
	  "id": 1
	}

.. raw:: html

   <p></p>
   </details>


**tunerstatus**
Report tuning statistics

.. raw:: html

   <details>
   <summary><a>Output</a></summary>

.. code-block:: json

	{
	  "STATUS": [
	    {
	      "STATUS": "S",
	      "When": 1595938492,
	      "Code": 203,
	      "Msg": "Tuner Status",
	      "Description": "BOSminer+ 0.2.0-ea64aec8e"
	    }
	  ],
	  "TUNERSTATUS": [
	    {
	      "ApproximateChainPowerConsumption": 1344,
	      "ApproximateMinerPowerConsumption": 1419,
	      "DynamicPowerScaling": "Disabled",
	      "PowerLimit": 1420,
	      "TunerChainStatus": [
	        {
	          "ApproximatePowerConsumptionWatt": 448,
	          "HashchainIndex": 6,
	          "Iteration": 0,
	          "LoadedProfileCreatedOn": 1595938289,
	          "PowerLimitWatt": 448,
	          "StageElapsed": 78,
	          "Status": "Tuning individual chips",
	          "TunerRunning": true,
	          "TuningElapsed": 98
	        },
	        {
	          "ApproximatePowerConsumptionWatt": 448,
	          "HashchainIndex": 7,
	          "Iteration": 0,
	          "LoadedProfileCreatedOn": 1595938289,
	          "PowerLimitWatt": 448,
	          "StageElapsed": 78,
	          "Status": "Tuning individual chips",
	          "TunerRunning": true,
	          "TuningElapsed": 98
	        },
	        {
	          "ApproximatePowerConsumptionWatt": 448,
	          "HashchainIndex": 8,
	          "Iteration": 0,
	          "LoadedProfileCreatedOn": 1595938289,
	          "PowerLimitWatt": 448,
	          "StageElapsed": 78,
	          "Status": "Tuning individual chips",
	          "TunerRunning": true,
	          "TuningElapsed": 98
	        }
	      ]
	    }
	  ],
	  "id": 1
	}

.. raw:: html

   <p></p>
   </details>


**devdetails**
Report device details

.. raw:: html

   <details>
   <summary><a>Output</a></summary>

.. code-block:: json

	{
	  "STATUS": [
	    {
	      "STATUS": "S",
	      "When": 1595938989,
	      "Code": 69,
	      "Msg": "Device Details",
	      "Description": "BOSminer+ 0.2.0-ea64aec8e"
	    }
	  ],
	  "DEVDETAILS": [
	    {
	      "Chips": 63,
	      "Cores": 7182,
	      "DEVDETAILS": 0,
	      "Device Path": "",
	      "Driver": "",
	      "Frequency": 799682118,
	      "ID": 6,
	      "Kernel": "",
	      "Model": "Bitmain Antminer S9",
	      "Name": "Hash Chain 6",
	      "Voltage": 8.416799545288086
	    },
	    {
	      "Chips": 63,
	      "Cores": 7182,
	      "DEVDETAILS": 1,
	      "Device Path": "",
	      "Driver": "",
	      "Frequency": 809812285,
	      "ID": 7,
	      "Kernel": "",
	      "Model": "Bitmain Antminer S9",
	      "Name": "Hash Chain 7",
	      "Voltage": 8.36398983001709
	    },
	    {
	      "Chips": 63,
	      "Cores": 7182,
	      "DEVDETAILS": 2,
	      "Device Path": "",
	      "Driver": "",
	      "Frequency": 770406487,
	      "ID": 8,
	      "Kernel": "",
	      "Model": "Bitmain Antminer S9",
	      "Name": "Hash Chain 8",
	      "Voltage": 8.575228691101074
	    }
	  ],
	  "id": 1
	}

.. raw:: html

   </details>

**pause**
Pause

.. raw:: html

   <details>
   <summary><a>Output</a></summary>

.. code-block:: json
	{
	  "STATUS": [
	    {
	      "STATUS": "S",
	      "When": 1610109274,
	      "Code": 204,
	      "Msg": "Pause",
	      "Description": "BOSminer bosminer-plus-am2-s17 0.6.0-fe72abe5"
	    }
	  ],
	  "PAUSE": [
	    true
	  ],
	  "id": 1
	}
.. raw:: html

   </details>

.. raw:: html

   <details>
   <summary><a>Bosminer Log Output</a></summary>

.. code-block:: 

	Jan 08 12:35:48.197 INFO Interrupted by PAUSE command
	Jan 08 12:35:48.198 INFO CHAIN/1: setting frequency 299.0 MHz on All (error 0.213 MHz)
	Jan 08 12:35:48.199 INFO CHAIN/2: setting frequency 299.0 MHz on All (error 0.213 MHz)
	Jan 08 12:35:48.461 INFO CHAIN/1: setting frequency 274.0 MHz on All (error 0.100 MHz)
	Jan 08 12:35:48.461 INFO CHAIN/2: setting frequency 274.0 MHz on All (error 0.100 MHz)
	Jan 08 12:35:48.728 INFO CHAIN/1: setting frequency 249.0 MHz on All (error 0.005 MHz)
	Jan 08 12:35:48.729 INFO CHAIN/2: setting frequency 249.0 MHz on All (error 0.005 MHz)
	Jan 08 12:35:48.991 INFO CHAIN/1: setting frequency 224.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:48.991 INFO CHAIN/2: setting frequency 224.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:49.255 INFO CHAIN/1: setting frequency 199.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:49.256 INFO CHAIN/2: setting frequency 199.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:49.518 INFO CHAIN/1: setting frequency 174.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:49.519 INFO CHAIN/2: setting frequency 174.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:49.785 INFO CHAIN/1: setting frequency 149.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:49.786 INFO CHAIN/2: setting frequency 149.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:50.048 INFO CHAIN/1: setting frequency 124.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:50.051 INFO CHAIN/2: setting frequency 124.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:50.313 INFO CHAIN/1: setting frequency 99.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:50.319 INFO CHAIN/2: setting frequency 99.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:50.576 INFO CHAIN/1: setting frequency 74.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:50.582 INFO CHAIN/2: setting frequency 74.0 MHz on All (error 0.037 MHz)
	Jan 08 12:35:50.847 INFO CHAIN/1: setting frequency 50.0 MHz on All (error 0.000 MHz)
	Jan 08 12:35:50.848 INFO CHAIN/2: setting frequency 50.0 MHz on All (error 0.000 MHz)
	Jan 08 12:35:54.545 INFO CHAIN/1: Terminating work thread
	Jan 08 12:35:54.546 INFO CHAIN/2: Terminating work thread
	Jan 08 12:35:54.546 INFO PWR/1: Disable voltage
	Jan 08 12:35:54.546 INFO PWR/2: Disable voltage
	Jan 08 12:35:55.819 INFO PSU: Setting voltage 19.25 V
	Jan 08 12:35:55.987 INFO Kicking fans up
	Jan 08 12:35:55.989 INFO Monitor: Fans off: nothing is running | Off Off | 1.4K 1.4K 1.5K 1.5K fan_0%
	Jan 08 12:36:01.086 INFO Tune/all: Status: paused
	Jan 08 12:36:01.088 INFO Hashboard 1: bm13xx 1.0.2 for Antminer S9 or higher built on 2020-12-04 14:49:18 UTC
	Jan 08 12:36:01.092 INFO Hashboard 2: bm13xx 1.0.2 for Antminer S9 or higher built on 2020-12-04 14:49:18 UTC
	Jan 08 12:36:01.094 INFO Waiting for RESUME command...
	Jan 08 12:36:04.313 INFO PWR/1: Voltage controller reset
	Jan 08 12:36:04.314 INFO PWR/2: Voltage controller reset
	Jan 08 12:36:07.388 INFO PWR/1: Voltage controller application started
	Jan 08 12:36:07.404 INFO PWR/2: Voltage controller application started
	Jan 08 12:36:08.454 INFO PWR/1: Voltage controller firmware version 0x88
	Jan 08 12:36:08.454 INFO CHAIN/1: Initializing (fingerprint: 21834ef58d647c6d, difficulty: 4)
	Jan 08 12:36:08.454 INFO CHAIN/1: Resetting hash board
	Jan 08 12:36:08.506 INFO PWR/2: Voltage controller firmware version 0x88
	Jan 08 12:36:08.506 INFO CHAIN/2: Initializing (fingerprint: 509203d3b6736772, difficulty: 4)
	Jan 08 12:36:08.506 INFO CHAIN/2: Resetting hash board
	Jan 08 12:36:13.490 INFO CHAIN/1: Waiting for trigger...
	Jan 08 12:36:13.490 INFO CHAIN/2: Waiting for trigger...

.. raw:: html

   </details>

**resume**
Pause

.. raw:: html

   <details>
   <summary><a>Output</a></summary>

.. code-block:: json
	{
	  "STATUS": [
	    {
	      "STATUS": "S",
	      "When": 1610109287,
	      "Code": 205,
	      "Msg": "Resume",
	      "Description": "BOSminer bosminer-plus-am2-s17 0.6.0-fe72abe5"
	    }
	  ],
	  "RESUME": [
	    true
	  ],
	  "id": 1
	}
.. raw:: html

   </details>

.. raw:: html

   <details>
   <summary><a>Bosminer Log Output</a></summary>

.. code-block:: 

	Jan 08 12:38:17.671 INFO RESUME command received
	Jan 08 12:38:18.840 INFO PWR/1: Enable voltage
	Jan 08 12:38:18.857 INFO PWR/2: Enable voltage
	Jan 08 12:38:21.023 INFO Monitor: Fans full speed: unknown temperature | Starting Starting | 1.3K 1.4K 1.5K 1.5K fan_100%
	Jan 08 12:38:21.590 INFO CHAIN/1: Setting IP core baud rate @ requested: 115740, actual: 115740, divisor 0x6b
	Jan 08 12:38:21.591 INFO CHAIN/1: Starting chip enumeration
	Jan 08 12:38:21.630 INFO CHAIN/2: Setting IP core baud rate @ requested: 115740, actual: 115740, divisor 0x6b
	Jan 08 12:38:21.630 INFO CHAIN/2: Starting chip enumeration
	Jan 08 12:38:22.551 INFO CHAIN/1: Discovered 44 chips
	Jan 08 12:38:22.551 INFO CHAIN/1: setting frequency 50.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:22.595 INFO CHAIN/2: Discovered 44 chips
	Jan 08 12:38:22.599 INFO CHAIN/2: setting frequency 50.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:22.998 INFO CHAIN/1: Setting IP core baud rate @ requested: 6250000, actual: 6250000, divisor 0x01
	Jan 08 12:38:22.999 INFO CHAIN/1: Monitor watchdog temperature task started
	Jan 08 12:38:23.050 INFO CHAIN/2: Setting IP core baud rate @ requested: 6250000, actual: 6250000, divisor 0x01
	Jan 08 12:38:23.050 INFO Tune/all: Status: resuming
	Jan 08 12:38:23.051 INFO CHAIN/1: setting frequency 75.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:23.051 INFO CHAIN/2: setting frequency 75.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:23.051 INFO CHAIN/2: Monitor watchdog temperature task started
	Jan 08 12:38:23.326 INFO CHAIN/2: setting frequency 100.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:23.329 INFO CHAIN/1: setting frequency 100.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:23.599 INFO CHAIN/2: setting frequency 125.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:23.602 INFO CHAIN/1: setting frequency 125.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:23.862 INFO CHAIN/2: setting frequency 150.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:23.865 INFO CHAIN/1: setting frequency 150.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:24.130 INFO CHAIN/2: setting frequency 175.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:24.134 INFO CHAIN/1: setting frequency 175.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:24.397 INFO CHAIN/1: setting frequency 200.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:24.397 INFO CHAIN/2: setting frequency 200.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:27.396 INFO CHAIN/1: setting frequency 225.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:27.396 INFO CHAIN/2: setting frequency 225.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:27.396 INFO CHAIN/1: setting frequency 250.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:27.397 INFO CHAIN/2: setting frequency 250.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:27.397 INFO CHAIN/1: setting frequency 275.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:27.397 INFO CHAIN/2: setting frequency 275.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:27.397 INFO CHAIN/1: setting frequency 300.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:27.398 INFO CHAIN/2: setting frequency 300.0 MHz on All (error 0.000 MHz)
	Jan 08 12:38:27.398 INFO CHAIN/1: setting frequency 324.0 MHz on All (error 0.352 MHz)
	Jan 08 12:38:27.398 INFO CHAIN/2: setting frequency 324.0 MHz on All (error 0.352 MHz)
	Jan 08 12:38:27.398 INFO Kicking fans up

.. raw:: html

   </details>

