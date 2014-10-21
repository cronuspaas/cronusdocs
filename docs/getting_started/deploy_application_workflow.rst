Deploy Application Workflow
==============================

This document describe steps to deploy an application to cloud by using Cronus framework.

**Preparation**

#. Install cronus agent and master

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

#. Checkout desired application

   .. code-block:: bash

      cd ~
      git clone https://github.com/stackscaling/cronusstacks.git
      cd cronusstacks/node_app

#. Packaging and Upload**

   .. code-block:: bash

      cd ~/cronusstack/node_app
      ./package.sh upload

   Expected outcome: node_app is packaged and uploaded to local CronusMaster, check http://localhost:9000/agent/cronuspkg for the uploaded package


**Deploy and Rollback**

* Deploy
   
   #. In CronusMaster, navigate to command -> oneclick launch
   #. Run "deploy_nodeapp_local", wait for it to complete
   #. Check for nodeapp at http://localhost:1313/

* Rollback

   #. You will need to deploy another version to be able to rollback, make some change to the nodeapp code, repeat above step for packaging, upload, deploy, and verify that your change is deployed
   #. Run oneclick launch "rollback_nodeapp_local", wait for it to complete
   #. Check the change is rolled back

**Startup, Shutdown and Restart**

* In CronusMaster, navigate to command -> summary
* Run service_action_lcm, go through command wizard, select nodegroup "Localhost", fill json user data of "serviceName": "nodeapp", "action": "restart", execute
* Wait for job to complete, click fulltext search in cmd job page to see the raw script output
* Repeat the above step for "shutdown", and "startup"

**Advanced Topics**
