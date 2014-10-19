Manage Command
===================

**Create command**

The best way to create command is to clone from an existing command, one needs to determine the command type

* async: request with reponse as the result
* asyncpoll: request followed by polling to get result

Fill in the command template with http request values, similar to defining http request in RESTful client like POSTMAN

Template parameter can be used in all http reuqest values, template parameter is identified by "<>", minimally if the command can be executed against different hosts, "<host>" needs to be part of the url value. Template parameters will be replace with real values at execution time, or if values are not provided, the parameters will be skipped.

Also defines user data, it will be use to drive command launch wizard

**Modify command**

Command can be modified, however the name of the command is immuatable

**Delete command**

Delete command from persistent store

**Run command** 

Run command will launch command wizard, allow user to select nodegroup against which command will be run, custom user data, and start command job
