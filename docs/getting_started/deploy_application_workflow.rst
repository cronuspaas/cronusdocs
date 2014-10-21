Deploy Application Workflow
==============================

This document describe steps to deploy an application to cloud by using Cronus framework.

**Prerequisites**

* Cronus agent and Cronus master are already installed

**Application Packaging**

* Checkout desired application, package and upload

   .. code-block:: bash

      cd ~
      git clone https://github.com/stackscaling/cronusstacks.git
      cd cronusstacks/node_app
      ./package.sh upload

   Expected outcome: node_app is packaged and uploaded to local CronusMaster, check http://localhost:9000/agent/packages for the uploaded package


**Deploy and Rollback**

* Deploy
   
   #. In CronusMaster, navigate to command -> oneclick launch
   #. Run "deploy_nodeapp_local", wait for it to complete
   #. Check for nodeapp at http://localhost:1337/

* Rollback

   #. You will need to deploy another version before rollback, make some change to the nodeapp code, repeat above step for packaging, upload, deploy, and verify that your change is deployed
   #. Run oneclick launch "rollback_nodeapp_local", wait for it to complete
   #. Check the change is rolled back

**Startup, Shutdown and Restart**

* In CronusMaster, navigate to commands -> commands
* Run Agent_Service_LCMAction, go through command wizard, select nodegroup "Localhost", fill in json user data of "serviceName": "nodeapp", "action": "restart", execute
* Wait for job to complete, click fulltext search in cmd job page to see the raw script output, note that shutdown and startup scripts are called respectively
* Repeat the above step for action="shutdown", and action="startup"

**Other Excercises**

* Create a new nodegroup with list of nodes that are not localhost, install agent on them and deploy the same node application to the new nodegroup
* Create a recurring job that deploy latest nodeapp to localhost every 5 minutes
* Deploy a tomcat application to localhost, or create a new application stack ready for cronus deployment from scratch

