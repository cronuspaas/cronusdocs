Quick Setup
==============================

Steps to install Cronus agent and Cronus master on a machine.

Prerequisites
--------------

* linux (Ubuntu, Cent, Redhat, Fedora etc.)
* sudo permission
* python > 2.6.7 && < 3
* openssl > 0.9.8
* wget
* system management daemon systemd or upstart
* unzip (for CronusMaster only)
* java runtime >= 1.5 (for CronusMaster only)

Install Agent and Master
--------------------------

   .. code-block:: bash

      cd ~; wget -qO- 'http://cronuspaas.github.io/downloads/install_cronusmaster' | bash

   **Expected outcome:**

   * CronusAgent is running at https://host:19000/agent
   * CronusMaster is running at http://host:9000
   * CronusMaster code is saved in ~/cronusmaster-master

   **Deploy First App**

   Now you can try to deploy the first app to local machine through cronusmaster. In CronusMaster (http://host:9000), run "Deploy Helloworld App", wait for it complete, check sample application running at http://host:8999

Install Agent Only
-------------------

   .. code-block:: bash

      cd /tmp; wget -qO- 'http://cronuspaas.github.io/downloads/install_agent' | sudo dev=true bash

   expected outcome:

   * CronusAgent is running at https://localhost:19000/agent

Tested IaaS Compute Profiles
-----------------------------

The following IaaS compute profiles have been tested working with Cronus.

AWS

* Ubuntu12.04
* Ubuntu14.04
* RHEL7

GCP

* Ubuntu12.04
* Ubuntu14.04
* RHEL7
* CentOS7

Azure

* Ubuntu12.04
* Ubuntu14.04
* Openlogic7
