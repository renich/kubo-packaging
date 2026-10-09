# Kubo (IPFS) RPM Packaging for Fedora & CentOS Stream

[![Copr build status](https://copr.fedorainfracloud.org/coprs/renich/kubo/package/kubo/status_image/last_build.png)](https://copr.fedorainfracloud.org/coprs/renich/kubo/package/kubo/)

RPM packaging specifications, vendor archives, systemd services, shell autocompletions, and automated Copr workflows for [Kubo](https://github.com/ipfs/kubo) (formerly `go-ipfs`), the reference Go implementation of the InterPlanetary File System (IPFS).

IPFS is a peer-to-peer hypermedia distribution protocol designed to make the web faster, safer, and more open. Kubo provides the core command-line interface, local content-addressed storage repository, HTTP gateway, and RPC daemon.

## Highlights & Features

* **Dual Systemd Services**:
  * **System Unit** (`ipfs.service` / `kubo.service`): Dedicated unprivileged `ipfs:ipfs` service user provisioned via systemd-sysusers with `IPFS_PATH=/var/lib/ipfs`.
  * **Rootless User Unit** (`ipfs.service` / `kubo.service`): Per-user daemon running under systemd user manager with `IPFS_PATH=%h/.ipfs`.
* **Dynamic Shell Autocompletions**: Native completions dynamically generated at build time for Bash, Fish, and Zsh for both `ipfs` and `kubo` command aliases.
* **Man Page Documentation**: Built-in man pages (`ipfs(1)` and `kubo(1)`).
* **Transparent Aliases & Compatibility**: Virtual `Provides: ipfs` and `Provides: go-ipfs` with symlinks across binaries, services, man pages, and completion scripts so commands work interchangeably.

## Target Distributions

The Copr repository provides automated builds for:

* **Fedora Rawhide** (`x86_64`, `aarch64`)
* **Fedora 45** (`x86_64`, `aarch64`)
* **Fedora 44** (`x86_64`, `aarch64`)
* **Fedora 43** (`x86_64`, `aarch64`)
* **CentOS Stream 10** (`x86_64`, `aarch64`)

## Installation

Enable the Copr repository and install the package using DNF:

```bash
sudo dnf copr enable renich/kubo
sudo dnf install kubo
```

## Running the Daemon

### 1. Per-User Service (Recommended for desktop & dev machines)

Initialize and enable the rootless user daemon:

```bash
# Initialize repo if running for the first time
ipfs init

# Enable and start the systemd user service
systemctl --user enable --now ipfs.service

# Check service status
systemctl --user status ipfs.service
```

### 2. System-Wide Service (Recommended for servers & gateways)

Enable and start the dedicated system-wide daemon:

```bash
# Enable and start the system service (automatically initializes repo on first run)
sudo systemctl enable --now ipfs.service

# Check system service status
sudo systemctl status ipfs.service
```

## CLI Quick Start

```bash
# Check version
ipfs version

# Add a file to IPFS
ipfs add example.txt

# Retrieve content by CID
ipfs cat <CID>
```
