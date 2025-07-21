# Lima Configurations

This repository contains a collection of [Lima](https://github.com/lima-vm/lima) (`limactl`) virtual machine configuration files. These YAML configs help you quickly spin up container-native Linux virtual machines with customized environments on macOS using Lima.

Each config defines a specific purpose — such as ISO building, development environments, or testing tools — and can be easily launched, managed, and shared.

---

## Table of Contents

- [Lima Configurations](#lima-configurations)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Initial Setup](#initial-setup)
  - [Daily Usage](#daily-usage)
  - [Management Commands](#management-commands)
  - [Working Directory \& Mounts](#working-directory--mounts)
    - [Example](#example)
  - [Troubleshooting](#troubleshooting)
  - [Example Workflow](#example-workflow)
  - [Config Descriptions](#config-descriptions)
    - [`iso-builder.yaml`](#iso-builderyaml)
  - [Resources](#resources)

---

## Overview

This repo provides reusable Lima VM configurations that:

- Use minimal cloud-init-like YAML files
- Mount a working directory from macOS for shared access
- Include provisioning scripts for system and user-level setup
- Target use cases like building ISOs, testing software, or creating reproducible dev environments

---

## Initial Setup

1. **Create a working directory** (optional, but useful):

```bash
mkdir -p ~/repos/iso-work  # Or whatever path your config mounts
````

2. **Start a Lima VM with one of the configs**:

```bash
limactl start ~/repos/limactl-configs/iso-builder.yaml
```

   Or name it explicitly:

```bash
limactl start --name=iso-builder ~/repos/limactl-configs/iso-builder.yaml
```

---

## Daily Usage

```bash
# Open a shell inside the VM
limactl shell iso-builder

# List running VMs
limactl list

# Show detailed info about a VM
limactl info iso-builder
```

---

## Management Commands

```bash
# Stop a VM (preserves state)
limactl stop iso-builder

# Restart it
limactl start iso-builder

# Delete a VM (irreversible)
limactl delete iso-builder
```

---

## Working Directory & Mounts

Most configs mount a directory from your macOS host into the VM. This allows seamless access to files between macOS and the Lima VM.

### Example

In your YAML config:

```yaml
mounts:
  - location: "~/repos/iso-work"
    writable: true
```

**Usage:**

* On macOS: work inside `~/repos/iso-work`
* Inside the VM: access the same files via `/mnt/lima-0`

This setup ensures changes made in either environment are synced immediately.

---

## Troubleshooting

```bash
# SSH config to connect manually or debug
limactl show-ssh iso-builder

# Force stop a stuck VM
limactl stop --force iso-builder

# View the Lima VM logs
limactl show-log iso-builder
```

---

## Example Workflow

```bash
# Start your environment
limactl start ~/repos/limactl-configs/iso-builder.yaml

# Access it
limactl shell iso-builder

# Inside the VM, work in your mounted directory
cd /mnt/lima-0

# Do your tasks (e.g., build ISOs, compile code)

# Exit and stop when finished
exit
limactl stop iso-builder
```

---

## Config Descriptions

Each config file is documented below.

### `iso-builder.yaml`

A Lima VM config for building ISO images. Includes tools like:

* `xorriso`
* `p7zip`
* `whois`
* `vim`

It mounts `~/repos/iso-work` from the host and provisions a minimal build environment.

More configurations will be added here as the collection grows.

---

## Resources

* Lima project: [https://github.com/lima-vm/lima](https://github.com/lima-vm/lima)
* Lima CLI reference: `limactl help`

---

