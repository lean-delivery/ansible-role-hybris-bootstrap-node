Node hybris prepare
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-hybris-bootstrap-node/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-hybris-bootstrap-node.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-hybris-bootstrap-node)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-hybris-bootstrap-node/badges/master/build.svg)](https://gitlab.com/lean-delivery/ansible-role-hybris-bootstrap-node/pipelines)
[![Galaxy](https://img.shields.io/badge/galaxy-lean__delivery.hybris__bootstrap__node-blue.svg)](https://galaxy.ansible.com/lean_delivery/hybris_bootstrap_node)
![Ansible](https://img.shields.io/ansible/role/d/30317.svg)
![Ansible](https://img.shields.io/badge/dynamic/json.svg?label=min_ansible_version&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2F30317%2F&query=$.min_ansible_version)

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
  - `htools_username` - hybris user name
	  default: `hybris`
	- `htools_groupname` - hybris group name
	  default: `hybris`
	- `htools_hybris_path` - hybris root directory
	  default: `/opt/hybris`
	- `htools_platform_path` - hybris platform directory
	  default: `{{ htools_hybris_path }}/bin/platform`
	- `htools_hybris_tools_path` - hybris preinstall directory
	  default: `/opt/hybris-tools`
	- `htools_download_dir` - directory used to download hybris artifacts
	  default: `/tmp`
	- `htools_download_mask` - hybris artifacts mask
	  default: `hybris*.zip`
	- `htools_upload_dir` - directory to unpack hybris artifacts
	  default: `/opt`
	- `htools_remove_packages` - option to remove artifacts after unpack
	  default: `true`
	- `htools_purge_hybris_home` - to clean hybris root directory before artifacts unpack
	  default: `false`
	- `htools_legacy_structure` - option to set what git structure used <TODO: add info>
	  default: `false`
	- `htools_env_type` - set env type (e.g. QA, DEV, PERF, PROD)
	  default: ``
	- `htools_server_type` - hybris node type (e.g. be, backend, fe, frontend, batch, front, etc)
	  default: ``
	- `htools_artifact_storage` - artifact storage url. Http only supported. S3 is under testing. To use s3 use `s3://bucket/folder`
	  default: `http://artifactory.example.com/download`
	- `htools_tomcat_wrapper` - use custom tomcat wrapper in tomcat dir
	  default: ``
	- `htools_initialization_error_strings` - pattern to consider as error in hybris preparation output log (e.g. during DB initialization or update).
	  default: `ERROR(.*)`
	- `htools_initialization_log_check` - to enable error check
	  default: `true`
	- `htools_keep_log_dir` - keep log files directory
	  default: `false`

  - `hybris_env_path` - hybris profile variables file path
	  default: `/etc/profile.d/hybris_path.sh`
	- `hybris_env_ant_opts` - ant options
	  default: `-Xmx512m -Dfile.encoding=UTF-8`
	- `hybris_env_platform_home` - path to hybris platform directory
	  default: `{{ htools_platform_path }}`
	- `hybris_env_ant_home` - ant home directory
	  default: `{{ htools_platform_path }}/apache-ant-1.9.1`; for hybris 1808.1 `{{ htools_platform_path }}/apache-ant`
	- `hybris_env_custom_ant_targets_path` - start all ant targets from custom directory (used custom buildscripts module)
	  default: `{{ htools_hybris_path }}/bin/custom/buildscripts/resources/buildscripts/ant`

  - `hybrisd_service_username` - user name for hybrisd service
	  default: `hybris`
	- `hybrisd_service_groupname` - group name for hybrisd service
	  default: `hybris`
	- `hybrisd_service_platform_home` - path to hybris platform directory
	  default: `{{ htools_platform_path }}`
	- `hybrisd_service_path` - path to service file
	  default: `/etc/systemd/system/hybrisd.service`
	- `hybrisd_service_pid` - path to service pid file
	  default: `{{ htools_platform_path }}/tomcat/bin/hybrisPlatform.pid`
	- `hybrisd_service_timeout` - timeout in seconds for service start
	  default: `900`
	- `hybrisd_service_restart` - to restart service by condition
	  default: `always`
	- `hybrisd_service_exec` - executable file for hybrisd service
	  default: `hybrisserver.sh`
  - `hybris_selinux_ports` - ports to add to selinux exception
    default: `9001,9002`
  - `set_localaddr` - add localaddr to hosts file
    default: `true`

Example Playbook
----------------

```yaml
- name: Prepare example
  hosts: all
  roles:
    - role: lean_delivery.java
      java_major_version: 8
      java_minor_version: 181
      java_arch: "x64"
      java_package: "jdk"
    - role: lean_delivery.hybris_bootstrap_node
```

License
-------

Apache

Author Information
------------------

authors:
  - Lean Delivery Team <team@lean-delivery.com>
