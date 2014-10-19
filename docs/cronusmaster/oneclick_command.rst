OneClick Command
===================

Oneclick command captures all the input necessary to launch a command job, this allows a job to be launched with "one click" (or one API call).

**Sample oneclick command**

.. code-block:: javascript

   {
    "name": "deploy_pyserver_local",                      // unique name of the oneclick command
    "userData": {                                         // custom user data
      "exe_retry": "3",
      "mon_retry": "3",
      "thrStrategy": "UNLIMITED",
      "var_values": "{\"serviceName\":\"pyserver\",\"package\":[\"http://<cmInternalIp>:9000/agent/downloadPkg/<pyserver-.*.all.cronus.latest>\"]}",
      "mon_int": "10"
    },
    "commandKey": "Agent_Deploy_Service",                 // command name
    "nodeGroupKey": "LOCALHOST"                           // nodegroup name
   }


