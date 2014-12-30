Basic Authentication
===============

This guide describe

* How to install agent with SSL cert
* How to enable agent basic authentication

.. code-block:: bash

    wget -qO- 'http://cronuspaas.github.io/downloads/install_agent' \
    | sudo server_pem={path_to_pem_file} agent_pwd={user:password} bash


**Expect outcome**

* Agent up and running with custom SSL cert
* Agent CUD APIs requires http Basic authentication (HTTP header "Authorization: Basic {base64_encoded_user:password}")


