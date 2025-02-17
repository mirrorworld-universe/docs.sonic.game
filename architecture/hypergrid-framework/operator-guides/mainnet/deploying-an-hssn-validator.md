---
description: >-
  Step-by-step guide for deploying a new HSSN Validator Node, including system
  requirements, hardware configurations, and setup instructions.
---

# Deploying an HSSN Validator

## Operating System Requirements

* **Ubuntu Server 22.04.4 LTS**

## Hardware Requirements

| Configuration | CPU      | RAM   | SSD  |
| ------------- | -------- | ----- | ---- |
| **Low-End**   | 64-core  | 128GB | 7TB  |
| **Mid-Range** | 128-core | 256GB | 10TB |

### Server Port Policy

* Open ports 80 and 443 to support RPC external services.
* Open TCP and UDP protocol ports 1317, 9090, 26656, 26657

### Key Things To Note

1. **This document is a work in progress**. Some parts of it may change as it's progressively polished. However, the bulk of will remain the same.
2. HSSN validator setup is currently only being gradually rolled out to selected Sonic's Ecosystem Partners, and will gradually be publicly rolled. If you would like to run an HSSN Node, please contact our team on [**Discord**](https://discord.gg/mApwvpm5E8)**.**
3. Please make use of the [**Discord**](https://discord.gg/mApwvpm5E8) chat to follow any new developments, as well as for node operator support.

## HSSN Validator Setup

### 1. Download and Install the Package

```bash
wget https://distribution.sonic.game/hypergrid-ssn_mainnet_alpha.tar.gz
tar -zxvf hypergrid-ssn_mainnet_alpha.tar.gz
cp .hypergrid-ssn/.hypergrid.yaml ~/
```

### 2. Initialization Settings

Navigate to the extracted directory:

```bash
cd .hypergrid-ssn
```

Create a key pair:

```bash
./bin/hypergrid-ssnd keys add my_validator --keyring-backend test

MY_VALIDATOR_ADDRESS=$(./bin/hypergrid-ssnd keys show my_validator -a --keyring-backend test)
```

Initialize the configuration (replace `<NODE_NAME>` with your node name):

```bash
./bin/hypergrid-ssnd init <NODE_NAME> --default-denom hsol --chain-id hypergridssn

sed -i '/\[api\]/,+3 s/enable = false/enable = true/' config/app.toml
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
persistent_peers = "d7b3d837af78dd806a4a4553f1824fe5a5a8131c@54.245.12.4:26656"
```

Create or Modify `~/.hypergrid.yaml` \


```bash
sonic:
  fee_program_id: SonicFeeSet1ement11111111111111111111111111
  fee_data_account_id: SonicFeeSet1ementData1111111111111111111112
  private_key: /home/ubuntu/.config/solana/id.json
  state_oracle_url: https://oracle.mainnet-alpha.sonic.game
  inbox_program_id: inboxov93C8R7uijTMTLtdNWqa1uzUdHXo3zpgL5a7j
```

### 3. Run the Validator

{% hint style="info" %}
Make sure to replace`<NODE_NAME>` with the name you configured in Step 2.
{% endhint %}

Create or Modify `~/.hypergrid-ssn/start.sh`

```
cd /home/ubuntu/.hypergrid-ssn
nohup ./bin/hypergrid-ssnd start --moniker <NODE_NAME> > ./logs/hssn.log 2>&1 &
cd -
```

Start service:

```bash
./start.sh
```

Check log:

```
tail -f /home/ubuntu/.hypergrid-ssn/logs/hssn.log
```

{% hint style="warning" %}
1. Please share your validator's public IP address to our team on [**Discord**](https://discord.gg/mApwvpm5E8).
2. Allow the `start` command to run until your node has fully indexed and caught up with all the rest of the HSSN network's block height. This may take a couple of hours.\
   \
   You can see the current block height of the HSSN network on the [Blocks page of the HSSN Explorer Dashboard](https://explorer-hssn.mainnet-alpha.sonic.game).
{% endhint %}

After your node has caught up with the rest of the network, please proceed to the next steps.

### 4. Networking

Display your validator's account address:

```bash
cd /home/ubuntu/.hypergrid-ssn
./bin/hypergrid-ssnd keys show my_validator -a --keyring-backend test
```

Use the account address obtained from the command above to get HSOL tokens.

{% hint style="info" %}
**Important**: The following steps require you to fund your validator account.\
\
Please contact our team on [**Discord**](https://discord.gg/mApwvpm5E8) and we can airdrop you some HSOL manually.
{% endhint %}

Because the new validator needs to sync and replay all blocks from the main validator, it will take a while to do that. Till you can query valid balances of the new account.

```
./bin/hypergrid-ssnd q bank balances <ACCOUNT_ADDRESS>
```

Display your HSSN validator public key:

```bash
cd /home/ubuntu/.hypergrid-ssn
./bin/hypergrid-ssnd tendermint show-validator
```

Create file `validator.json`, replace the `<VALIDATOR_PUBKEY>` in validator.json with the public key you obtained above and update the `<NODE_NAME>`with the corresponding node name you configured in Step 2.

{% hint style="info" %}
Please make sure to replace the \<VALIDATOR\_PUBKEY> and \<NODE\_NAME> fields.
{% endhint %}

```json
{
   "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"<VALIDATOR_PUBKEY>"},
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

press `y` to sign the transaction.\
Check if the operation was successful:

```bash
./bin/hypergrid-ssnd q staking validators
```

### 5. Install and Configure Solana Client

Install the Solana client:

```bash
sh -c "$(curl -sSfL https://release.anza.xyz/stable/install)"
```

Verify the installation:

```bash
solana --version
```

Set the Solana configuration:

```bash
solana config set --url https://api.mainnet-alpha.sonic.game
solana-keygen new --no-passphrase
solana address
```

Use the account address obtained from the command above to get SOL tokens from the Sonic grids and Solana Mainnet-Beta.

### 6. Register Node

{% hint style="info" %}
Make sure to replace `<NODE_NAME>` with the name you configured in Step 2, and change the `"https://your-node-rpc-url.com/"` to the rpc URL of your choice for your node. (The url corresponding service at port 1317, which is not required at this step. You can configure this later using [NGINX](https://ubuntu.com/tutorials/install-and-configure-nginx#1-overview)).
{% endhint %}

```bash
cd /home/ubuntu/.hypergrid-ssn

./bin/hypergrid-ssnd tx hypergridssn create-hypergrid-node $(solana address) "<NODE_NAME>" "https://your-node-rpc-url.com/" 1 "" $(date +%s) --from my_validator -y --chain-id hypergridssn --keyring-backend test
```

### 7. Confirm Validator Node Registration

You can confirm that your node was successfully registered with the following command:

```shell
./bin/hypergrid-ssnd query hypergridssn list-hypergrid-node
```

You should be able to see your validator node in the list of validators. You should also see your validator in the [Validators page of the HSSN Explorer](https://explorer-hssn.mainnet-alpha.sonic.game/validators).

Congratulations! You should now be successfully running your HSSN node at this stage.

***

### 8. Community / Support

Please join the [HyperGrid Discord](https://discord.gg/mApwvpm5E8) to get more updates on latest changes as well as to get faster support responses.

For more official queries, please email [operators@sonic.game](mailto:operators@sonic.game) for assistance.
