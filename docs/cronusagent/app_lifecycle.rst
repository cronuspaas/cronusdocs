Application Lifecycle
=======================

This section describes application lifecycle defined by Cronus framework.


**Terminology**

* Service: an application, one or more instances serving the same function and code.
* Manifest: a version of an application, composed of one or more packages. Multiple manifests may be deployed to a ServiceInstance, but only one can be active at a time.
* Active Manifest: a manifest actively in use.
* Package: the smallest unit of deployment that Cronus understands. It can be a tarball containing application runtime, code, config, and cronus structure required for deployment.
* Cronus Package: a cronus structure that is zip-tarred as a {service}-{version}.{platform}.cronus package.
* ServiceInstance - is a single instance of a service, typically mapped to a single compute.

**Application Hierarchy**

* A service have one or many manifests, among which, 0 or 1 active manifest, one manifest can have one or many cronus packages
   service (1) --> (1..n) manifests (1) --> (1..n) packages
   service (1) --> (0..1) active_manifest_x
* A service deployed in cloud have one or many compute, each deployed with the active manifest of the same version
   service_deployed_in_cloud --> (1..n) service_instances (1) --> active_manifest_x

**Application Life Cycle**

* Service lifecycle: Service can be created on a compute, and later can be deleted if no active manifest presents.
* Manifest lifecycle: Manifest can be created on a compute for a particular service, activate a manifest will deactivating any existing active manifest and make the manifest active manifest, active manifest can be startup, shutdown, and restart, active manifest can be deactivated and then deleted.

**Application Package Structure (aka Cronus Package)**

* Cronus Package
   All packages within an application manifest need to be packaged with certain structure and provide a set of life cycle scripts

   .. code-block:: bash

      package_root/                       # package root directory
          cronus/                         # cronus directory
              cronus.prop                 # prop file describe the package
              cronus.ini                  # configuration file in json format to customize agent behavior
              scripts/                    # optional scripts directory containing package scripts
                  install                 # execute when package is untarred the first time, executed once only during package life time.
                  activate                # execute when parent manifest has been set active.
                  startup                 # execute to 'start' a package
                  running                 # execute to check package runtime state
                  shutdown                # execute to 'shut down' a package
                  deactivate              # execute when parent manifest gets deactivated
          ...                             # any application files or directories to be packaged

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

**Cronus packaging support**

Build deployable Cronus Package for Generic Application (Working Example) Generic Scripts for Cronus Package

**Application Life Cycle Management**

* LCM scripts

Agent requires a set of LCM scripts in order to control the application life cycle precisely
Script	Required	Description
install	optional	

    additional installation operations after software package is uncompressed and manifest created
    run only once within manifest life time

activate	optional	

    activate manifest
    run once every time manifest is activated, or reset

startup	        required	

    start the application
    run once every time application is activated, startup, restart, or reset

shutdown	optional	

    shutdown the application
    run once every time application is activated, shutdown, restart, or reset

deactivate	optional	

    deactivate the application
    run once every time manifest is activated, or reset


Special care when launch application process from startup script

Because application startup script is called by a process launched by agent, one must make sure that

    Startup script returns without blocking
    Fork the application process to a separate process
    setsid() to make the new process new group leader so that it does not terminate when agent process shutdown/restart
    For more details see http://stackoverflow.com/questions/2613104/why-fork-before-setsid

Environment variable made available to LCM scripts

Agent injects the following environment variables to application before invoking application life cycle scripts

    $CRONUSAPP_HOME: absolute path to the application service root directory on the local machine e.g. /var/cronus/software/service_nodes/myservice
    $LCM_CORRELATIONID: correctionid if any passed for the LCM API

Application service metadata made available to LCM scripts

How to pass additional parameters to LCM scripts

    Set parameters in service metadata with namespace key like {"metadata" : {"lcm.parameters.startup" : "-a -x something"}} use service updatemetadata API
    Consume the parameter from local .metadata.json in the LCM scripts
    For more information on updatemetadata for service, see Agent REST API

**LCM script communication to Agent**

Application life cycle scripts can communicate its result to agent via its return code. Code '0' means success, anything else means failure. More information including status, progress, and message can be communicated to agent via the following format

Sample message to agent for progress or for success
[AGENT_MESSAGE] 
{
	"progress":100,
	"result":[
    	{"key":"resultKey", "value":resultValue}
    ]
}
Sample message to agent for error
[AGENT_MESSAGE] 
{
	"error":{errorCode},
	"errorMsg":{errorMessage},
	"result":[
    	{"key":"resultKey", "value":resultValue}
    ]
}

    Return code: non-zero return code = failure
    Stdout or Stderr: while executing application script, agent reads from stdout and process any message matches the above format and use it to update status. If script fails with non-zero return code, agent reads from stderr, or if it is missing, from last readout from stdout, for anything matches the above format and update error status. Both status can be retrieved from agent status API "/status/:uuid"
    Mutli-line support: with mutli-line output, agent looks for line with ending "}" as end of message indicator, there is a limit of 8k for agent message
    Encoding: only ascii is supported, other encoding will be skipped

### Application Life Cycle Management APIs
Summary

Path
	

GET
	

PUT
	

POST
	

DELETE
	

Description

/services
	

list all services
	

400
	

400
	

400
	

/services/:sid
	

list manifests of a service
	

400
	

creates service
	

deletes service
	

requires lock on service for delete

/service/:sid/manifests/:mid
	

list packages of a manifest
	

400
	

creates manifest
	

deletes manifest
	

requires lock on service
/service/:sid/action/:aid	400	400	

