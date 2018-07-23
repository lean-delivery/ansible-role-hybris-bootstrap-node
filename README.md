Node hybris prepare
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-jboss/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-bootstrap-hybris-node.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-bootstrap-hybris-node)
## Summary

This role:
  - prepares host on CentOS 7 to deploy SAP Hybris(c) artifacts.
  - updates configuration file for h-up (/etc/default/h-tools)
Current role can be installed on OS Linux 7.*

Role tasks
------------
  - Install necessary OS packages
  - Install necessary python modules
  - Create group and user for hybris
  - Create and set permissions to hybris-tools directory
  - Create and set permissions to hybris directory
  - Copy hybris-tools files:
    - /opt/hybris-tools/h-up: hybris preinstall utility
    - /sbin/ifup-local: utility to add localaddr record to hosts file. Used in hybris cluster configuration.
  - Add hybrisd service
  - Add hybris environment profile file

Requirements
------------

 - Minimal Version of the ansible for installation: 2.4
 - **Java 8** [![Build Status](https://travis-ci.org/lean-delivery/ansible-role-java.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-java)
 - **Supported OS**:
   - CentOS
     - 7

## Role Variables
--------------	 
  - `htools_files` - map with file names and permissions for hybris preinstall
  - `hybris_path` - hybris root directory
    default: `/opt/hybris`
  - `hybris_platform_path` - path to hybris platform directory
    default: `/opt/hybris/bin/platform`
  - `htools` - configuration for hybris preinstall utility
    - `username` - hybris user name
	  default: `hybris`
	- `groupname` - hybris group name
	  default: `hybris`
	- `hybris_path` - hybris root directory
	  default: `/opt/hybris`
	- `hybris_tools_path` - hybris preinstall directory
	  default: `/opt/hybris-tools`
	- `download_dir` - directory used to download hybris artifacts
	  default: `/tmp`
	- `download_mask` - hybris artifacts mask
	  default: `hybris*.zip`
	- `upload_dir` - directory to unpack hybris artifacts
	  default: `/opt/hybris`
	- `remove_packages` - option to remove artifacts after unpack
	  default: `True`
	- `purge_hybris_home` - to clean hybris root directory before artifacts unpack
	  default: `False`
	- `legacy_structure` - option to set what git structure used <TODO: add info>
	  default: `False`
	- `env_type` - set env type (e.g. QA, DEV, PERF, PROD)
	  default: ``
	- `server_type` - hybris node type (e.g. be, backend, fe, frontend, batch, front, etc)
	  default: ``
	- `artifact_storage` - artifact storage url. Http only supported. S3 is uner testing.
	  default: `http://artifactory.example.com/download`
	- `tomcat_wrapper` - use custom tomcat wrapper in tomcat dir
	  default: ``
	- `initialization_error_strings` - pattern to consider as error in hybris preparation output log (e.g. during DB initialization or update).
	  default: `ERROR(.*)`
	- `initialization_log_check` - to enable error check
	  default: `True`
  - `hybris_env` - hybris variables
    - `env_path` - hybris profile variables file path
	  default: `/etc/profile.d/hybris_path.sh`
	- `ant_opts` - ant options
	  default: `-Xmx512m -Dfile.encoding=UTF-8`
	- `platform_home` - path to hybris platform directory
	  default: `/opt/hybris/bin/platform`
	- `ant_home` - ant home directory
	  default: `/opt/hybris/bin/platform/apache-ant-1.9.1`
	- `custom_ant_targets_path` - start all ant targets from custom directory (used custom buildscripts module)
	  default: `/opt/hybris/bin/custom/buildscripts/resources/buildscripts/ant`
  - `hybrisd_service` - hybrisd service options
    - `username` - user name for hybrisd service
	  default: `hybris`
	- `groupname` - group name for hybrisd service
	  default: `hybris`
	- `platform_home` - path to hybris platform directory
	  default: `/opt/hybris/bin/platform`
	- `service_path` - path to service file
	  default: `/etc/systemd/system/hybrisd.service`
	- `service_pid` - path to service pid file
	  default: `/opt/hybris/bin/platform/tomcat/bin/hybrisPlatform.pid`
	- `service_timeout` - timeout in seconds for service start
	  default: `900`
	- `service_restart` - to restart service by condition
	  default: `always`
	- `service_exec` - executable file for hybrisd service
	  default: `hybrisserver.sh`
  - `hybris_selinux_ports` - ports to add to selinux exception
    default: `9001,9002`
  - `set_localaddr` - add localaddr to hosts file
    default: `True`

Example Playbook
----------------

```yaml
- name: Prepare example
  hosts: all
  roles:
    - role: lean-delivery.ansible-role-java
	  java_major_version: 8
      java_minor_version: 181
      java_arch: "x64"
      java_package: "jdk"
    - role: node-hybris-prepare
```

License
-------

Apache2

Author Information
------------------

authors:
