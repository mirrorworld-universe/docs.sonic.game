---
description: >-
  Step-by-step guide for deploying a new HSSN Validator Node, including system
  requirements, hardware configurations, and setup instructions.
---

# Deploying an HSSN Validator

## Operating System Requirements

* **Ubuntu Server 22.04.4 LTS**

## Hardware Requirements

| Configuration | CPU     | RAM   | SSD  |
| ------------- | ------- | ----- | ---- |
| **Low-End**   | 32-core | 128GB | 5TB  |
| **Mid-Range** | 64-core | 256GB | 10TB |

## HSSN Validator Setup

### 1. Download and Install the Package

```bash
wget https://grid-sonic.hypergrid.dev/downloads/hypergrid-ssn.tar.gz
tar -zxvf hypergrid-ssn.tar.gz
```

### 2. Initialization Settings

Navigate to the extracted directory:

```bash
cd .hypergrid-ssn
```

Create a key pair:

```bash
./bin/hypergrid-ssnd keys add my_validator --keyring-backend test
```

Initialize the configuration (replace `<NODE_NAME>` with your node name):

```bash
./bin/hypergrid-ssnd init <NODE_NAME> --default-denom hsol --chain-id hypergridssn
```

Copy the `~/.hypergrid-ssn/genesis.json` file and overwrite the the contents of the file generated at `~/.hypergrid-ssn/config/genesis.json`.

Modify `config/app.toml`:

```toml
minimum-gas-prices = "0hsol"
```

Modify `config/config.toml`:

Update the external IP. Make sure to replace the `<YOUR_NODE_IP_ADDRESS>` with your node's external IP address.&#x20;

```toml
external_address = "<YOUR_NODE_IP_ADDRESS>:26656"
persistent_peers = "1a4b042887edd9ea3da11d858ab4bcb953069f96@172.31.10.244:26656"
```

### 3. Run the Validator

{% hint style="info" %}
Make sure to replace `<NODE_NAME>` with the name you configured in Step 2.
{% endhint %}

```bash
./bin/hypergrid-ssnd start --moniker <NODE_NAME>
```

### 4. Networking

Display your validator's account address:

```bash
./bin/hypergrid-ssnd keys show my_validator -a --keyring-backend test
```

Use the account address obtained from the command above to get HSOL tokens from the HSSN faucet.

Show your Tendermint validator public key:

```bash
./bin/hypergrid-ssnd tendermint show-validator
```

{% hint style="info" %}
Replace the `pubkey` in `validator.json`  with the the public key you obtained above and update the `moniker` with the corresponding node name you configured in Step 2:
{% endhint %}

```json
{
    "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"KRrCrMHaeog5IAwSxFsUk/teRwDaZIhHQ5gFkrqcums="},
    "amount": "100000000hsol",
    "moniker": "<NODE_NAME>",
    "commission-rate": "0.1",
    "commission-max-rate": "0.2",
    "commission-max-change-rate": "0.01",
    "min-self-delegation": "1"
}
```

Stake as a validator:

```bash
./bin/hypergrid-ssnd tx staking create-validator ./validator.json --from my_validator --keyring-backend test --chain-id hypergridssn
```

Check if the operation was successful:

```bash
./bin/hypergrid-ssnd q staking validators
```

### 5. Install and Configure Solana Client

Install the Solana client:

```bash
sh -c "$(curl -sSfL https://release.solana.com/v1.18.18/install)"
```

Verify the installation:

```bash
solana --version
```

Set the Solana configuration:

```bash
solana config set --url http://sonic-grid.hypergrid.dev
solana-keygen new --no-passphrase
solana address
```

Use the account address obtained from the command above to get SOL tokens from the Sonic grid faucet and the Solana Devnet faucet.

### 6. Register Node

{% hint style="info" %}
Make sure to replace `<NODE_NAME>` with the name you configured in Step 2.
{% endhint %}

```bash
./bin/hypergrid-ssnd tx hypergridssn create-hypergrid-node $(solana address) "<NODE_NAME>" "https://hssnx.hypergrid.dev/" 1 "" $(date +%s) --from my_validator -y --chain-id hypergridssn --keyring-backend test
```

If you have any questions, please email [operators@sonic.game](mailto:operators@sonic.game) for assistance.
