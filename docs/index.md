---
hero:
  eyebrow: NETWORK DAEMON
  title: cloud-netconfig
  lead: >-
    Automatic network configuration for cloud instances using provider
    metadata — secondary IPs, routing tables, and policy-based routing
    on multi-interface VMs.
  swatches:
    - {label: "Azure"}
    - {label: "AWS EC2"}
    - {label: "GCP"}
    - {label: "Apache-2.0"}
  highlights:
    - {value: "3", label: "Cloud metadata providers supported — Azure, AWS EC2, GCP", footnote: "1"}
    - {value: "0.3.1", label: "Current release"}
    - {value: "5209", label: "Default local HTTP API port", footnote: "2"}
    - {value: "1", label: "Linux capability required to run unprivileged — CAP_NET_ADMIN", footnote: "3"}
footnotes:
  - {marker: "1", text: "Multi-cloud metadata clients: Azure, AWS EC2, GCP, and more.", href: "https://github.com/hypersdk/cloud-netconfig#features", href_label: "See README — Features."}
  - {marker: "2", text: "The local HTTP API binds to 127.0.0.1:5209 by default (server.listen in the config file).", href: "user/configuration.md", href_label: "See Configuration."}
  - {marker: "3", text: "The daemon starts as root, creates state directories, then drops to the cloud-network system user while retaining only CAP_NET_ADMIN.", href: "user/admin-basics.md", href_label: "See Admin basics — Privileges."}
---

Automatic network configuration for cloud instances using provider metadata (Azure, AWS, GCP, and others). Handles secondary IPs, routing tables, and policy-based routing on multi-interface VMs.

## Features

<div class="icon-badge-list" markdown="1">

- ☁️ Multi-cloud metadata clients (Azure, AWS EC2, GCP, and more)
- 🔀 Event-driven reconfiguration via netlink
- 🛣️ Policy-based routing for multi-homed hosts
- 🔌 Local HTTP API for instance metadata
- 🔒 Runs unprivileged with `CAP_NET_ADMIN`

</div>

## Installation

```bash
git clone https://github.com/hypersdk/cloud-netconfig.git
cd cloud-netconfig
make build
sudo make install
sudo useradd -M -s /usr/bin/nologin cloud-network 2>/dev/null || true
sudo systemctl enable --now cloud-netconfigd
```

## Configuration

Default path: `/etc/cloud-network/cloud-network.yaml`

```yaml
logging:
  level: info
  format: text

server:
  listen:
    address: 127.0.0.1
    port: 5209

metadata:
  refresh_interval: 300s
  request_timeout: 10s

network:
  interfaces:
    enabled:
      - eth1
      - eth2
  routing:
    table_base: 9999
    policy_routing: true
```

Annotated reference: `distribution/etc/cloud-network/config.yaml` in the repository.

## CLI

```bash
cnctl status system
cnctl show interfaces
```

## Troubleshooting

```bash
sudo journalctl -u cloud-netconfigd -f
cnctl status system
```

Enable debug logging in the config file (`logging.level: debug`) when diagnosing metadata or routing issues.

## Enterprise

Community Edition is the open-source daemon. Supported multi-cloud rollouts, SLAs, and HyperSDK migration integration are handled by Zyvor, not GitHub Issues. See [Enterprise](enterprise.md).

## Support the project

cloud-netconfig Community Edition is free and open source, maintained by **Susant Sahani** · [Zyvor AI Labs](https://zyvor.dev?utm_source=github&utm_medium=cloud-netconfig).

- **Enterprise / production:** [zyvor.dev/contact](https://zyvor.dev/contact?utm_source=github&utm_medium=cloud-netconfig) · [sales@zyvor.dev](mailto:sales@zyvor.dev)
- **Community help:** [GitHub Issues](https://github.com/hypersdk/cloud-netconfig/issues)

## License

This repository is licensed under the Apache License, Version 2.0. You may use, modify, and run it for personal, lab, and commercial production use at no charge, subject to Apache-2.0 (preserve notices / NOTICE where required). Production support, SLAs, and Zyvor Enterprise products are licensed separately — contact [sales@zyvor.dev](mailto:sales@zyvor.dev) or see [zyvor.dev](https://zyvor.dev).
