---
description: >-
  Step-by-step guide for deploying a new Sonic RPC Node, including system
  requirements, hardware configurations, and setup instructions.
---

# Deploying a New Grid

## System Requirements

### Operating System

* Ubuntu Server 22.04.4 LTS

### Hardware Requirements

| Configuration | CPU      | RAM   | SSD  |
| ------------- | -------- | ----- | ---- |
| Low-End       | 64-core  | 128GB | 5TB  |
| Mid-Range     | 128-core | 384GB | 10TB |

## Install Grid Validator Client

#### Option 1: Pre-built Binary Package

```bash
wget https://grid-sonic.hypergrid.dev/downloads/hypergrid-rpcnode.tar.gz
tar -zxvf hypergrid-rpcnode.tar.gz
```

#### Option 2: Build from Source Code

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

### Initialization

```bash
cd grid_node
./bin/solana-keygen new --no-passphrase
./bin/solana config set --url http://127.0.0.1:8899

./init_validator.sh
./bin/solana address
```

{% hint style="info" %}
**Note:** Use the account address obtained from the last command to get SOL tokens from the Sonic grid faucet.
{% endhint %}

### HSSN Client Setup

```bash
cd ..
wget https://grid-sonic.hypergrid.dev/downloads/hypergrid-ssn_client.tar.gz
tar -zxvf hypergrid-ssn.tar.gz

cd .hypergrid-ssn
./bin/hypergrid-ssnd init
```

Modify the `config/client.toml` file:

```toml
node = "tcp://172.31.10.244:26657"
```

Create a key pair:

```bash
./bin/hypergrid-ssnd keys add my_key --keyring-backend test
./bin/hypergrid-ssnd keys show my_key -a --keyring-backend test
```

{% hint style="info" %}
**Note:** Use the account address obtained from the last command to get HSOL tokens from the HSSN faucet.
{% endhint %}

### Run the Validator

```bash
cd ../grid_node
./run_validator.sh
```

### Register Node

```bash
./bin/solana validators
```

Send the Identity information to [**operators@sonic.game**](mailto:operators@sonic.game)

## Post-Deployment

After successfully deploying your new Grid, it becomes part of the HSSN, participating in the broader Grid ecosystem. Your Grid will:

* Process transactions and interact with users
* Participate in the gas fee distribution process
* Sync with the Solana Base Layer through the HSSN

For monitoring your Grid's performance and interactions within the network, refer to the [HSSN Dashboard Explorer](../hssn-explorer-overview.md).

{% hint style="info" %}
Learn more about the [hssn-explorer-overview.md](../hssn-explorer-overview.md "mention")
{% endhint %}

## Operator Guides

It is recommended to read the [guides and operator best practices](https://docs.solanalabs.com/operations/best-practices/general) outlined in the official Solana documentation.

## Support

For support or questions, contact the Sonic Operators team at [operators@sonic.game](mailto:operators@sonic.game) or DM [@codebender828](https://x.com/codebender828) on X.

