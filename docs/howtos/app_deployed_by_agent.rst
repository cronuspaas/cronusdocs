Deploy application by agent API
=======================

**Requires:**

* Cronus packaged application HTTP accessible by agent on target machine 

**How:**

.. code-block:: bash

   curl -k -X POST -d '{"package": ["{cronus_package_url}"], "manifest": "{manifest_name}"}' \ 
   https://host:12020/services/{service_name}/action/deploy               

   service_name: mandatory service name
   cronus_package_url: mandatory url location to your cronus package 
   manifest_name: optional manifest name, default to use cronus package version

or 

Just use cronusmaster UI

**Expected Outcome:**

* Service is created if not already exist
* Manifest is created if not already exist
* Any existing active manifest is shutdown, deactivate
* New manifest is install, activate, and startup

