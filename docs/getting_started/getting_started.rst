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

      cd ~
      git clone https://github.com/stackscaling/cronusmaster.git
      cd cronusmaster/AgentMaster/conf
      cp application.conf.template application.conf
      cp application.conf.template application.conf.prod
      ./install.sh

   expected outcome:

   * CronusAgent is running at https://localhost:12020/agent
   * CronusMaster is running at http://localhost:9000


**Install Agent Only**

   .. code-block:: bash

      cd /tmp
      wget -qO- 'http://www.stackscaling.com/downloads/install_agent' | sudo dev=true bash

   expected outcome:

   * CronusAgent is running at https://localhost:12020/agent

