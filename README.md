# Prometheus Node Exporter Setup Guide

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Prometheus](https://img.shields.io/badge/Prometheus-Node%20Exporter-E6522C?logo=prometheus&logoColor=white)](https://github.com/prometheus/node_exporter)
[![Platform](https://img.shields.io/badge/Platform-Linux-blue)](#installation-linux)

A step-by-step guide to installing, running, and connecting **Prometheus Node Exporter** to a Prometheus server for host-level monitoring.

---

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation (Linux)](#installation-linux)
- [Verify the Installation](#verify-the-installation)
- [Connect to Prometheus](#connect-to-prometheus)
- [Configuration](#configuration)
- [Docker (Optional)](#docker-optional)
- [Windows](#windows)
- [Upgrade](#upgrade)
- [Uninstall](#uninstall)
- [Troubleshooting](#troubleshooting)
- [Official Resources](#official-resources)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

[Node Exporter](https://github.com/prometheus/node_exporter) is a Prometheus exporter for hardware and OS metrics exposed by \*NIX kernels. It runs on each host you want to monitor and exposes metrics over HTTP, which a Prometheus server scrapes on a schedule.

Typical metrics include:

- CPU usage and load average
- Memory and swap
- Disk space and disk I/O
- Network traffic
- Filesystem and system information

| Item | Value |
| --- | --- |
| Default port | `9100` |
| Metrics endpoint | `http://<host>:9100/metrics` |
| Supported systems | Linux and other \*NIX systems |

> **Note:** Node Exporter is a standalone Go binary. It is unrelated to Node.js and does not require Node.js to be installed.

## Prerequisites

- A Linux host with `systemd`
- `sudo` or root access
- Outbound internet access to download the release (or a way to copy the binary to the host)
- A running Prometheus server that can reach the host on port `9100`

---

## Installation (Linux)

> Check the [Prometheus downloads page](https://prometheus.io/download/#node_exporter) for the latest release and set `VERSION` accordingly. Use `arm64` instead of `amd64` on ARM hosts.

### 1. Download and install the binary

```bash
VERSION="1.12.0"
ARCH="amd64"

cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v${VERSION}/node_exporter-${VERSION}.linux-${ARCH}.tar.gz
tar xzf node_exporter-${VERSION}.linux-${ARCH}.tar.gz
sudo install -m 0755 node_exporter-${VERSION}.linux-${ARCH}/node_exporter /usr/local/bin/
```

### 2. Create a dedicated user

Run the service as an unprivileged user with no login shell:

```bash
sudo useradd --no-create-home --shell /usr/sbin/nologin node_exporter
```

### 3. Create the systemd service

Create `/etc/systemd/system/node_exporter.service`:

```ini
[Unit]
Description=Prometheus Node Exporter
After=network-online.target
Wants=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

### 4. Start and enable the service

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now node_exporter
systemctl status node_exporter
```

### 5. Clean up

```bash
rm -rf /tmp/node_exporter-${VERSION}.linux-${ARCH}*
```

---

## Verify the Installation

Confirm the service is running and serving metrics:

```bash
systemctl is-active node_exporter
curl -s http://localhost:9100/metrics | head
```

You should see lines such as `# HELP node_cpu_seconds_total ...` followed by metric values.

---

## Connect to Prometheus

On your Prometheus server, add a scrape job to `prometheus.yml`:

```yaml
scrape_configs:
  - job_name: "node"
    static_configs:
      - targets:
          - "<server-1-ip>:9100"
          - "<server-2-ip>:9100"
```

Reload or restart Prometheus, then open **Status → Targets** in the Prometheus UI and confirm each target shows as `UP`.

Quick check with a PromQL query:

```promql
up{job="node"}
```

> **Security:** Node Exporter exposes detailed system information and has no authentication by default. Restrict port `9100` in your firewall so that only the Prometheus server can reach it.
>
> Example with `ufw`:
>
> ```bash
> sudo ufw allow from <prometheus-ip> to any port 9100 proto tcp
> ```

---

## Configuration

Node Exporter is configured with command-line flags, set on the `ExecStart` line of the systemd unit. After any change, run:

```bash
sudo systemctl daemon-reload
sudo systemctl restart node_exporter
```

Common examples:

| Goal | Flag |
| --- | --- |
| Change the listen address or port | `--web.listen-address=":9100"` |
| Enable an optional collector | `--collector.<name>` (for example `--collector.systemd`) |
| Disable a default collector | `--no-collector.<name>` |
| Expose custom metrics from files | `--collector.textfile.directory=/var/lib/node_exporter/textfile` |

See all available options and collectors:

```bash
node_exporter --help
```

> Some collectors are disabled by default because of high cardinality or long run times, so enable extra collectors with care.

---

## Docker (Optional)

Node Exporter is designed to monitor the host, so a container needs extra flags to see the host rather than itself. A native install is simpler and more accurate. If you need a container:

```bash
docker run -d \
  --name node_exporter \
  --net="host" \
  --pid="host" \
  --restart unless-stopped \
  -v "/:/host:ro,rslave" \
  quay.io/prometheus/node-exporter:latest \
  --path.rootfs=/host
```

Docker Compose equivalent:

```yaml
services:
  node_exporter:
    image: quay.io/prometheus/node-exporter:latest
    container_name: node_exporter
    command:
      - "--path.rootfs=/host"
    network_mode: host
    pid: host
    restart: unless-stopped
    volumes:
      - "/:/host:ro,rslave"
```

---

## Windows

Node Exporter targets \*NIX systems. For Windows hosts, use [`windows_exporter`](https://github.com/prometheus-community/windows_exporter) instead:

1. Download the `.msi` installer from the releases page.
2. Run the installer and complete the wizard.
3. Add the host to your Prometheus scrape config (the default port is `9182`).

---

## Upgrade

```bash
VERSION="<new-version>"
ARCH="amd64"

cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v${VERSION}/node_exporter-${VERSION}.linux-${ARCH}.tar.gz
tar xzf node_exporter-${VERSION}.linux-${ARCH}.tar.gz

sudo systemctl stop node_exporter
sudo install -m 0755 node_exporter-${VERSION}.linux-${ARCH}/node_exporter /usr/local/bin/
sudo systemctl start node_exporter

node_exporter --version
```

## Uninstall

```bash
sudo systemctl disable --now node_exporter
sudo rm /etc/systemd/system/node_exporter.service
sudo systemctl daemon-reload
sudo rm /usr/local/bin/node_exporter
sudo userdel node_exporter
```

---

## Troubleshooting

<details>
<summary><strong>Target shows as DOWN in Prometheus</strong></summary>

- Confirm the service is running: `systemctl status node_exporter`
- Test locally: `curl http://localhost:9100/metrics`
- Test from the Prometheus server: `curl http://<host>:9100/metrics`
- Check firewall and cloud security-group rules for port `9100`
- Verify the target address in `prometheus.yml`

</details>

<details>
<summary><strong>Service fails to start</strong></summary>

View the logs:

```bash
journalctl -u node_exporter -e --no-pager
```

Common causes: wrong binary path in `ExecStart`, a missing `node_exporter` user, or a typo in a flag.

</details>

<details>
<summary><strong>Port 9100 already in use</strong></summary>

Find the process using the port:

```bash
sudo ss -tulpn | grep 9100
```

Either stop that process or change the port with `--web.listen-address`.

</details>

<details>
<summary><strong>"Permission denied" or exec format error</strong></summary>

- Make sure the binary is executable: `sudo chmod +x /usr/local/bin/node_exporter`
- Make sure you downloaded the right architecture (`uname -m`): `amd64` for x86_64, `arm64` for aarch64
- On SELinux systems, restore the file context: `sudo restorecon -v /usr/local/bin/node_exporter`

</details>

---

## Official Resources

| Resource | Link |
| --- | --- |
| Node Exporter (GitHub) | https://github.com/prometheus/node_exporter |
| Prometheus Downloads | https://prometheus.io/download/ |
| Prometheus Documentation | https://prometheus.io/docs/ |
| Node Exporter Guide | https://prometheus.io/docs/guides/node-exporter/ |
| Windows Exporter | https://github.com/prometheus-community/windows_exporter |

---

## Contributing

Contributions and suggestions are welcome! To improve this guide:

1. Fork this repository
2. Create a new branch
3. Make your changes
4. Submit a Pull Request

Please keep instructions in line with the official Node Exporter documentation.

## License

This project is licensed under the [MIT License](LICENSE).
