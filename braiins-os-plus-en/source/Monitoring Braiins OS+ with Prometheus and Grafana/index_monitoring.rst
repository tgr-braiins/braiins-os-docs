
.. raw:: html

    <script>
      window.fwSettings={
      'widget_id':77000003511
      };
      !function(){if("function"!=typeof window.FreshworksWidget){var n=function(){n.q.push(arguments)};n.q=[],window.FreshworksWidget=n}}()
    </script>
    <script type='text/javascript' src='https://euc-widget.freshworks.com/widgets/77000003511.js' async defer></script>

.. _monitoring:

==================================================
Monitoring Braiins OS+ with Prometheus and Grafana
==================================================

.. contents::
  :local:
  :depth: 2

Introduction
============

Starting with Braiins OS+ 22.02.2, each miner is producing statistics in form that can be easily digested by Prometheus. Prometheus data can be then visualized in Grafana.

Prometheus is an open-source systems monitoring and alerting toolkit. Prometheus collects and stores its metrics as time series data, i.e. metrics information is stored with the timestamp at which it was recorded, alongside optional key-value pairs called labels.

Grafana open source is open source visualization and analytics software. It allows you to query, visualize, alert on, and explore your metrics. It provides you with tools to turn your time-series database data into insightful graphs and visualizations.

Prometheus + Grafana is almost an industry standard for monitoring, visualization, and alerting. For Braiins OS+, endpoint for Prometheus metrics is available at ``[IP ADDRESS]:8081/metrics``

Limitations
-----------

-  Only limited set of metrics is available for S9 devices
-  Monitoring only works for miners with Braiins OS+ installed

Setup
=====

Quick Start
-----------

- ``git clone https://github.com/braiins/bos-farm-monitor.git``
- Change list of IP address ranges to scan in ``/supercronic/scan_crontab``
- ``docker-compose up -d``
- Go to localhost:3000

Prerequisites
-------------

**Linux**

-  Follow `docker installation instructions <https://docs.docker.com/engine/install/ubuntu/>`__
-  Follow `Optional docker post installation steps <https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user>`__
-  Follow `docker-compose installation <https://docs.docker.com/compose/install/>`__

**RPi**

-  Follow one of the many manuals - e.g. https://jfrog.com/connect/post/install-docker-compose-on-raspberry-pi/

Installation
------------

**Verify prerequisite installed**

.. code-block::

    docker version
    docker-compose --version
    git --version

**Download Braiins Repository**

You can clone the repository using git:

.. code-block::

   sudo apt update
   sudo apt install git
   git clone https://github.com/braiins/bos-farm-monitor.git

You can download zip file with all the files `https://github.com/braiins/bos-farm-monitor/archive/refs/heads/master.zip <https://github.com/braiins/bos-farm-monitor/archive/refs/heads/master.zip>`__

Prometheus Configuration
------------------------

Before you can start monitoring your farm, you will need to prepare
configuration based on examples in the config directory. There are two
files:

-  ``/config/prometheus_scan.yml``
-  ``/config/prometheus_static.yml``

The only major difference between the two is that “scan” is best to be
used in case your miners have IP addresses assigned by DHCP, while
“static” can be used when your miners have static IP addresses.

**Default Configuration**

The default configuration in has the following features:

-  Job for scraping Braiins OS+ metrics named braiinsos-data
-  Relabeling of metrics endpoint addresses (removal of 8081 port)
-  Parsing of IP addresses:

   -  Second octet: label ``site_id``
   -  Third octet: label ``subnet_id``
   -  Fourth octet: label ``host_id``

