Security Best practice
========================

**Why:**

* Agent has elevated access to resource required by its workload
* Agent is mission critical service, interference is not desirable
* Agents are deployed everywhere in target infrastructure

**Existing Agent Security Mechanisms:**

* All traffic through agent API on HTTPS
* Basic HTTP authentication or PKI based authentication
* Separation of agent user and app user, application managed by agent does not have same privilege as agent has
* Agent and app users are setup as system accounts with login disabled

**Security Best Practice:**

* Do not enable access to agent API from internet without any restriction. It's best to only allow access from intranet via cronusmaster, or if that's not feasible, only allow access from internet from trusted IPs
* Do NOT use default SSL certs when install agent, always provide your own certs
* Do NOT leave agent API open without authentication, at least set basic auth with your own user:password
* Do NOT keep your credentials in your source code, or encrypt it with your private key

