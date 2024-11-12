---
description: >-
  Step-by-step guide for deploying a new Sonic RPC Node on Testnet V1, including
  system requirements, hardware configurations, and setup instructions.
---

# Deploying a Sonic RPC Node

## Table of Contents

1. [System Requirements](deploying-a-sonic-rpc-node.md#system-requirements)
2. [Server Port Policy](deploying-a-sonic-rpc-node.md#server-port-policy)
3. [System Tuning](deploying-a-sonic-rpc-node.md#system-tuning)
4. [Install Sonic Devnet Validator](deploying-a-sonic-rpc-node.md#install-sonic-devnet-validator)
5. [Initialization](deploying-a-sonic-rpc-node.md#initialization)
6. [Configuration](deploying-a-sonic-rpc-node.md#configuration)
7. [Running the Node](deploying-a-sonic-rpc-node.md#running-the-node)
8. [Shutting Down the Node](deploying-a-sonic-rpc-node.md#shutting-down-the-node)
9. [Operator Guides](deploying-a-sonic-rpc-node.md#operator-guides)
10. [Support](deploying-a-sonic-rpc-node.md#support)

## System Requirements

### Operating System

* Ubuntu Server 22.04.4 LTS

## Hardware Requirements

| Configuration | CPU      | RAM   | SSD  |
| ------------- | -------- | ----- | ---- |
| Low-End       | 64-core  | 512GB | 10TB |
| Mid-Range     | 128-core | 1T    | 15TB |

###

## Server Port Policy

* Open ports 80 and 443 for RPC external services.
* Open TCP and UDP protocol ports in the range of 8000 to 9000.
* Whitelist IP addresses 52.13.90.86.

After configuration, send your server's public IP address to [operators@sonic.game](mailto:operators@sonic.game).

## System Tuning

Your system needs to be tuned to run properly. Your RPC validator may not start without these settings.

### Optimize sysctl knobs:

```bash
sudo bash -c "cat >/etc/sysctl.d/21-solana-validator.conf <<EOF
# Increase UDP buffer sizes
net.core.rmem_default = 134217728
net.core.rmem_max = 134217728
net.core.wmem_default = 134217728
net.core.wmem_max = 134217728

# Increase memory mapped files limit
vm.max_map_count = 1000000

# Increase number of allowed open file descriptors
fs.nr_open = 1000000
EOF"

sudo sysctl -p /etc/sysctl.d/21-solana-validator.conf
```

### Increase systemd and session file limits:

Add to the `[Service]` section of your `systemd` service file:

```sh
LimitNOFILE=1000000
```

Or add to the `[Manager]` section of `/etc/systemd/system.conf`:

```sh
DefaultLimitNOFILE=1000000
```

Then run:

```bash
sudo systemctl daemon-reload

sudo bash -c "cat >/etc/security/limits.d/90-solana-nofiles.conf <<EOF
# Increase process file descriptor count limit
* - nofile 1000000
EOF"
```

{% hint style="warning" %}
**Important**: Close all open sessions. Log out then log in again after making these changes.
{% endhint %}

## Install Sonic Testnet Validator

### Option 1: Pre-built Binary Package

```bash
wget https://grid-sonic.hypergrid.dev/downloads/sonic_testnet_grid_v1.tar.gz
tar -zxvf sonic_testnet_grid_v1.tar.gz
```

### Option 2: Build from Source Code

#### 1. Install dependencies:

```bash
curl https://sh.rustup.rs -sSf | sh
source $HOME/.cargo/env
rustup component add rustfmt

sudo apt-get update
sudo apt-get install libssl-dev libudev-dev pkg-config zlib1g-dev llvm clang cmake make libprotobuf-dev protobuf-compiler
```

#### 2. Download the source code:

```bash
git clone https://github.com/mirrorworld-universe/hypergrid-grid
cd hypergrid-grid
git checkout base_on_1.18.23
```

#### 3. Build

```sh
mkdir ~/sonic_node
./scripts/cargo-install-all.sh ~/sonic_node
cp ./start_node.sh ~/sonic_node/
cp ./stop_node.sh ~/sonic_node/
```

## Initialization

```bash
cd ~/sonic_node
mkdir config
mkdir logs

./bin/solana-keygen new --no-passphrase
./bin/solana config set --url http://127.0.0.1:8899
./bin/solana-keygen new -o ./config/validator-keypair.json
```

## Check System Variables

```bash
cat /proc/sys/net/core/rmem_default
cat /proc/sys/net/core/rmem_max
cat /proc/sys/net/core/wmem_default
cat /proc/sys/net/core/wmem_max
cat /proc/sys/vm/max_map_count
cat /proc/sys/fs/nr_open
cat /proc/sys/fs/file-max
cat /proc/sys/vm/swappiness
```

## Configuration

Edit the `start_node.sh` file and replace **`YOUR_PUBLIC_IP`** with your machine's public IP address.

This script contains various configuration options. Ensure you replace **`YOUR_PUBLIC_IP`** with your actual public IP address, **`VALIDATOR_ID`**, with the public address of the known validator on the cluster, and **`VALIDATOR_GENESIS_HASH`** with the genesis hash of the cluster you are deploying for.

```sh
mkdir -p /data/logs
mkdir -p /data1/accounts
mkdir -p /data/ledger

#!/bin/bash
export PATH=/bin:/usr/bin:/root/sonic_node/bin
export RUST_LOG=${RUST_LOG:-solana=info,solana_runtime::message_processor=info,solana_metrics::metrics=warn}
export RUST_BACKTRACE=full
export SONIC_FEE_MULTIPLIER=5000

exec solana-validator \
    --identity ./config/validator-keypair.json \
    --known-validator VALIDATOR_ID \
    --repair-validator VALIDATOR_ID \
    --ledger ./ledger \
    --rpc-port 8899 \
    --full-rpc-api \
    --no-voting \
    --rpc-bind-address 0.0.0.0 \
    --only-known-rpc \
    --gossip-host YOUR_PUBLIC_IP \
    --gossip-port 8001 \
    --entrypoint 52.13.90.86:8001 \
    --public-rpc-address YOUR_PUBLIC_IP:8899 \
    --enable-rpc-transaction-history \
    --enable-extended-tx-metadata-storage \
    --no-wait-for-vote-to-start-leader \
    --no-os-network-limits-test \
    --rpc-pubsub-enable-block-subscription \
    --rpc-pubsub-enable-vote-subscription \
    --account-index program-id \
    --account-index spl-token-owner \
    --account-index spl-token-mint \
    --accounts-db-cache-limit-mb 102400 \
    --accounts-index-memory-limit-mb 40960 \
    --accounts-index-scan-results-limit-mb 40960 \
    --limit-ledger-size 500000000 \
    --expected-genesis-hash VALIDATOR_GENESIS_HASH \
    --wal-recovery-mode skip_any_corrupted_record \
    --log ./logs/validator.log &
```

Make sure to replace **`YOUR_PUBLIC_IP`** with your actual public IP address, and replace **`VALIDATOR_ID`** and **`VALIDATOR_GENESIS_HASH`** with the right values.

### Testnet V1 Variables:

* **`VALIDATOR_ID`**: `Ci2TRaVpJoNmTUVhw5tkTVnskXVJ7FQHCMKRFGrQSHJB`
* **`VALIDATOR_GENESIS_HASH`** : E8nY8PG8PEdzANRsv91C2w28Dbw9w3AhLqRYfn5tNv2C

### Devnet Variables (Deprecated):

* **`VALIDATOR_ID`**: `CQqu5MsTpH1mTwEsZ75QzPtXGTz9ziEvKwpcAstKG9WJ`
* **`VALIDATOR_GENESIS_HASH`** : BsJstMXKW4DpjzHPsSCdEcAn4YtpNiLFRFa5M5L7UxFx

## Configuring Domain Name for your RPC Node

At this point, you should be ready to configure a domain name for your service. You may use Nginx or any reverse proxy of your choice.

## Running the Node

```bash
./start_node.sh
```

## Shutting Down the Node

You can gracefully shut down the node by running the following command inside the `~/sonic_node`directory

```bash
./stop_node.sh
```

## Operator Guides

It is recommended to read the [guides and operator best practices](https://docs.solanalabs.com/operations/best-practices/general) outlined in the official Solana documentation.

## Support

For support or questions, contact the Sonic Operators team at [operators@sonic.game](mailto:operators@sonic.game) or DM [@codebender828](https://x.com/codebender828) on X.