-  Removal of some more data intense metrics (you can add them back, just make sure you instance is sized appropriately)
-  Temporary re-write of client_id into individual labels (will be fixed in future versions of Braiins OS+)
-  Static label building for prometheus_static.yml (label is assigned dynamically when prometheus_scan.yml is used (more on this later).

**Structure Your Farm for Good Observability**

For a bigger farm, you might want to group miners into some logical
groupings so that you can see performance by individual components. The
grouping might differ depending on the size and structure of your farm,
some of the most typical elements in the farm topology are:

-  Building
-  Section
-  Tank
-  Aisle
-  Row

To achieve this you have the following options:

 **Use subnets and parse octets of IP addresses**
   If you have static IP addresses and you are using these to organize your
   miners, the easiest way to prepare data for reporting is to enhance
   prometheus configuration with relabels derived from IP addresses. The
   example below shows how to do it. You can obviously use different names
   than section, tank, miner.

   .. code-block::

      relabel_configs:
      # Extract the second octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.(\\d+)\\.\\d+\\.\\d+.*"
        target_label: "section"
      # Extract the third octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.\\d+\\.(\\d+)\\.\\d+.*"
        target_label: "tank"
      # Extract the last octet of IPv4 address
      - source_labels: ["__address__"]
        regex: "\\d+\\.\\d+\\.\\d+\\.(\\d+).*"
        target_label: "miner"
 
 **Use separate jobs together with optional custom label**
   One configuration of Prometheus (stored in prometheus.yml) can contain multiple jobs. For example, you can create separate jobs for each building or container. Each metric has a job label, making it a very convenient approach to group instances (miners). In case when you have other (non-mining) jobs in your configuration, you might want to add a custom label to each job so that you can use that label for filtering/grouping. An example that could be used in relabel_configs section to add building label to each instance that is monitored by the job with value “Bulding A”:

   .. code-block::

      - target_label: "building"
        replacement: "Building A"
 **Use multiple prometheus instances**
   In the case of thousands or more miners it might be easier to setup a separate Prometheus instance for each group of miners. Refer to Prometheus documentation on how to setup `federation <https://prometheus.io/docs/prometheus/latest/federation/>`__.

 **Use username/workername and re-labels (not recommended)**
   Using username/workername for encoding information about physical location of miners is a typically used approach with legacy monitoring applications. This approach does not work well with how Prometheus manages and stores time-series, which is nothing like a traditional relational database. We do not recommend using username/workername for structuring you farm with prometheus for the following reasons:

   -  majority of metrics do not have worker name as labels and joins would need to be created in queries (slows things down, prone to errors)
   -  there can be multiple usernames / workernames associated with a single miner; this makes the joins even more difficult (necessary pre-aggregation with logic which value to choose)

 **Use multiple IP ranges with scan approach**
   If you have miners with IP assigned by DHCP and you are using scanning of your network to get miners to Prometheus, you can define multiple network ranges and each range can have a unique value defined and assigned to label (more on that in the following section).

**Adding miners to configuration**

There are the following basic options how to add your miners to the
configuration:

-  Use service discovery options provided by Prometheus
-  List IP addresses in the configuration file manually

Listing IP addresses directly works best when IP addresses assigned to
miners are static. In the case of DHCP, service discovery is a better
option.

**Service Discovery**

File-based service discovery is the option enabled by default. To start
using it, you will need to configure file ``/supercronic/scan_crontab`` in a
text editor. Current examples are:

.. code-block::

    \*/3 \* \* \* \* \* ssh_scan.sh "10.1.1.0-255" "Building A"
    \*/3 \* \* \* \* \* ssh_scan.sh "10.2.0-255.2" "Building B"

Each line will scan the defined IP range for responding miners and will store the list so that it is available to prometheus. The string “Building A” / “Building B” can be an arbitrary name. Currently, it will get dynamically mapped to label building. The scan is performed every three minutes - you can change it based on the size of your farm and your needs.

**List IP addresses**

In order to use a static list of IP addresses, you need to change the file ``docker-compose.yml``,

First, comment-out the crontab so that dynamic scan is disabled:

.. code-block::

   # bos_scanner:
   # image: braiins/bos_scanner:latest
   # container_name: bos_scanner
   # volumes:
   # - ./supercronic/scan_crontab:/usr/local/share/scan_crontab
   # - scanner_data:/mnt:rw
   # build:
   # context: supercronic
   # dockerfile: Dockerfile
   # network_mode: "host"
   #

Second, comment-out the dynamic scanning and enable use of a different
configuration file. It should look like this after changes:

.. code-block::

   #- '--config.file=/etc/prometheus/prometheus_scan.yml'
   - '--config.file=/etc/prometheus/prometheus_static.yml'

IP addresses are listed as an array in the configuration file
`prometheus_static.yml`. Change the entries with list of your miners:

.. code-block:

   - targets: ['10.35.31.2:8081','10.35.32.2:8081']

Note that:

-  Port has to be added at the end of the IP address. Port 8081 is where the metrics for Prometheus are available
-  IP addresses are quoted and separated by comma

In case you do not have static IP addresses, the IP address of any miner can change. If you still want to use this static approach, try to increase the lease time to high value (e.g. 48 hours) for your DHCP server, so that IP address is re-assigned even when the miner is offline for some time.

In order to get all the miners to the list you can scan your farm for devices using BOS Toolbox and generate configuration from results. You can use either UX or command-line to get the list.

Command-line example (linux):

.. code-block::

   ./bos-toolbox scan -o ips.txt 10.10.0.0/16
   cat ips.txt \| sed "s/.*/'&:8081'/" \| paste -sd',' \| sed "s/.*/[&]/"

The first command will scan all IP addresses in the range 10.10.0.0 and 10.10.255.255. The second will print an array with IP addresses that you can paste in the configuration.

Only miners with Braiins OS+ can be monitored. In case you are using miners without Braiins OS+, it is better to use:

.. code-block::
   
   ./bos-toolbox scan 10.10.0.0/16 &> ips.txt
   grep "\| bOS" ips.txt \| cut -d"(" -f2 \| cut -"d)" -f1 \| sed "s/.*/'&:8081'/" \| paste -sd',' \| sed "s/.*/[&]/"

For different IP ranges you can use:

-  10.10.10.0/24 for range 10.10.10.0 - 10.10.10.255
-  10.10.0.0/16 for range 10.10.0.0 to 10.10.255.255
-  10.0.0.0/8 for range 10.0.0.0 to 10.255.255.25

Start monitoring
----------------

.. code-block::

   docker-compose up -d

You can verify that container is running using `docker ps`.

Now you can go to: http://<your_host>:3000.

Operations
----------

**Changing configuration**

Change configuration file according to your needs

.. code-block::

   docker-compose restart prometheus

**Updating to newer version**

.. code-block::

   git pull origin master
   docker-compose up -d

Dashboards
==========

In our repository we provide sample dashboards that can get you started to prepare monitoring for your farm the best suits your needs.

Farm Dashboard
--------------

This is the high-level dashboard that monitors all of the miners in your farm. It has a built-in data source selector in case you have multiple prometheus instances running. It also features several drill-down reports highlighted in the screenshot below:

  .. |pic3| image:: ../_static/monitoring_dashboard.png
      :width: 100%
      :alt: Dashboard

  |pic3|

Parts highlighted in red will lead you to a drill-down report listing the instances. Parts highlighted in blue will go directly to the miner UX.

Example Farm Dashboard - By Building
------------------------------------

Dashboard has a feature where rows of grafana panels are automatically displayed for each defined building. This is created dynamically based on the values of the building label. The full flow is as follows in the example configuration:

-  two separate jobs are created in prometheus.yml
-  each job has label building added with value representing the building
-  grafana dashboard has parameter building defined which is linked to building label
-  row header has $building as a name - this will get expanded with label values
-  each panel has $building as a filter

Metrics and Labels
==================

Overview:

-  ``application_version_details (instance, version_full, toolchain)``
-  ``hashboard_nominal_hashrate_gigahashes_per_second (instance,hashboard)``
-  ``hashboard_shares (instance, hashboard)``
-  ``miner_metadata (instance, model, os_version)``
-  ``miner_power (instance, type: *wall \| estimate \| limit, socket*)``
-  ``temperature (instance, chip_addr, chip_in_domain, voltage_domain,hashboard, location: *chip \| pcb*)``
-  ``stratum_accepted_shares_counter (instance, client_id, host, user,worker, protocol, connection type)``
-  ``stratum_rejected_shares_counter (instance, client_id, host, user,worker, protocol, connection type)``
-  ``tuner_stage (instance)``

Application Version Details
---------------------------

Version of the application

``application_version_details``

**Labels**

-  ``version_full``
-  ``toolchain``

Hashboard Nominal Hashrate (Gh/s)
---------------------------------

Nominal hashrate for each hashboard in Gh/s.

``hashboard_nominal_hashrate_gigahashes_per_second``

**Labels**

-  instance
-  hashboard

**Examples**

Hashboard Shares
----------------

Number of valid shares produced by hashboards. Hashboard shares can be used to calculate real hashrate for hashboard, miner, or other group. This metric does not provide information whether shares were accepted by target - stratum_accepted_shares_counter should be used for this.

``hashboard_shares (counter)``

**Labels**

-  instance
-  hashboard

**Examples**

Average number of hashes per second over last 20 seconds for all instances:

.. code-block::

   sum(rate(hashboard_shares[20s])) \* 2^32

Average number of hashes per second over last 20 seconds by instance:

.. code-block::

   sum by(instance) (rate(hashboard_shares[20s])) \* 2^32

Average number of hashes per second over last 20 seconds for all instances by miner type:

.. code-block::

   sum by (model) (
      (sum by (instance)((rate(hashboard_shares[20s])))*2^32)
      * on(instance) group_left(model) count by (instance, model) (miner_metadata)
   )

Miner Metadata
--------------

``miner_metadata``

**Labels**

- ``instance``
- ``model`` - model of the miner
-  ``os_version``

**Examples**

Number of miners by model:

.. code-block::

   count_values by (model) ("x", miner_metadata)

Miner Power
-----------

``miner_power``

**Labels**

-  ``instance``
-  ``type: estimated, limit, psu, wall``
-  ``socket``

**Examples**

Total estimated power consumption for all instances:

.. code-block::

   sum(miner_power{type="estimated"})

Total power limit for all instances:

.. code-block::

  sum(miner_power{type="limit"})

Stratum accepted shares counter
-------------------------------

Total number of shares accepted by target. For one instance, there are
typically more targets, represented by client_id label.

``stratum_accepted_shares_counter (counter)``

**Labels**

-  ``instance``
-  ``client_id`` - full identification of target accepting shares

**Examples**

Average number of accepted shares per second over last 20 seconds for
all instances by target:

.. code-block::

   sum by(client_id) (stratum_accepted_shares_counter[20s])) \* 2^32

