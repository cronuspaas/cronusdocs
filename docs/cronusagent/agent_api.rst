Agent API List
==============

* "GET /services/" : list all services
* "GET /services" : list all services
* "POST /services/{service}" : create service
* "DELETE /services/{service}" : delete service
* "GET /services/{service}" : list manifests of service
* "GET /services/{service}/action/log" : view service log
* "POST /services/{service}/action/updatemetadata" : set service metadata
* "GET /services/{service}/action/getmetadata" : get service metadata as json
* "GET /services/{service}/action/getmetadataprops" : get service metadata as property format
* "GET /services/{service}/action/listpackages" : list all packages of the service
* "POST /services/{service}/manifests/{manifest}" : create manifest
* "DELETE /services/{service}/manifests/{manifest}" : delete manifest
* "GET /services/{service}/manifests/{manifest}" : list all manifests
* "GET /services/{service}/manifests/{manifest}/action/log" : view manifest log
* "POST /services/{service}/manifests/{manifest}/action/activate" : activate manifest
* "POST /services/dist/startdownload" : download cronus package
* "POST /services/dist/validatepackage" : validate md5sum of cronus package against its prop file
* "GET /services/dist/listpackages" : list all packages
* "POST /services/dist/uploadpackage" : upload a cronus package
* "DELETE /services/dist/{package}" : delete a package
* "GET /status/{uuid}" : get status of a job (from job uuid)
* "DELETE /status/{uuid}" : cancel a job (from job uuid)
* "GET /status/uuidoutput/{uuid}" : get raw logs of a job (from job uuid)
* "GET /status/guidoutput/{guid}" : get raw logs of a job (from pass in guid)
* "GET /agent/logs" : view agent logs
* "GET /agent/logs/{fileName}" : view one agent log file
* "GET /agent" : view agent ValidateInternals page
* "GET /agent/ValidateInternals" : agent ValidateInternals in json format
* "GET /agent/ValidateInternals.html" : agent ValidateInternals html page
* "GET /agent/agenthealth" : agent health information in json format
* "GET /agent/monitors" : agent run monitoring value snapshot
* "GET /agent/threads" : agent thread information in json format
* "POST /services/{service}/action/restart" : restart service
* "POST /services/{service}/action/reset" : reset current active manifest (shutdown, deactivate, activate, startup)
* "POST /services/{service}/action/startup" : startup service
* "POST /services/{service}/action/shutdown" : shutdown service
* "POST /services/{service}/useraction/{useraction}" : run user provide script
* "POST /services/{service}/action/deactivatemanifest" : deactivate current active manifest
* "POST /services/{service}/action/deploy" : deploy a service, create service, manifest and activate
* "POST /services/{service}/action/rollback" : rollback a service to last activated version
* "DELETE /services/{service}/action/cleanup" : delete everything of a service
* "POST /agent/cleanup" : delete all services except agent from the host
* "POST /agent/shutdown" : shutdown agent
* "POST /agent/safeshutdown" : shutdown agent when agent is idle (no activity)
* "POST /agent/selfupdate" : upgrade agent
* "DELETE /agent/selfupdate" : cancel an agent upgrade
* "GET /agent/config" : get agent config
* "POST /agent/config" : override agent config
* "DELETE /agent/config" : delete an agent config override
* "POST /agent/cleanproc" : cleanup all cronusapp processes
* "GET /agent/api" : list this agent API
* "POST /agent/gc" : ondemand agent GC
* "GET /agent/servicesinfo" : list all services and everything of the services
* "GET /agent/modules" : get list of agent modules
* "GET /agent/modules/{module}" : get information of a agent module
* "POST /agent/modules/{module}" : create an agent module
* "DELETE /agent/modules/{module}" : delete an agent module
* "POST /admin/executeScript" : execute script
* "POST /admin/executeCmd" : execute a shell command
* "GET /security/listkeys" : list PKI keys
* "POST /security/key/{key}" : create PKI key
* "DELETE /security/key/{key}" : delete PKI key
* "POST security/validatetoken" : validate an existing token

