Build and Package Cronus Package
==============================

How to build and package a cronus enabled node application and deploy through CronusMaster

* Checkout cronusstacks from git, http://github.com/cronuspaas
* Run download_container.sh to download node runtime (adjust the script for the different node runtime of your choice, and additional node module installation)
* Make sure env variable $cronusmaster=host is set
* Run "package.sh upload" to package and upload the package (package.sh alone will package cronus deployable package)
* Deploy in CronusMaster (look for uploaded package in Agent -> Package section), check app running at http://localhost:1337


.. image:: /images/build_package_cronus.gif

