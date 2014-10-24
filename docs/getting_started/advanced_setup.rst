Advance Setup
===============

Advance CronusMaster Settings
-----------------------------

Default cronusmaster installation uses application.conf, edit configurations of application.conf.{env} for additional features

.. code-block:: bash

   # cronusmaster public and private IP, needed for smart parameters in command 
   agentmaster.externalIp=cronusmaster_external_ip
   agentmaster.internalIp=cronusmaster_internal_ip

   # user data location (applicable only if use local fs for user data)
   agentmaster.userDataDao.file.dir=.appdata

   # agent basic authentication
   agentmaster.cronusagent.password=username:password

Redeploy CronusMaster after changes ``scripts/install.sh {env}``


Advance CronusAgent Settings
------------------------------

Default cronusagent installation uses default SSL cert, and no authentication, install agent with the following parameters for additional features

.. code-block:: bash

   # provide path_to_ssl_cert for custom ssl cert
   # set user:password for basic authentication
   curl -sSL 'http://www.stackscaling.com/downloads/install_agent' \
   | sudo server_pem=path_to_ssl_cert agent_pwd=user:password bash

Agent is reployed with new ssl cert, and basic authentication enabled, once authentication enabled, CronusMaster configuration of "agentmaster.cronusagent.password" needs to be updated to be able to work with updated agent.

