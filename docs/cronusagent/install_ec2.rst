Install agent and master on AWS VM
=========================

.. code-block:: bash

   # install agent
   curl -sSL 'http://www.stackscaling.com/downloads/install_agent' | sudo dev=true bash
   # check agent availability
   curl -k https://localhost:12020/agent

   # install cronus master
   git clone https://github.com/stackscaling/cronusmaster.git
   # manually copy AgentMaster/conf/application.conf.template 
   # to application.conf.prod and change config values as needed
   ./install.sh prod
   # check agent master availability
   curl http://localhost:9000/

