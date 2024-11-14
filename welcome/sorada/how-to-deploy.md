---
description: Deploy your own Sorada, today.
---

# How to Deploy

## How to Deploy

### System Requirements

Operating System

* Ubuntu Server 22.04 LTS

Hardware Requirements

* CPU: 2-core
* RAM: 8GB
* SSD: 16GB

Server Port Policy

* Open port 80 to support RPC external services.

### System Tuning

Optimize `sysctl` knobs

```bash
sudo bash -c "cat >/etc/sysctl.d/21-haproxy.conf <<EOF
# Increase system level number of allowed open file descriptors
fs.nr_open=1100000
fs.file-max=1100000

# Set tcp config
net.ipv4.ip_local_port_range=5000 65000
net.ipv4.tcp_syncookies = 1
net.core.somaxconn = 4096
EOF"

sudo sysctl -p /etc/sysctl.d/21-haproxy.conf
```

Increase `systemd` file limits

```bash
sudo mkdir /etc/systemd/system/haproxy.service.d
sudo bash -c "cat >/etc/systemd/system/haproxy.service.d/21-override.conf <<EOF
[Service]
# Increase user level number of hard limit and soft limit of file descriptors
LimitNOFILE=1000000
EOF"
sudo systemctl daemon-reload
sudo systemctl enable haproxy
```

### Install `HAProxy`

```bash
sudo apt-get install --no-install-recommends software-properties-common
sudo add-apt-repository ppa:vbernat/haproxy-3.0
sudo apt-get install haproxy=3.0.\*
```

### Add Routing

Edit `/etc/haproxy/haproxy.cfg`, routing archival requests to archival RPC endpoint and other requests to HyperGrid.

```
    acl methodGetBlock req.body -m reg \"getBlock\"
    acl methodGetBlocks req.body -m reg \"getBlocks\"
    acl methodGetBlocksWithLimit req.body -m reg \"getBlocksWithLimit\"
    acl methodGetBlockTime req.body -m reg \"getBlockTime\"
    acl methodGetSigsForAddr req.body -m reg \"getSignaturesForAddress\"
    acl methodGetTransaction req.body -m reg \"getTransaction\"
    use_backend archival if methodGetBlock
    use_backend archival if methodGetBlocks
    use_backend archival if methodGetBlocksWithLimit
    use_backend archival if methodGetBlockTime
    use_backend archival if methodGetSigsForAddr
    use_backend archival if methodGetTransaction
    default_backend HyperGrid
```

### Start

```bash
systemctl start haproxy
```
