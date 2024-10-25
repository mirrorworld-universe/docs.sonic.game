
# Oracles  
This reference guide presents various Oracles that you can utilize while building on Sonic. Oracles supply offchain data to the blockchain, enabling smart contracts to access a diverse range of information.

## Pyth Network

The [Pyth Network](https://pyth.network/) is one of the largest first-party Oracle network, delivering real-time data across [a vast number of chains](https://docs.pyth.network/price-feeds/contract-addresses).

The network comprises some of the world’s [largest exchanges, market makers, and financial services providers](https://pyth.network/publishers). These publish proprietary data on-chain for aggregation and distribution to smart contract applications. 

## Using Pyth network

The Pyth introduces an innovative low-latency [pull oracle design](https://docs.pyth.network/documentation/pythnet-price-feeds/on-demand), where users can pull price updates onchain when needed, enabling everyone in the onchain environment to access that data point most efficiently. Pyth network updates the prices every **400ms**, making Pyth one of the fastest on-chain oracles.

Developers on Sonic have permissionless access to any of [Pyth’s price feeds](https://pyth.network/developers/price-feed-ids) for equities, ETFs, commodities, foreign exchange pairs, and cryptocurrencies.

 It is generally recommended to follow the [consumer best practices](https://docs.pyth.network/documentation/pythnet-price-feeds/best-practices) when consuming Pyth data.

For more information, check out the official [Pyth documentation](https://docs.pyth.network/price-feeds). There are details on the various functions available for interacting with the Pyth smart contract in the [API Reference section](https://api-reference.pyth.network/price-feeds/evm/getPrice).

## Pyth on Sonic

The Pyth Network smart contract is available at the following address:

Receiver address:

- Testnet: [rec5EKMGg6MxZYaMdyBfgwp4d5rB9T1VQH5pJv5LtFJ](https://explorer.sonic.game/address/rec5EKMGg6MxZYaMdyBfgwp4d5rB9T1VQH5pJv5LtFJ?cluster=testnet)
- Devnet: [rec5EKMGg6MxZYaMdyBfgwp4d5rB9T1VQH5pJv5LtFJ](https://explorer.sonic.game/address/rec5EKMGg6MxZYaMdyBfgwp4d5rB9T1VQH5pJv5LtFJ?cluster=devnet)

Price feed program:

- Testnet: [pythWSnswVUd12oZpeFP8e9CVaEqJg25g1Vtc2biRsT](https://explorer.sonic.game/address/pythWSnswVUd12oZpeFP8e9CVaEqJg25g1Vtc2biRsT?cluster=testnet)
- Devnet: [pythWSnswVUd12oZpeFP8e9CVaEqJg25g1Vtc2biRsT](https://explorer.sonic.game/address/pythWSnswVUd12oZpeFP8e9CVaEqJg25g1Vtc2biRsT?cluster=devnet)

Additionally, click to access the [Pyth price-feed IDs](https://pyth.network/developers/price-feed-ids).