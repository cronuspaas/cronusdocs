Getting Started
==============================

Steps to install Cronus agent and Cronus master on a machine.

**Prerequisites**

* linux (Ubuntu, Cent, Redhat, Fedora etc.)
* sudo permission
* python > 2.6.7 && < 3
* openssl > 0.9.8
* wget
* system management daemon systemd or upstart

**Install Agent and Master**

   .. code-block:: bash

      # Prepare GCE centos7 VM
      sudo yum install wget

      # Prepare EC2 ubuntu14 VM
      sudo apt-get install git

      cd ~
      wget -qO- 'http://www.stackscaling.com/downloads/install_cronusmaster' | bash
      git clone https://github.com/stackscaling/cronusmaster.git

   expected outcome:

   * CronusAgent is running at https://host:12020/agent
   * CronusMaster is running at http://host:9000
   * CronusMaster code is downloaded and saved in ~/cronusmaster-master

**Deploy First App**

  In CronusMaster, navigate to Commands -> Oneclick Launch, run deploy_pyserver_local, wait for it complete, check application running at http://host:8999

**Install Agent Only**

   .. code-block:: bash

      cd /tmp
      wget -qO- 'http://www.stackscaling.com/downloads/install_agent' | sudo dev=true bash

   expected outcome:

   * CronusAgent is running at https://localhost:12020/agent

