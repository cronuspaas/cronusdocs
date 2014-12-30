Application Lifecycle Scripts
===============

This guide describe how to create application lifecycle scripts.

#. Bootstrap your application with cronus package structure and lifecycle script skeleton

   .. code-block:: bash

      cd application_root
      wget -qO- 'http://cronuspaas.github.io/downloads/bootstrap_cronus' | DIR=. bash

#. Modify scripts to your need

   * Startup script have nodaemon mode, distinguished by cmd line parameter $1="nodaemon" 
      * in nodaemon mode, application process is managed by external service manager (systemd or upstart), in nodaemon mode startup script needs to start the application and block until application exit.
      * in daemon mode, use exec or nohup to fork your application main process, also use setsid to detach your application process from parent agent process.
   * All parameters passed in from agent deploy API call are made available to lifecycle scripts as environment parameters except startup and shutdown scripts, reason being the two script can be executed multiple times out of context of a deployment.
   * Locally persisted application data that have lifecycle beyond the manifest can be accessed through .appdata symlink under the package root directory.

#. Test locally make sure scripts are working as expected

   * Optionally delete the scripts that are not needed

#. Package and upload, test deploy remotely in the cloud

