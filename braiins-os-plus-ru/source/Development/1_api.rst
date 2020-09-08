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

