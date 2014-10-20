OneClick Launch
===================

Oneclick command captures all the input necessary to launch a command job, this allows a job to be launched with "one click" (or one API call).

**Why it is useful**

* Deploy or rollback applications with one click or one API call
* Save a job execution for repeated execution easily
* Can auto adapt with evolving parameters, for example always deploy latest application package to target nodes

**Sample oneclick launch**

.. code-block:: javascript

   {
    "name": "deploy_pyserver_local",                      // unique name of the oneclick command
    "userData": {                                         // custom user data
      "exe_retry": "3",                                   // execution options
      "mon_retry": "3",
      "thrStrategy": "UNLIMITED",
      "mon_int": "10"
      // user data
      "var_values": "{\"serviceName\":\"pyserver\",\"package\":[\"http://<cmInternalIp>:9000/agent/downloadPkg/<pyserver-.*.all.cronus.latest>\"]}",
    },
    "commandKey": "Agent_Deploy_Service",                 // command name
    "nodeGroupKey": "LOCALHOST"                           // nodegroup name
   }

**Create oneclick launch**

Oneclick launch can be created in two ways

#. From command log tab, save an already executed command job as oneclick launch
#. From oneclick launch tab, create by cloning an existing oneclick launch

**Run oneclick**

Run from "oneclick launch" tab with a single click, or one API call, will redirect to command job log page upon successful launch.

