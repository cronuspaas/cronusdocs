Application Lifecycle
=======================

This section describes application lifecycle defined by Cronus framework.


Terminology
--------------------

* Service: an application, one or more instances serving the same function and code.
* Manifest: a version of an application, composed of one or more packages. Multiple manifests may be deployed to a ServiceInstance, but only one can be active at a time.
* Active Manifest: a manifest actively in use.
* Package: the smallest unit of deployment that Cronus understands. It can be a tarball containing application runtime, code, config, and cronus structure required for deployment.
* Cronus Package: a cronus structure that is zip-tarred as a {service}-{version}.{platform}.cronus package.
* ServiceInstance - is a single instance of a service, typically mapped to a single compute.

Application Hierarchy
-----------------------

* A service have one or many manifests, among which, 0 or 1 active manifest, one manifest can have one or many cronus packages
   service (1) --> (1..n) manifests (1) --> (1..n) packages
   service (1) --> (0..1) active_manifest_x
* A service deployed in cloud have one or many compute, each deployed with the active manifest of the same version
   service_deployed_in_cloud --> (1..n) service_instances (1) --> active_manifest_x

Application Life Cycle
-----------------------

* Service lifecycle: Service can be created on a compute, and later can be deleted if no active manifest presents.
* Manifest lifecycle: Manifest can be created on a compute for a particular service, activate a manifest will deactivating any existing active manifest and make the manifest active manifest, active manifest can be startup, shutdown, and restart, active manifest can be deactivated and then deleted.

Application Package Structure (aka Cronus Package)
---------------------------------------------------------------------

* Cronus Package
   All packages within an application manifest need to be packaged with certain structure and provide a set of life cycle scripts

   .. code-block:: bash

      package_root/              # package root directory
          cronus/                # cronus directory
              cronus.prop        # prop file describe the package
              cronus.ini         # configuration file in json format to customize agent behavior
              scripts/           # optional scripts directory containing package scripts
                  install        # execute when package is untarred the first time, executed once only during package life time.
                  activate       # execute when parent manifest has been set active.
                  startup        # execute to 'start' a package
                  running        # execute to check package runtime state
                  shutdown       # execute to 'shut down' a package
                  deactivate     # execute when parent manifest gets deactivated
          ...                    # any application files or directories to be packaged

* Cronus package deployed on compute

   .. code-block:: bash

      /path_to_service_home/{service}     # service root directory
         .metadata.json                   # metadata json file
         .appdata/                        # application data directory, shared among manifests
         manifests/                       # application manifests
            active                        # symlink point to active manifest
            1.0.0/                        # manifest 1.0.0
               pkg1                       # inflated cronus package1

* Naming Convention
   Tarred cronus package has to follow the naming convention of "{packageName}-{majorVersion}.{minorVersion}.{incrementalVersion}.[optionalVersionSuffix].{platform}.cronus", e.g. agent-1.0.0.x86_ubuntu.cronus
   An optional corresponding property file that specify metadata of the cronus package, most importantly md5 checksum of the cronus package, property file has to follow the naming convention of "{sameCronusPackageName}.prop", e.g. PortalWebAppConfiguration-1.0.0.unix.cronus.prop, for example

   .. code-block:: bash
      {
      "name":"agent-0.1.45.unix.cronus",
      "md5":"74680838d5d487912a99ee4913e974fc",
      "description":"agent package",
      "long description":"optional long description",
      "size":1199403,
      "contact":"user@abc.com"
      }

Application Life Cycle Management
----------------------------------------------
**Life Cycle Management Scripts**

  Agent requires a set of LCM scripts in order to control the application life cycle precisely

 ========== ========= =====================
  Script     Required        Description
  ========== ========= =====================
  install    optional  additional installation operations after software package 
                       is uncompressed and manifest created run only once 
                       within manifest life time
  activate   optional  activate manifest run once every time manifest is 
                       activated, or reset
  startup    required  start the application run once every time application is 
                       activated, startup, restart, or reset
  shutdown   optional  shutdown the application run once every time application is 
                       activated, shutdown, restart, or reset
  deactivate optional  deactivate the application run once every time manifest is 
                       activated, or reset
  ========== ========= =====================

* Because application startup script is called by a process launched by agent, one must make sure that

 * Startup script returns without blocking
 * Fork the application process to a separate process
 * Detach the application process from its parent process, use setsid() to make the new process new group leader so that it does not terminate when agent process shutdown/restart. For more details see http://stackoverflow.com/questions/2613104/why-fork-before-setsid

**Passing Parameters**

* Default environment variables: agent injects the following environment variables to application before invoking application life cycle scripts

  * $CRONUSAPP_HOME: absolute path to the application service root directory
  * $LCM_CORRELATIONID: correctionid if any passed for the LCM API

* Additional environment variables: any additional parameters passed in through agent deploy API are made available to LCM scripts as environment variables

**Timeouts and Passing Information to Agent**

* Timeout: Scripts must exit before timeout expires, or process be killed, default timeout is 15 minutes configurable by agent config.
* Progress Timeout: Scripts must demonstrate progress (progress number increasing) by passing progress information to agent or process be killed, default progress timeout is 15 minutes configurable by agent config.
* Passing Information to Agent while Running: Scripts can pass progress and other information to agent via stdout while running, in syntax

  .. code-block:: javascript

    [AGENT_MESSAGE] 
    {
        "progress": 50,
    }
    [AGENT_MESSAGE_END]

* Passing Result to Agent at Exit: Scripts can pass result to agent via its return code and stdout. 

  * 0: success 
  * Any non-zero return code: failure 
  * Additional information including status, progress, and message can be passed to agent via stdout, in syntax

    .. code-block:: javascript

      // sample message to agent for progress or for success
      [AGENT_MESSAGE] 
      {
        "progress": 100,
        "result":[
          {"key": {result_key}, "value": {result_value}}
        ]
      }
      [AGENT_MESSAGE_END]

      // sample message to agent for error

      [AGENT_MESSAGE] 
      {
        "error": {error_code},
        "errorMsg": {error_message},
        "result":[
          {"key": {result_key}, "value": {result_value}}
        ]
      }
      [AGENT_MESSAGE_END]

  * Stdout or Stderr: while executing application script, agent reads from stdout and process any message matches the above format and use it to update status. If script fails with non-zero return code, agent reads from stderr, or if it is missing, from last readout from stdout, for anything matches the above format and update error status. Both status can be retrieved from agent status API "/status/:uuid"
  * Mutli-line support: with single-line output, [AGENT_MESSAGE_END] can be omitted. With mutli-line output, agent looks for [AGENT_MESSAGE_END] as end of message indicator, there is a limit of 8k for agent message
  * Encoding: only ascii is supported, other encoding will be skipped

