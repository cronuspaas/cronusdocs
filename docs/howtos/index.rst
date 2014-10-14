======================
Cronus Howto
======================


Access locally persisted application data
===============

**Data local to specific manifest**

Any data within the manifest package, or generated after activation in the manifest package is local to the specific manifest only. It can be deleted by agent after the manifest is deactivated and garbage collected

**Data local to specific service**

Agent creates .appdata under service root directory and creates symlink .appdata in each cronus package root during manifest activation. Data persisted in .appdata and its subdirectories is accessible across all manifests of the service. It can be deleted by agent when the service is deleted.

::

   - service_root
       - .appdata
       - manifests
           - active
               - package1
                   - .appdata (symlink to service_root/.appdata)




**Environment variables**

Any data passed in from deploy API will be available to the application life cycle script as environment variables

::

    # deploy API call
        curl -k -X POST -d '{"package": ["http://$_cronus_package_url_"], "env": "production"' \
        https://host:12020/services/$_service_name_/action/deploy

    # in activate script, this will print env=production
        echo "env=$env"


Create application lifecycle scripts
===============

#. Bootstrap your application with cronus package structure and lifecycle script skeleton

   In application root, run ``wget -qO- 'http://www.stackscaling.com/downloads/bootstrap_cronus' | DIR=. bash``

#. Complete scripts

   * Startup script have nodaemon mode, distinguished by cmd line parameter $1="nodaemon", in nodaemon mode, application process is managed by external service manager (systemd or upstart), hence startup script needs to start the application and block until application exit.
   * To start application in daemon mode, use exec or nohup to fork your application main process.
   * To start application in daemon mode, use setsid to detach your application process from parent agent process.
#. Test locally make sure scripts are working as expected
 
   * Optionally delete the scripts that are not needed


Setup SSL with basic Authentication
===============

This section describes:

# How to install agent with SSL cert
# How to install agent with basic authentication

::

    wget -qO- 'http://www.stackscaling.com/downloads/install_agent' \
    | sudo server_pem={path_to_pem_file} agent_pwd={user:password} bash


**Expect outcome**

# Agent up and running with custom SSL cert
# Agent CUD APIs requires http Basic authentication (HTTP header "Authorization: Basic {base64_encoded_user:password}")


Upgrade agent
=================

This section describes how to upgrade an existing Agent

**New agent package must exist in release folder in git http://github.com/stackscaling/cronusagent/releases/agent-{target_version}.unix.cronus**

::

    curl -k -H "content-type:application/json" -X POST -d '{"version": "version_eg_0.1.46"}' \
    https://host:12020/agent/selfupdate

or

Update through cronusmaster UI

**Expect Outcome:** 

Agent upgraded to new version, version number can be found in agent VI page https://host:12020/agent/ValidateInternals.html

Others:

.. toctree::
   :maxdepth: 1 

   Agent_API_Howto
   Agent_Config_Howto