Stratum rejected shares counter
-------------------------------

Total number of shares rejected by target.

`stratum_rejected_shares_counter (counter)`

**Labels**

-  ``client_id`` - full identification of target rejecting shares
-  ``instance``

There are the following labels added when data is imported to Prometheus:

- ``connection_type``
- ``protocol``
- ``host``
- ``workername``
- ``user``

**Examples**

Average number of rejected shares per second over last 20 seconds for all instances by target:

.. code-block::

   sum by(client_id) (stratum_accepted_shares_counter[20s]))

Temperature
-----------

Every available temperature sensor will provide the data. There might be sensor at different locations (pcb or chip).

`temperature`

**Labels**

-  ``instance``
-  ``chip_addr``
-  ``chip_in_domain``
-  ``voltage_domain``
-  ``hashboard``
-  ``location: chip|pcb``

**Examples**

Average maximum temperature across all instances (miners):

.. code-block::

   avg(max by (instance) (temperature))

Average maximum temperature across all instances (miners) by miner type:

.. code-block::

   avg by (model) (
     (max by (instance) (temperature)) * on (instance)
     group_left(model) count by (instance, model) (miner_metadata)
   )

Tuner stage
-----------

Stage of the tuner:

-  2: testing performance profile
-  3: tuning individual chips
-  4: stable
-  6: manual configuration running

``tuner_stage``

**Labels**

-  ``instance``
-  ``miner``

**Examples**

Number of instances by stage:

.. code-block::

   count_values ("Stage", max by (instance) (tuner_stage))

Other Examples
--------------

**Extracting parts of IP address**

If you are managing your farm by assigning different IP ranges to different parts of your farm, grouping metrics by octet of IP address might be useful. Example for maximum chip temperature by 3rd octet:

.. code-block::

   max by (segment) (label_replace(
     temperature{location="chip"}, "segment", "$1", "instance","\\d+\\.\\d+\\.(\\d+)\\.\\d+.*"
   ))

If you need to do this for many/all metrics, it is better to have parts of the IP address as custom labels. See the Configuration section with an example.

Getting Help
============

For more information about Prometheus and Grafana, please refer to the official documentation:

-  `Prometheus Documentation <https://prometheus.io/docs/introduction/overview/>`__
-  `Grafana Documentation <https://grafana.com/docs/>`__

In case you have questions that are specific to monitoring of Braiins OS+ miners with Prometheus and Grafana, please contact our support team on Telegram.
