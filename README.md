# OTrack
Slogan 1: Trustless computing over the history of Bitcoin
Slogan 2: Trustless Bitcoin coprocessor for decentralized applications 或“智能合约”
OTrack allows smart contracts 或者不同虚拟机中的程序语言 to trustlessly compute over the entire history of Bitcoin, including UTXO执行指令数据 and Taproot scripts. Developers can off-load Bitcoin blockchain data monitoring & computing tasks to OTrack's trustless edge computing network and feed their dApps/smart contracts with ZK-verified results. The demo application of OTrack will be feeding trustless BRC20 token balance data for Bitcoin L2 DEXs.

## Background
In addition to being the digital cash, the bitcoin ledger has been leveraged as the asset settlement layer for many new assets created via off-chain protocols. These asset protocols use fields in Bitcoin UTXO (OP_RETURN, Taproot scripts) that are not executed by miners to log information to the Bitcoin ledger. Bitcoin L2 extends the programmability of Bitcoin L1, allowing L1 assets to have more utilities and interact with L2 dApps. Transaction settlements to L1 will remain as a need and L1 asset data feed to L2 will be the foundation to lots of L2 dApps.

The lack of a trustless data indexing service is one of the primary reasons why Bitcoin L1 protocols struggle to achieve wider adoption. Currently, there is no way to trustlessly interoperate with existing information stored in Taproot/OP_RETURN inputs in the Bitcoin UTXOs. Off-chain applications such as wallets and exchanges rely on centralized indexing services to provide Bitcoin L1 asset information, significant risks of single point of failure链接 and double spending attack链接.

On December 27, 2023, the Blockstream satellite transmitted a succinct zero-knowledge proof of the Bitcoin chain state链接 developed by the [Zerosync](https://zerosync.org/) team, enabling applications to verify the Bitcoin chain state without the need to validate each block individually.

This has inspired us for our endeavor to create a trustless Bitcoin data computing coprocessor, allowing dApps and smart contracts to access and compute over the data on Bitcoin in a trustless way.

## Protocol design
OTrack consists of two main technical pieces:

- OTrack edge network - a permissionless（staking可以算无需许可吗）edge computing network that carries block indexing & computing tasks
- OTrack ZK-oracle - a ZK-verified oracle that aggregates & outputs deterministic results to dApps & smart contracts  

The protocol is designed under these challenges:

1. Edge nodes have no capability to provide ZK-proof or execute heavy computation tasks.
2. ZK-oracle should provide ZK-proof of honest data aggregation within the 10-minute block time, the sooner the better.
3. The entire process remains trustless.

### OTrack edge network
Edge nodes are execution devices in the network, each node gets reward by executing tasks distributed by ZK-oracle and submitting correct results. Edge nodes mainly performs two processes：Bitcoin block indexing & computing task execution.
#### Bitcoin block indexing
Different from traditional blockchain data indexing, which needs to sync nearly 500GB of historical blockchain data, edge nodes use "one-block-only" indexing. In the view of Bitcoin data tracking, edge node can be seen as a state processor rather than an indexer, since it is designed to only carrying light computing tasks in the protocol. In OTrack, edge nodes only needs to process the data of the latest Bitcoin block and compute over it, leaving the state transition process to happen in the OTrack oracle (we will talk about this in later section). 

There are different methods to retrieve blockchain data for the data source, the simplest and least secure or decentralized way is to obtain it from a trusted Bitcoin peer, the most secure or decentralized way is to obtain it from a node that provides ZK-Bitcoin chain state proof, a ZK-proof that verifies Bitcoin consensus for the given block headers, transactions or witness data. Project such as [Zerosync](https://zerosync.org/) has delivered demo of such proof.

Nevertheless, OTrack is designed to output deterministic data with secure data aggregation algorithm (we will talk about this in later section), it means that the network will have its own way to find out "the correct answer" when edge nodes are submitting inconsistent answers, therefore using different data sources is allowed but trying to provide the correct answers is the way to maximize profits for edge nodes. OTrack will also provide ZK-Bitcoin chain state proof verification methods for edge node clients.
#### Computing task execution
Edge node provides a WASM environment for executing computation tasks from various applications and languages. This allows developers to drop any program logic based on Bitcoin data to OTrack network and obtain desired results feeding back to their smart contracts or off-chain apps trustlessly. 

[补个node的图]







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
