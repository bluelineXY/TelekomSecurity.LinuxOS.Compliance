-------------------------------------------------------------------------------

# Telekom Security Linux OS Compliance			                         

Company: [T-systems International GmbH](https://www.t-systems.com)

Author : [Markus Schumburg](mailto://security.automation@telekom.de)

Version: 0.9

Date   : 24. May 2018

-------------------------------------------------------------------------------

## Description

This Ansible role can be used to perform compliance checks on servers with
Linux OS based on security requirements from Telekom Security (see 'References'
for used document).

-------------------------------------------------------------------------------

**IMPORTANT:** The Ansible role for compliance checks is non-intrusive. This
means no changes will be made on systems. The scripts include shell commands to 
perform checks on files, system parameters etc. The playbook must run in Ansible 
check mode ("dry run"). See `Execute` for more information.

-------------------------------------------------------------------------------

Note! It is an corresponding Ansible role available to automatically deploy
      security configuration based on security requirements from Telekom
      Security for Linux Server OS.

## Platforms

The role is tested with the following Linux versions:

   * Ubuntu 14.04 LTS
   * Ubuntu 16.04 LTS
   * RedHat Enterprise Linux 7.x
   * CentOS 7.x

-------------------------------------------------------------------------------

**IMPORTANT:** This role only supports Linux versions for SERVERS only! The role
is not tested with desktop systems and can cause unexpected malfunctions.

-------------------------------------------------------------------------------

## Structure

The role **TelekomSecurity.LinuxOS.Compliance** includes the following
directories and files:

Directory `defaults` includes the following files for default variables:

* /defauls/main.yml

Directory `meta` includes the following files with meta data for Ansible Galaxy:

* /meta/main.yml

Directory `tasks` includes the following files with tasks of the playbook:

* /tasks/main.yml
* /tasks/compliance_01_system_hardening.yml
* /tasks/compliance_02_system_update.yml
* /tasks/compliance_03_protecting_data.yml
* /tasks/compliance_04_authentication_authorization.yml
* /tasks/compliance_05_logging.yml
* /tasks/compliance_06_system_management.yml

Directory `vars` includes the following files for specific variables:

* /vars/main.yml         
* /vars/Ubuntu.yml

## Inventory

Ansible by default uses the '/etc/ansible/hosts' file to specify hosts and
needed parameters. To use your own 'hosts' file use

```
$ ansible-playbook -i <location-of-host-file> <playbook>.yml --check
```

This is fine for testing purposes. In case of productive environments
a dynamic inventory triggered by the used orchestration should be
used. For more information see:

* http://docs.ansible.com/ansible/latest/intro_inventory.html
* http://docs.ansible.com/ansible/latest/intro_dynamic_inventory.html

## Variables

The variable files of roles for hardening and compliance checks for Linux
OS for servers from Telekom Security are identically. This allows to use
the files from hardening for later compliance checks.

file: `/defaults/main.yml`

This files includes variables that must be specified for any system before
hardening is done. The following variables are mandatory and the necessary
values must be defined by the user of the role:

Variables for default user:

        - config_new_user: enables (true) or disables (false) if a new user is
          configured on system
        - use_user_name: username
        - use_password: password for user
        - use_groups: groups for user (default: sudo,ssh)
        - use_key_file: path to public key for user

-------------------------------------------------------------------------------

**IMPORTANT:** The role will disable SSH remote login with password and root
login. Access to system is not longer possible if no user with stored public key
and group membership to `sudo` and ssh exists on the system!

-------------------------------------------------------------------------------

Variable to enable/disable IPv6:

        - config_ipv6_enable: enables (true) or disables (false) the use of IPv6
          protocol.

Variables for management interface:

        - config_mgmt_interface_ipv4: enables (true) or disables (false) if a
          dedicated mgmt interface is used (ipv4)
        - use_mgmt_interface_ipv4: IPv4 address of management interface
        - config_mgmt_interface_ipv6: enables (true) or disables (false) if a
          dedicated mgmt interface is used (ipv6)
        - use_mgmt_interface_ipv6: IPv6 address of management interface

Variables for partitions:

        - config_partition_tmp: enables (true) or disables (false) if  partition
          exists for /tmp
        - config_partition_var: enables (true) or disables (false) if partition
          exists for /var
        - config_partition_var_tmp: enables (true) or disables (false) if
          partition exists for /var/tmp
        - config_partition_var_log: enables (true) or disables (false) if
          partition exists for /var/log
-------------------------------------------------------------------------------

**IMPORTANT:** Partitions must be generated during OS installation. This task
could not be done during automated hardening!

-------------------------------------------------------------------------------

Variable for system upgrade:

          - do_system_upgrade: enables (true) or disables (false) if system
            upgrade should be performed during hardening

-------------------------------------------------------------------------------

**IMPORTANT:** If a system upgrade should be performed during hardening an
connection to a local repository or to Internet must exits!

-------------------------------------------------------------------------------

Variable for logging solution:

        - config_central_logging: enables (true) or disables (false) the use of a
          local syslog server for logging
        - use_syslog_type: definition of syslog solution. Note! in current
          version only rsyslog is supported!
        - use_logging_server: IPv4/v6 address of syslog server

Variable for NTP solution:

        - config_ntp: enables (true) or disables (false) if  NTP should be used
          for time sync
        - use_ntp_servers: IP (4/6) address or domain name of NTP server
        - set_timezone: enables (true) or disables (false) if timezone should
          be set
        - use_timezone: timezone to set on system

file: `/vars/main.yml`

This file includes the main variables based on security requirements of
Deutsche Telekom AG. Changes to variables in this file must be verified
and approved by a security expert. Any changes to these variables can  
result in a system configuration that is not compliant to DTs security
requirements!

file: `/vars/Ubuntu.yml`

This files include specific variables that are dependent to the used Linux
distribution and version. Any changes to these variables can result in a
system configuration that is not compliant to DTs security requirements!


## Execute

Ansible is agent-free. This means no agent is needed on systems that
should be configured with Ansible. But Ansible uses python. Python
must be installed on systems to be managed with Ansible!

Ansible uses SSH to connect to remote systems. To connect and to perform
all tasks a user is needed on the system that should be hardened.
This user needs root rights and must be a member of sudo group. Needed
parameters for the user can be defined in inventory or playbook file.

Check for user parameters:
http://docs.ansible.com/ansible/latest/intro_inventory.html

The default path to store roles is `/etc/ansible/roles`. In the file
`/etc/ansible/ansible.cfg` with variable `roles_path` an own path can
be specified.

Start playbook with:

```
$ ansible-playbook playbook.yml --check
```

Paramter '--check' is important. As this role only checks compliance it
must run in check mode ("dry run"). Otherwise the playbook will stop with
an error.


## References


Telekom Security - Security Requirements:
* SecReq 3.xx: Linux OS for Servers


## License

Copyright 2018, T-Systems International GmbH

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