perform life cycle actions on service, aid

    activatemanifest: shutdown old -> deactivate old -> switch active link -> activate new -> startup new
    deactivatemanifest: shutdown active -> deactivate active -> remove active link
    startup: startup active
    shutdown: shutdown active
    restart: shutdown active -> startup active
    reset: shutdown active -> deactivate active -> activate active -> startup active

	400	requires lock on service
/services/:sid/action/:aid	

    getmetadata: get application service metadata (from local .metadata.json)
    log: view log

		

    updatemetadata: update application service metadata file (.metadata.json)

		no lock required
**LCM APIs in action**

create service
curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST https://host:12020/services/myservice

create manifest
curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST -d '{"package": ["http://pkgrepo/tomcat-7.0.0.unix.cronus", "http://pkgrepo/myservice-1.0.0.unix.cronus"]}' https://host:12020/services/myservice/manifests/myservice-1.0.0

activate manifest
curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST -d '{"manifest": "myservice-1.0.0"}' https://host:12020/services/myservice/action/activatemanifest

restart application
curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST https://host:12020/services/myservice/action/restart

reset application
curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST https://host:12020/services/myservice/action/reset

stop application
curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST https://host:12020/services/myservice/action/stop

deactivate manifest
curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST https://host:12020/services/myservice/action/deactivatemanifest

delete manifest
curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X DELETE https://host:12020/services/myservice/manifests/myservice-1.0.0

delete service
curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X DELETE https://host:12020/services/myservice
 

**LCM APIs working examples**

    Deploy an application

    # create service (THIS ONLY NEEDS TO BE RUN ONCE, BEFORE THE FIRST EVER DEPLOYMENT OF THE APPLICATION), this create an application on the machine called "myservice"
    curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST https://host:12020/services/myservice

    # create manifest, this create a new version of the application "myservice" on the machine, in this case the version is "myservice-1.0.0"
    curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST -d '{"package": ["http://pkgrepo/tomcat-7.0.0.unix.cronus", "http://pkgrepo/myservice-1.0.0.unix.cronus"]}' https://host:12020/services/myservice/manifests/myservice-1.0.0

    # activate manifest, this deactivate an existing version if any, and activate the new version "myservice-1.0.0"
    curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST -d '{"manifest": "myservice-1.0.0"}' https://host:12020/services/myservice/action/activatemanifest

    Restart or Reset an application

    # restart application, this shutdown and start the current active manifest of the application
    curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST https://host:12020/services/myservice/action/restart

    # reset application, this shutdown, deactivate, activate, and start the current active manifest of the application
    curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST https://host:12020/services/myservice/action/reset

    Shutdown an application

    # stop application, this shutdown the current active manifest of the application
    curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST https://host:12020/services/myservice/action/stop

    Uninstall an application 

    # deactivate manifest, if any
    curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST https://host:12020/services/myservice/action/deactivatemanifest

    # delete service, cleanup everything related to the service from the machine
    curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X DELETE https://host:12020/services/myservice

**Force install fresh packages**

Use case: Typically agent will skip download and unzip a package if it already exists on the machine. Sometimes it's desirable to force install a fresh package even if it is already present on the machine

How:

    In create manifest, metadata indicate force download package even it already exist in unzipped form in installed-packages folder

    # create manifest with force install, this will force install tomcat package even the same version already existed on the machine
    curl -k -H "Authorization:Basic YWdlbnQ6dG95YWdlbnQ=" -H "content-type:application/json" -X POST -d '{"package": ["http://pkgrepo/tomcat-7.0.0.unix.cronus", "http://pkgrepo/myservice-1.0.0.unix.cronus"], "forcePackageName": ["tomcat"]}' https://host:12020/services/myservice/manifests/myservice-1.0.0

    Unzip to a different location in installed-packages folder, with an added suffix of manifestname (formatted to remove special characters)

    # installed packages directory with force installation
    -installed-packages
      -tomcat
         -7.0.0.myservice100		# used by current active manifest
         -7.0.0.myservice101		# force download and installed by new manifest

**Additional Configurations**

Additional configurations are available in cronus.ini that provide desirable features and customization 
View application logs via agent

Specify application log directory in cronus.ini, and use agent API to view application logs

{
	"logAppDir": ["cronus/scripts/log"]
}

Note: the logging directory needs to have group level rx access for agent to access and read files, one may consider creating this directory with the right permission in your source control, or create it by activation script with the right permission

View application logs https://host:12020/services/{serviceid}/action/log

Note: the link work for either full serviceid or serviceid prefix (first 2 of 3 parts of the service id, et. .env.appsvc)
Configurable LCM script timeouts

Application can control agent behavior on life cycle operations via package configurations

To use this feature, specify the following configuration in cronus.ini

{
	"lcm_script_options": {
		"startup": {"timeout": 3600, "progress_timeout": 1200},
		"shutdown": {"timeout": 3600, "progress_timeout": 1200},
		"activate": {"timeout": 3600, "progress_timeout": 1200},
		"deactivate": {"timeout": 3600, "progress_timeout": 1200},
		"install": {"timeout": 3600, "progress_timeout": 1200},
		"uninstall": {"timeout": 3600, "progress_timeout": 1200}
	}
}

Note: configurations are optional, if not specified, agent will use values from agent default configuration (timeout=3600 sec, progress_timeout=600 sec)
Behaviors system restart or agent restart

Application can control agent behavior on how to handle application on Restart scenarios via cronus.ini configuration
Scenario	Agent Behavior	Default
System restart (reboot)	startup|reset|noop	startup
Agent restart	startup|reset|noop	noop

{
	"lcm_on_system_reboot": "startup|reset|noop",
	"lcm_on_agent_restart": "startup|reset|noop"
}

