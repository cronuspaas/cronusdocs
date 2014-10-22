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
      sudo yum install git

      # Prepare EC2 ubuntu14 VM
      sudo apt-get install git

      cd ~
      git clone https://github.com/stackscaling/cronusmaster.git
      cd cronusmaster/AgentMaster/conf
      cp application.conf.template application.conf
      cp application.conf.template application.conf.prod
      ./install.sh

   expected outcome:

   * CronusAgent is running at https://host:12020/agent
   * CronusMaster is running at http://host:9000

**Deploy First App**

  In CronusMaster, navigate to Commands -> Oneclick Launch, run deploy_pyserver_local, wait for it complete, check application running at http://host:8999

**Install Agent Only**

   .. code-block:: bash

      cd /tmp
      wget -qO- 'http://www.stackscaling.com/downloads/install_agent' | sudo dev=true bash

   expected outcome:

   * CronusAgent is running at https://localhost:12020/agent

