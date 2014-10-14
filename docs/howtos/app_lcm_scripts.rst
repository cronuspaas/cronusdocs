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

