Configurations
================

This section explains cronus master configurations

**Basic configurations**

Cronus master configuration section in a sample play application.conf

.. code-block:: bash

   # use local file | aws_s3 | openstack_swift to store user data
   agentmaster.userDataDao=file
   agentmaster.logProgressEnabled=true
   
   # cronusmaster public and private IP if any
   agentmaster.externalIp=some_external_ip
   agentmaster.internalIp=some_internal_ip

   # user data location (applicable only if use local fs for user data)
   #agentmaster.userDataDao.file.dir=.appdata

   # s3 related configs (applicable only if use aws_s3 for user data)
   agentmaster.userDataDao.s3.myAccessKeyID=your_access_key_id
   agentmaster.userDataDao.s3.mySecretKey=your_secret_key

   # swift related configs (applicable only if use swift for user data)
   agentmaster.userDataDao.swift.tenantId=your_tenant_id
   agentmaster.userDataDao.swift.tenantName=your_tenant_name
   agentmaster.userDataDao.swift.username=your_username
   agentmaster.userDataDao.swift.password=your_password
   agentmaster.userDataDao.swift.authenticationUrl=authn_url

   # elastic search
   # use embedded or remote elastic search backend
   agentmaster.localEsEnabled=true
   # location of search index (applicable only if use embedded elastic search)
   agentmaster.esData=user_data/elasticsearch_data/data
   # search end point (applicable only if use remote elastic search)
   agentmaster.esEp=some_search_host

   # agent basic authentication
   agentmaster.cronusagent.password=username:password
   # agent PKI authentication
   agentmaster.cronusagent.pkicert=path_to_cert

   # ... rest of the play configuration in application.conf

**Configuration override for different environment**

One can build separate application.conf.{environment} in the play conf/ directory to be used for deploying cronus master in different environment. Upon deployment time

#. Pass the environment value through agent deploy API call, e.g. ``curl -X POST '{..., "env": "prod"}' https://host:19000/services/cronusmaster/action/deploy``
#. Agent will try to find the configuration file with .prod postfix and use it (by renaming it to application.conf)
#. Or if the environment specific configuration file is not found, will use the default base configuration application.conf

Same schemes work for logging configurations (log4j.properties) as well.

