# keepalived

[![Source Code](https://img.shields.io/badge/github-source%20code-blue?logo=github&logoColor=white)](https://github.com/rolehippie/keepalived)
[![General Workflow](https://github.com/rolehippie/keepalived/actions/workflows/general.yml/badge.svg)](https://github.com/rolehippie/keepalived/actions/workflows/general.yml)
[![Readme Workflow](https://github.com/rolehippie/keepalived/actions/workflows/docs.yml/badge.svg)](https://github.com/rolehippie/keepalived/actions/workflows/docs.yml)
[![Galaxy Workflow](https://github.com/rolehippie/keepalived/actions/workflows/galaxy.yml/badge.svg)](https://github.com/rolehippie/keepalived/actions/workflows/galaxy.yml)
[![License: Apache-2.0](https://img.shields.io/github/license/rolehippie/keepalived)](https://github.com/rolehippie/keepalived/blob/master/LICENSE)
[![Ansible Role](https://img.shields.io/badge/role-rolehippie.keepalived-blue)](https://galaxy.ansible.com/rolehippie/keepalived)

Ansible role to configure failover IPs via Keepalived.

## Sponsor

Building and improving this Ansible role have been sponsored by my current and previous employers like **[Cloudpunks GmbH](https://cloudpunks.de)** and **[Proact Deutschland GmbH](https://www.proact.eu)**.

## Table of content

- [Requirements](#requirements)
- [Default Variables](#default-variables)
  - [keepalived_exporter_args](#keepalived_exporter_args)
  - [keepalived_exporter_download](#keepalived_exporter_download)
  - [keepalived_exporter_enabled](#keepalived_exporter_enabled)
  - [keepalived_exporter_listen_address](#keepalived_exporter_listen_address)
  - [keepalived_exporter_read_json](#keepalived_exporter_read_json)
  - [keepalived_exporter_telemetry_path](#keepalived_exporter_telemetry_path)
  - [keepalived_exporter_version](#keepalived_exporter_version)
  - [keepalived_instances](#keepalived_instances)
  - [keepalived_script_group](#keepalived_script_group)
  - [keepalived_script_shell](#keepalived_script_shell)
  - [keepalived_script_user](#keepalived_script_user)
- [Discovered Tags](#discovered-tags)
- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## Requirements

- Minimum Ansible version: `2.10`

## Default Variables

### keepalived_exporter_args

Optional list of additional arguments for the keepalived exporter

#### Default value

```YAML
keepalived_exporter_args: []
```

### keepalived_exporter_download

URL to the keepalived exporter to install

#### Default value

```YAML
keepalived_exporter_download: 
  https://github.com/gen2brain/keepalived_exporter/releases/download/v{{ 
  keepalived_exporter_version }}/keepalived_exporter-{{ 
  keepalived_exporter_version }}-amd64.tar.gz
```

### keepalived_exporter_enabled

Enable the installation of the keepalived exporter

#### Default value

```YAML
keepalived_exporter_enabled: true
```

### keepalived_exporter_listen_address

Address to bind the exporter to

#### Default value

```YAML
keepalived_exporter_listen_address: 0.0.0.0:9650
```

### keepalived_exporter_read_json

Send SIGJSON and decode JSON file instead of parsing text files

#### Default value

```YAML
keepalived_exporter_read_json: true
```

### keepalived_exporter_telemetry_path

Path to serve the metrics from

#### Default value

```YAML
keepalived_exporter_telemetry_path:
```

### keepalived_exporter_version

Version of the keepalived exporter to install

#### Default value

```YAML
keepalived_exporter_version: 0.7.1
```

### keepalived_instances

Definitions for floating IPs

#### Default value

```YAML
keepalived_instances: []
```

#### Example usage

```YAML
keepalived_instances:
  - name: haproxy
    interface: ens224
    router: 228
    password: p455w0rd
    script: /usr/bin/killall -0 haproxy
    address:
      - 192.168.1.17/28
    routes:
      - 0.0.0.0/1 via 213.32.231.129 dev ens224
      - 128.0.0.0/1 via 213.32.231.129 dev ens224
    interfaces:
      - ens224
    peers:
      - 192.168.1.18
      - 192.168.1.19
    state:
      haproxy-01: MASTER
      haproxy-02: BACKUP
    priority:
      haproxy-01: 99
      haproxy-02: 98
    notify:
      master: |
        /usr/sbin/route add default gw 192.168.1.1 || true
      backup: |
        /usr/sbin/route delete default gw 192.168.1.1 || true
```

### keepalived_script_group

Group for running scripts

#### Default value

```YAML
keepalived_script_group: '{{ keepalived_script_user }}'
```

### keepalived_script_shell

Shell for the script user

#### Default value

```YAML
keepalived_script_shell: /usr/sbin/nologin
```

### keepalived_script_user

User for running scripts

#### Default value

```YAML
keepalived_script_user: keepalive_script
```

## Discovered Tags

**_keepalived_**

**_keepalived-exporter_**

## Dependencies

- None

## License

Apache-2.0

## Author

[Thomas Boerger](https://github.com/tboerger)
