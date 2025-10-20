# Network Configuration Management

[![ci](https://github.com/trevorlauder/ansible-example-home-network/actions/workflows/ci.yml/badge.svg)](https://github.com/trevorlauder/ansible-example-home-network/actions/workflows/ci.yml)

[![Dependabot Updates](https://github.com/trevorlauder/ansible-example-home-network/actions/workflows/dependabot/dependabot-updates/badge.svg)](https://github.com/trevorlauder/ansible-example-home-network/actions/workflows/dependabot/dependabot-updates)

This example uses Ansible to generate network configuration files from structured YAML data.

## Overview

This example automates the generation of network configuration files. It provides:

- **DHCP Configuration** (`dhcpd.conf`)
- **Host File** (`hosts`)
- **Firewall Rules** (`nftables`)

The configurations are generated from YAML-based infrastructure-as-code definitions, allowing you to manage your network declaratively.

## Structure

```
├── main.yaml                        # Main Ansible playbook
├── host_vars/                       # Ansible host_vars
│   ├── firewall.yaml                # Firewall configuration
│   ├── devices/                     # Device definitions by domain
│   │   ├── dmz.example.local.yaml
│   │   └── home.example.local.yaml
│   └── subnets/                     # Subnet definitions by domain
│       ├── dmz.example.local.yaml
│       └── home.example.local.yaml
├── schemas/                         # JSON Schema definitions for validation
│   ├── devices.yaml
│   ├── firewall.yaml
│   └── subnets.yaml
├── templates/                       # Jinja2 templates for config generation
│   ├── dhcpd.conf.j2
│   ├── hosts.j2
│   └── nftables.j2
└── dist/                            # Output directory for generated configs
    ├── dhcpd.conf
    ├── hosts
    └── nftables
```

## Prerequisites

- **Ansible** (see [Ansible Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/index.html))


### Generate Configuration Files

Run the main playbook to generate all configuration files:

```bash
ansible-playbook main.yaml
```

This will:
1. Load firewall configuration from `host_vars/firewall.yaml`
2. Load subnet configurations for all domains
3. Load device configurations for all domains
4. Combine variables into dictionaries
5. Generate output files in the `dist/` directory:
   - `dist/dhcpd.conf`
   - `dist/hosts`
   - `dist/nftables`

### nftables

The `nftables` file can be applied on your firewall with `nft -f dist/nftables`
