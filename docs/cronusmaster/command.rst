Command Template
=========================

Command template is a complete HTTP(S) request in json format with replaceable variables

**Sample synchronous HTTP request**

.. code-block:: javascript

  {
   "name" : "SAMPLE_SYNC_COMMAND",                              // unique name of the command
   "category" : "sample",                                       // optional category name for grouping
   "httpTaskRequest" : {
     "taskType" : "async",                                      // this is a http request with response
     "httpClientType" : "httpClient",                           // type of http client to use 
     "executionOption" : {                                      // below are options for sending request
       "timeOutSec" : 0,                                        // time out in second
       "initDelaySec" : 0,                                      // initial delay in second
       "maxRetry" : 3,                                          // total # of retry on failure
       "retryDelaySec" : 0                                      // delay before retry
     },
     "urlTemplate" : {                                          // http request template
       "url" : "https://<host>:19000/services/<serviceName>",   // url
       "method" : "POST",                                       // http method
       "body" : {},                                             // http body
       "headers" : {
         "content-type" : "application/json"                    // http headers
       },
       "parameters" : {}                                        // query string parameters
     },
     "resProcessorName" : "agentProcessor"                      // bean for processing http response
   },
   "userData" : {                                               // user provided data to launch command Job
    "serviceName" : "myservice"
   }
  }


**Sample asynchronous HTTP request with polling for result**

.. code-block:: javascript

   {
    "name" : "SAMPLE_ASYNCPOLL_COMMAND",
    "category" : "sample",
    "httpTaskRequest" : {
      "taskType" : "asyncpoll",                              // this is a request followed by polling
      "httpClientType" : "httpClient",
      ...                                                    // same url template as above for request
      "monitorOption" : {                                    // options for polling 
        "timeOutSec" : 0,                                    
        "initDelaySec" : 0,
        "maxRetry" : 3,
        "retryDelaySec" : 0,
        "intervalSec" : 10                                   // polling interval in second
      },
      "pollTemplate" : {                                     // http polling template
        "url" : "https://<host>:19000/status/<uuid>",
        "method" : "GET",
        "body" : {},
        "headers" : {
          "content-type" : "application/json"
        },
        "parameters" : {}
      },
      "resProcessorName" : "agentPollProcessor",             // bean for processing http response
    },
    "userData" : {
      "serviceName" : "myservice"
    }
   }

**Sample agent request**

.. code-block:: javascript


  {
    "name" : "Agent_Clean_Service",
    "category" : "agent",              // agent command with many fields predefined and can be skipped
    "httpTaskRequest" : {
      "taskType" : "asyncpoll",
      "httpClientType" : "httpClient",
      "urlTemplate" : {
        "url" : "https://<host>:19000/services/<serviceName>/action/cleanup",
        "method" : "POST",
        "body" : { },
        "headers" : { },
        "parameters" : { }
      }
    },
    "userData" : {
      "serviceName" : "myservice"
    }
  }

