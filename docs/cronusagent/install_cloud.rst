Install in IaaS Cloud 
=========================

Follow the steps to install agent in AWS EC2, or Goggle GCE Cloud

**Prerequisites**

* linux (Ubuntu, Cent, Redhat, Fedora etc.)
* sudo permission
* python > 2.6.7 && < 3
* openssl > 0.9.8
* wget
* system management daemon systemd or upstart

**Install in dev (default ssl cert, no auth)**
 
.. code-block:: bash

   # install agent
   curl -sSL 'http://www.stackscaling.com/downloads/install_agent' | sudo dev=true bash
   # check agent availability
   curl -k https://localhost:12020/agent

**Install in prod**

.. code-block:: bash
   
   # install agent
   curl -sSL 'http://www.stackscaling.com/downloads/install_agent' | \
   sudo server_pem=path_to_ssl_cert agent_pwd=user:password bash
   # check agent availability
   curl -k https://localhost:12020/agent

