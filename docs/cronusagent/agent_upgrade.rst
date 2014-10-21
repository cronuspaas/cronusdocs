Agent Upgrade
=================

This section describes how to upgrade an existing Agent

**New agent package must exist in release folder in git http://github.com/stackscaling/cronusagent/releases/agent-{target_version}.unix.cronus**

.. code-block:: bash

    curl -k -H "content-type:application/json" -X POST -d '{"version": "version_eg_0.1.46"}' \
    https://host:12020/agent/selfupdate

or

Update through cronusmaster UI

**Expect Outcome:**

Agent upgraded to new version, version number can be found in agent VI page https://host:12020/agent/ValidateInternals.html


