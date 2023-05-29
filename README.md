[TOC]

Role: etcd
=========

This role is intended to deploy `etcd cluster` by install from package manager only.

Copyright (C) 2023  Mikhail Shurutov

This program is free software; you can redistribute it and/or
modify it under the terms of the GNU General Public License
as published by the Free Software Foundation; either version 2
of the License, or any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, write to the Free Software
Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA.

Requirements
------------

This role is not depend on other role.

Role Variables
--------------

There are required variables.

* `etcd_templates_dir` (default value is `{{ role_path }}/templates`): this variable defines directory where role looks for templates.
* `etcd_conf_dir` (default value is `/etc/etcd`): this variable define directory where config file is pushed to. This directory should be absolute path. This variable depend on distributive:
    * on RedHat-base/like it is `/etc/sysconfig`;
    * on Debian-base/like it is `/etc/default`;
    * on Gentoo it is `/etc/conf.d`;
* `etcd_conf_file` (default value is `{{ etcd_conf_dir }}/etcd.conf`): this file (see `templates/etcd.conf.j2`) contains shell variables for etcd service.
* `etcd_data_dir` (default value is `/var/lib/etcd`): this variable define storage directory.
* `etcd_client_port` (default value is `2379`): this variable defines port for client connections.
* `etcd_peer_port` (default value is `2380`): this variable defines port for peer (cluster members) connections.
* `etcd_cluster_group` (default value is `etcd_cluster`): this variable defines host group.
* `etcd_initial_cluster_state` (default value is `new`)
* `etcd_heartbeat_interval` (default value is `100`)
* `etcd_election_timeout` (default value is `1000`)
* `etcd_initial_cluster_token` (default value is `etcd_cluster`)

Last four variables is getting from any documentation and default configs from any distros.

Other variables is defined in `{{ role_path }}/templates/etcd.conf.j2`.

If you use RedHat-based distribution you should define follow variables:
* `etcd_dnf_repo_uri`
* `etcd_dnf_repo_description`
* `etcd_dnf_repo_name`
* `etcd_dnf_repo_gpgcheck`
 

Dependenciee
------------

This role is dependency for mshurutov.postgres role.

Example Playbook
----------------

A part of inventory:
```
...
[etcd_group01]
etcdhost01
etcdhost02
etcdhost03
...
[etcd_groups:children]
etcd_group01
```
A part of one from any templates (for group `etcd_group01`):
```
...
etcd_cluster_group: etcd_group01
...
```
So part of playbook (for all etcd clusters):
```
...
- hosts: etcd_groups
  roles:
    - role: etcd
...
```

License
-------

[GPLv2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.txt)

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
