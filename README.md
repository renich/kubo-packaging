# Kubo RPM Packaging for Fedora and CentOS Stream

[![Copr build status](https://copr.fedorainfracloud.org/coprs/renich/kubo/package/kubo/status_image/last_build.png)](https://copr.fedorainfracloud.org/coprs/renich/kubo/package/kubo/)

RPM packaging specifications, vendor archives, systemd services, shell autocompletions, and automated Copr workflows for [Kubo](https://github.com/ipfs/kubo), the reference Go implementation of the InterPlanetary File System (IPFS).

IPFS is a peer-to-peer hypermedia distribution protocol designed to make the web faster, safer, and more open. Kubo provides the core command-line interface, local content-addressed storage repository, HTTP gateway, and RPC daemon.

## Highlights and Features

* **Dual Systemd Services**:
  * **System Unit** (`ipfs.service`/`kubo.service`): Dedicated unprivileged `ipfs:ipfs` service user provisioned via systemd-sysusers with `IPFS_PATH=/var/lib/ipfs`.
  * **Rootless User Unit** (`ipfs.service`/`kubo.service`): Per-user daemon running under systemd user manager with `IPFS_PATH=%h/.ipfs`.
* **Dynamic Shell Autocompletions**: Native completions dynamically generated at build time for Bash, Fish, and Zsh for both `ipfs` and `kubo` command aliases.
* **Man Page Documentation**: Built-in man pages (`ipfs(1)` and `kubo(1)`).
* **Transparent Compatibility Aliases**: Virtual `Provides: ipfs` and `Provides: go-ipfs` with symlinks across binaries, services, man pages, and completion scripts so commands work interchangeably.

## Target Distributions

The Copr repository provides automated builds for:

* **Fedora Rawhide** (`x86_64`, `aarch64`)
* **Fedora 45** (`x86_64`, `aarch64`)
* **Fedora 44** (`x86_64`, `aarch64`)
* **Fedora 43** (`x86_64`, `aarch64`)
* **CentOS Stream 10** (`x86_64`, `aarch64`)

## Installation Instructions

### Enable Copr Repository

```bash
sudo dnf copr enable renich/kubo
```

### Install Kubo

```bash
sudo dnf install kubo
```

The package provides `ipfs` and `go-ipfs` compatibility aliases. Binaries, systemd units, man pages, and shell completions work interchangeably under both `ipfs` and `kubo`.

## Running the Daemon

### 1. Per-User Daemon: Desktop and Development Environments

The per-user daemon runs rootless under the systemd user manager with storage rooted at `~/.ipfs`:

```bash
# Initialize local repository on first run
ipfs init

# Enable and start the systemd user service
systemctl --user enable --now ipfs.service

# Inspect service status and live logs
systemctl --user status ipfs.service
journalctl --user -u ipfs.service -f
```

### 2. System-Wide Service: Dedicated Servers and Gateways

The system-wide daemon runs under an unprivileged `ipfs:ipfs` service user provisioned via systemd-sysusers with storage rooted at `/var/lib/ipfs`:

```bash
# Enable and start the system service (initializes repository automatically if absent)
sudo systemctl enable --now ipfs.service

# Inspect service status and live logs
sudo systemctl status ipfs.service
sudo journalctl -u ipfs.service -f
```

## Shell Autocompletions

Native completions for Bash, Fish, and Zsh are installed system-wide:

* **Bash**: `/usr/share/bash-completion/completions/{ipfs,kubo}`
* **Fish**: `/usr/share/fish/vendor_completions.d/{ipfs,kubo}.fish`
* **Zsh**: `/usr/share/zsh/site-functions/{_ipfs,_kubo}`

Completions load automatically in new shell sessions.

## Firewall Configuration

If hosting a public gateway or peering across external networks:

```bash
# Swarm listening port (p2p transport)
sudo firewall-cmd --permanent --add-port=4001/tcp
sudo firewall-cmd --permanent --add-port=4001/udp

# HTTP Gateway (optional, default bind is localhost:8080)
# sudo firewall-cmd --permanent --add-port=8080/tcp

sudo firewall-cmd --reload
```

## Web UI and Verification

Once the daemon is active, access the bundled Web UI:

* **Local Web UI Dashboard**: http://127.0.0.1:5001/webui
* **Gateway Endpoint**: http://127.0.0.1:8080/ipfs/<CID>

Basic verification commands:

```bash
# Verify version and connected node identity
ipfs version
ipfs id

# Add and inspect sample content
echo "Hello IPFS from Fedora Copr" > hello.txt
ipfs add hello.txt
ipfs cat <CID>
```

## Upstream and Packaging Repositories

* Upstream Kubo: https://github.com/ipfs/kubo
* IPFS Documentation: https://docs.ipfs.tech
* Packaging Repository (GitLab): https://gitlab.com/renich/kubo-packaging
* Packaging Repository (GitHub mirror): https://github.com/renich/kubo-packaging
