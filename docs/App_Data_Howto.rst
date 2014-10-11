
Howto guide to locally persisted application data
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

