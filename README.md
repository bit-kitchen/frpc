ansible-role-frp
================

[![Ansible Role: bit_kitchen.frpc](https://img.shields.io/ansible/role/47388.svg)](https://galaxy.ansible.com/bit_kitchen/frpc)
[![Build Status: bit-kitchen/frpc](https://travis-ci.org/bit-kitchen/frpc.svg?branch=master)](https://travis-ci.org/bit-kitchen/frpc)

[![Ansible Role: bit_kitchen.frps](https://img.shields.io/ansible/role/47389.svg)](https://galaxy.ansible.com/bit_kitchen/frps)
[![Build Status: bit-kitchen/frps](https://travis-ci.org/bit-kitchen/frps.svg?branch=master)](https://travis-ci.org/bit-kitchen/frps)

Install and configure [frp](https://github.com/fatedier/frp) client or server on Windows, Linux or OpenWrt.

    ansible-galaxy install bit_kitchen.frpc
    ansible-galaxy install bit_kitchen.frps

Requirements
------------

None.

Role Variables
--------------

### frpc

Variable         | Required/Optional | Default     | Comment
--------         | ----------------- | -------     | -------
frpc_version     | Optional          | (undefined) | frp [release](https://github.com/fatedier/frp/releases) version. <br> Defaults to the latest version.
frpc_config_name | Optional          | (undefined) | Used for frpc config file name and frpc service name.
||
frpc_config_file | Optional          | (undefined) | Local config file to be copied to remote. <br> If this is specified, the following options are not considered for frpc config.
||
frpc_server_addr | Optional          | `127.0.0.1` | Server address for frpc.
frpc_server_port | Optional          | `7000`      | Server port for frpc.
frpc_token       | Optional          | (undefined) | frp token used for authentication if specified on server.
frpc_http_proxy  | Optional          | (undeinfed) | The proxy to use for connections to frp server. <br> This option is also used as proxy for other network related operations.


### frps

Variable         | Required/Optional | Default     | Comment
--------         | ----------------- | -------     | -------
frps_version     | Optional          | (undefined) | frp [release](https://github.com/fatedier/frp/releases) version. <br> Defaults to the latest version.
frps_config_name | Optional          | (undefined) | Used for frps config file name and frps service name.
||
frps_config_file | Optional          | (undefined) | Local config file to be copied to remote. <br> If this is specified, the following options are not considered for frps config.
||
frps_bind_addr   | Optional          | `0.0.0.0`   | Bind address for frps. Defaults to listen on all interfaces.
frps_bind_port   | Optional          | `7000`      | Bind port for frps.
frps_token       | Optional          | (undefined) | frp token used for authentication.



Dependencies
------------

* `bit_kitchen.nssm`: (Required for Windows only) used for service creation on Windows.
* `gekmihesg.openwrt`: (Required for OpenWrt only) used to support OpenWrt without Python.
* `kewlfft.aur`: (Required for Arch Linux only) used for AUR support.

Example Playbook
----------------

### Install and configure frp using defaults

```yml
- hosts: servers
  roles:
  - bit_kitchen.frps

- hosts: clients
  roles:
  - bit_kitchen.frpc
```

### Install and configure frp using existing config files

```yml
- hosts: servers
  roles:
  - role: bit_kitchen.frps
    frps_config_file: /path/to/my/frps.ini

- hosts: clients
  roles:
  - role: bit_kitchen.frpc
    frpc_config_file: /path/to/my/frpc.ini
```

### Install and configure frp using variables

```yml
- hosts: servers
  roles:
  - role: bit_kitchen.frps
    frps_bind_port: 7000
    frps_token: MySecretToken

- hosts: clients
  roles:
  - role: bit_kitchen.frpc
    frpc_server_addr: frp.example.com
    frpc_token: MySecretToken
```

### Install and configure two instances of frp

```yml
- hosts: servers
  roles:
  - role: bit_kitchen.frps
    frps_config_name: server1
    frps_config_file: /path/to/my/frps-1.ini

  - role: bit_kitchen.frps
    frps_config_name: server2
    frps_config_file: /path/to/my/frps-2.ini

- hosts: clients
  roles:
  - role: bit_kitchen.frpc
    frpc_config_name: client1
    frpc_config_file: /path/to/my/frpc-1.ini

  - role: bit_kitchen.frpc
    frpc_config_name: client2
    frpc_config_file: /path/to/my/frpc-2.ini
```


License
-------

[MIT](LICENSE)

Author Information
------------------

[bit.kitchen](https://github.com/bit-kitchen)
