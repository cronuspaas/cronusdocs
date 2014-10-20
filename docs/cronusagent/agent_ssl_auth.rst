Setup SSL with basic Authentication
===============

This section describes:

# How to install agent with SSL cert
# How to install agent with basic authentication

::

    wget -qO- 'http://www.stackscaling.com/downloads/install_agent' \
    | sudo server_pem={path_to_pem_file} agent_pwd={user:password} bash


**Expect outcome**

# Agent up and running with custom SSL cert
# Agent CUD APIs requires http Basic authentication (HTTP header "Authorization: Basic {base64_encoded_user:password}")


