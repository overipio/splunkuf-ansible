Splunk Universal Forwarder Ansible role
=======================================

[![Build Status](https://travis-ci.org/overipio/splunkuf-ansible.svg?branch=master)](https://travis-ci.org/overipio/splunkuf-ansible)

> Basic install and configuration of a splunk forwarder


Role Variables
--------------

At the moment, the entire input/output config files for splunk are configured via variables as you can see in the defaults below.

```yaml

# SYSTEM RELATED
# what user splunk uf should run under
splunkforwarder_system_user: splunk
splunkforwarder_path: /opt/splunkforwarder
splunkforwarder_start_on_boot: yes
splunkforwarder_get_via_curl: no

# PACKAGE LOCATION
# splunk uf binary to install. 
splunkforwarder_url: 'https://www.splunk.com/bin/splunk/DownloadActivityServlet?architecture=x86_64&platform=linux&version=6.5.1&product=universalforwarder&filename=splunkforwarder-6.5.1-f74036626f0c-Linux-x86_64.tgz&wget=true'
splunkforwarder_filename: 'splunkforwarder-6.5.1-f74036626f0c-Linux-x86_64.tgz'
splunkforwarder_md5: 'md5:e8468b95b4ca03f73f33714a4430c82e'

# AUTH RELATED
# login specific stuff
splunkforwarder_user: admin
splunkforwarder_pass: changeme

# DEFAULT INDEX
splunkforwarder_default_index: default

# CONFIG FILE CONTENTS
# Likely a better way to do this, but to get started, here are the config files
# we want to deploy to the system
splunkforwarder_outputs: |
  defaultGroup = primary

  [tcpout:primary]
  server = localhost:9997

# add all the input you want here, basic default
splunkforwarder_inputs: |
  [default]
  index         = default

  [monitor://$SPLUNK_HOME/var/log/splunk]
  index = _internal
```


Example Playbook
----------------

```yaml
- hosts: servers
  roles:
    - overipio.splunk-universalforwarder
```

Add additional monitors to an existing installation

```yaml
- hosts: servers
  vars:
    - splunkforwarder_inputs_monitor:
        - path: "/opt/applications/helloworld/*.log"
          sourcetype: application_log
          index: differentindex
        - path: "/opt/foo/bar.log"

  roles: 
    - overipio.splunk-universalforwarder
