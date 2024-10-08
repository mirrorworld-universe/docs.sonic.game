---
description: >-
  Step-by-step guide for deploying a new Sonic RPC Node, including system
  requirements, hardware configurations, and setup instructions.
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
| Low-End       | 64-core  | 256GB | 10TB |
| Mid-Range     | 128-core | 512GB | 15TB |

###

## Server Port Policy

* Open ports 80 and 443 for RPC external services.
* Open TCP and UDP protocol ports in the range of 8000 to 9000.
* Whitelist IP addresses 52.10.174.63 and 35.164.22.3.

After configuration, send your server's public IP address to [operators@sonic.game](mailto:operators@sonic.game).

## System Tuning

Optimize sysctl knobs:

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

Increase systemd and session file limits:

```bash
sudo bash -c "cat >/etc/security/limits.d/90-solana-nofiles.conf <<EOF
# Increase process file descriptor count limit
* - nofile 1000000
EOF"
```

{% hint style="warning" %}
**Important**: Close all open sessions after making these changes.
{% endhint %}

## Install Sonic Devnet Validator

### Option 1: Pre-built Binary Package

```bash
bashCopywget https://grid-sonic.hypergrid.dev/downloads/hypergrid-rpcnode.tar.gz
tar -zxvf hypergrid-rpcnode.tar.gz
```

### Option 2: Build from Source Code

Install dependencies:

```bash
curl https://sh.rustup.rs -sSf | sh
source $HOME/.cargo/env
rustup component add rustfmt

sudo apt-get update
sudo apt-get install libssl-dev libudev-dev pkg-config zlib1g-dev llvm clang cmake make libprotobuf-dev protobuf-compiler

git clone https://github.com/mirrorworld-universe/hypergrid-sonic-origin-grid
cd hypergrid-sonic-origin-grid

mkdir ~/grid_node
./scripts/cargo-install-all.sh ~/grid_node
cp ./run_rpcnode.sh ~/grid_node/
```

## Initialization

```bash
cd ~/grid_node
mkdir config
mkdir logs

./bin/solana-keygen new --no-passphrase
./bin/solana config set --url http://127.0.0.1:8899
./bin/solana-keygen new -o ./config/validator-keypair.json
```

## Configuration

Edit the `run_rpcnode.sh` file and replace `YOUR_PUBLIC_IP` with your machine's public IP address.

## Running the Node

```bash
./run_rpcnode.sh
```

This script contains various configuration options. Ensure you replace **`YOUR_PUBLIC_IP`** with your actual public IP address, **`KNOWN_VALIDATOR`**, with the public address of the known validator on the cluster, and **`GENESIS_HASH`** with the genesis hash of the cluster you are deploying for.

```sh
./bin/solana-validator \
    --identity ./config/validator-keypair.json \
    --known-validator KNOWN_VALIDATOR \
    --ledger ./ledger \
    --rpc-port 8899 \
    --full-rpc-api \
    --no-voting \
    --rpc-bind-address 0.0.0.0 \
    --only-known-rpc \
    --gossip-host YOUR_PUBLIC_IP \
    --gossip-port 8001 \
    --entrypoint 52.10.174.63:8001 \
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
    --limit-ledger-size 5000000000 \
    --expected-genesis-hash GENESIS_HASH \
    --wal-recovery-mode skip_any_corrupted_record \
    --log ./logs/validator.log &
```

### Devnet Variables:

* `KNOWN_VALIDATOR`: CQqu5MsTpH1mTwEsZ75QzPtXGTz9ziEvKwpcAstKG9WJ
* `GENESIS_HASH` : BsJstMXKW4DpjzHPsSCdEcAn4YtpNiLFRFa5M5L7UxFx

### Testnet Variables:

* `KNOWN_VALIDATOR`: `HXQyiQxmVipgohFSDex3TSyFkFp6yttF1T3Rdp7fnfZP`
* `GENESIS_HASH` : Ep5wb4kbMk8yHqV4jMXNqDiMWnNtnTh8jX6WY59Y8Qvj

```bash
./run_rpcnode.sh
```

## Shutting Down the Node

To gracefully shut down the node:

```bash
cd ~/grid_node
./bin/solana-validator exit --force
```

## Operator Guides

It is recommended to read the [guides and operator best practices](https://docs.solanalabs.com/operations/best-practices/general) outlined in the official Solana documentation.

## Support

For support or questions, contact the Sonic Operators team at [operators@sonic.game](mailto:operators@sonic.game) or DM [@codebender828](https://x.com/codebender828) on X.

