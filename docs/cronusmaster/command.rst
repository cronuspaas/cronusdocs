Command Template
=========================

Command template is a complete HTTP(S) request in json format with replaceable variables

**Sample synchronous HTTP request**

.. code-block:: javascript

  {
   "name" : "SAMPLE_SYNC_COMMAND",
   "httpTaskRequest" : {
     "taskType" : "async",
     "httpClientType" : "httpClient",
     "executionOption" : {
       "timeOutSec" : 0,
       "initDelaySec" : 0,
       "maxRetry" : 3,
       "retryDelaySec" : 0
     },
     "urlTemplate" : {
       "url" : "https://<host>:12020/services/<serviceName>",
       "method" : "POST",
       "body" : {
       },
       "headers" : {
         "content-type" : "application/json"
       },
       "parameters" : {
       }
     },
     "resProcessorName" : "agentProcessor"
   },
   "userData" : {
    "serviceName" : "myservice"
   }
  }


**Sample asynchronous HTTP request with polling for result**

.. code-block:: javascript

   {
    "name" : "SAMPLE_ASYNCPOLL_COMMAND",
    "httpTaskRequest" : {
      "taskType" : "asyncpoll",
      "httpClientType" : "httpClient",
      "executionOption" : {
        "timeOutSec" : 0,
        "initDelaySec" : 0,
        "maxRetry" : 3,
        "retryDelaySec" : 0
      },
      "urlTemplate" : {
        "url" : "https://<host>:12020/services/<serviceName>/action/restart",
        "method" : "POST",
        "body" : {
        },
        "headers" : {
          "content-type" : "application/json",
        },
        "parameters" : {
        }
      },
      "monitorOption" : {
        "timeOutSec" : 0,
        "initDelaySec" : 0,
        "maxRetry" : 3,
        "retryDelaySec" : 0,
        "intervalSec" : 10
      },
      "pollTemplate" : {
        "url" : "https://<host>:12020/status/<uuid>",
        "method" : "GET",
        "body" : {
        },
        "headers" : {
          "content-type" : "application/json"
        },
        "parameters" : {
        }
      },
      "resProcessorName" : "agentPollProcessor",
    },
    "userData" : {
      "serviceName" : "myservice"
    }
   }

**Sample agent request**

.. code-block:: javascript


  {
    "name" : "Agent_Clean_ALL_Services",
    "httpTaskRequest" : {
      "taskType" : "asyncpoll",
      "httpClientType" : "httpClient",
      "urlTemplate" : {
        "url" : "https://<host>:12020/agent/cleanup",
        "method" : "POST",
        "body" : { },
        "headers" : { },
        "parameters" : { }
      }
    },
    "userData" : { },
    "category" : "agent"
  }

