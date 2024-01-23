# OTrack
Decentralized blockchain data indexing node for edge computing
## Background
In addition to being the digital cash, the bitcoin ledger has been leveraged as the asset settlement layer for many new assets created via off-chain protocols. These asset protocols use fields in Bitcoin UTXO (OP_RETURN, Taproot scripts) that are not executed by miners to log information to the Bitcoin ledger. Bitcoin L2 extends the programmability of Bitcoin L1, allowing L1 assets to have more utilities and interact with L2 dApps. Transaction settlements to L1 will remain as a need and L1 asset data feed to L2 will be the foundation to lots of L2 dApps.

The lack of a trustless data indexing service is one of the primary reasons why Bitcoin L1 protocols struggle to achieve wider adoption. Currently, there is no way to trustlessly interoperate with existing information stored in Taproot/OP_RETURN inputs in the Bitcoin UTXOs. Off-chain applications such as wallets and exchanges rely on centralized indexing services to provide Bitcoin L1 asset information, significant risks of single point of failure and double spending attack.

On December 27, 2023, the Blockstream satellite transmitted a succinct zero-knowledge proof of the Bitcoin chain state, a technology developed by the [Zerosync](https://zerosync.org/) team, enabling applications to verify the Bitcoin chain state without the need to validate each block individually.

This has inspired us for our endeavor to create a decentralized data indexing oracle for Bitcoin L1 assets, allowing millions of nodes to enter without the need to index the entire chain.

## OTrack Light Index
"OTrack light index" aims to develop efficient and low-cost on-chain data indexing solution, harnessing ZK rollup and DePIN to create decentralized oracle ouptting deterministic & verifiable on-chain data. This solution will be significantly valuable for establishing "account state" of the assets recorded in the blockchain ledger but are not processed by chain miners, allowing state transition to "happen" in a trustless & programmable way. Naturally fit for blockchain like Bitcoin that is lack of programmability in layer 1.

Let's take “creating account state for a BRC20 token" as the first challenge.

### Account model
Inheriting Bitcoin's UTXO model, we will track BRC20 token balance by the valid inscriptions (UTXOs) belong to each address, based on the BRC20 protocol and Ordinals protocol. Light indexers can align with the latest protocol metadata through client version update, these protocols can be BRC20, BRC30, ARC20, SCR20, etc. After obtaining new inscriptions, light indexers will validate inscriptions by the corresponding protocols and abandon invalid inscriptions.

### Transaction verification
We call it "one-block-only" indexing. Different from traditional blockchain data indexing, which needs to sync nearly 500GB of historical blockchain data, OTrack light index uses a [succinct zero-knowledge Bitcoin chainstate proof](https://zerosync.org/zerosync.pdf) (kudos to the Zerosync team) and only need to index the latest Bitcoin block for pasring newest BRC20 token state transition information, and verify the transation against a zero-knowledge global state proof provided by the oracle which will be introduced below.

<img width="395" alt="image" src="https://github.com/OTrack-Oracle/Otrack/assets/86393764/549ad370-bade-4665-9cc5-f60f12b977bb">


## Decentralized data aggregation (the oracle)

### Consensus
### Reputation governance
### Global state proof
