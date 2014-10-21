This recipe describes steps to package an application ready for cronus deployment

* Bootstrap application with packaging script
```
cd app_root
curl -sSL 'http://www.stackscaling.com/downloads/bootstrap_agent' | bash
```
* Modify package.sh with appropriate values for appname, version, src_pkgs, and platform
* Package the application: ./package.sh
* Package and upload to cronusmaster
```
export cronusmaster=_cronusmaster_ip_
./package.sh upload
```

Expected Outcome

* Cronus package and its prop file in app_root/target_cronus/appname-version.platform.cronus
* If uploaded, check package available in cronusmaster UI under Agent->Package 
