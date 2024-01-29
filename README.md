# OTrack
Slogan 1: Trustless computing over the history of Bitcoin

Slogan 2: Trustless Bitcoin coprocessor for decentralized applications 或“智能合约”

OTrack allows smart contracts 或者不同虚拟机中的程序语言 to trustlessly compute over the entire history of Bitcoin, including UTXO执行指令数据 and Taproot scripts. Developers can off-load Bitcoin blockchain data monitoring & computing tasks to OTrack's trustless edge computing network and feed their dApps/smart contracts with ZK-verified results. The demo application of OTrack will be feeding trustless BRC20 token balance data for Bitcoin L2 DEXs.

## Background
In addition to being the digital cash, the bitcoin ledger has been leveraged as the asset settlement layer for many new assets created via off-chain protocols. These asset protocols use fields in Bitcoin UTXO (OP_RETURN, Taproot scripts) that are not executed by miners to log information to the Bitcoin ledger. Bitcoin L2 extends the programmability of Bitcoin L1, allowing L1 assets to have more utilities and interact with L2 dApps. Transaction settlements to L1 will remain as a need and L1 asset data feed to L2 will be the foundation to lots of L2 dApps.

The lack of a trustless data indexing service is one of the primary reasons why Bitcoin L1 protocols struggle to achieve wider adoption. Currently, there is no way to trustlessly interoperate with existing information stored in Taproot/OP_RETURN inputs in the Bitcoin UTXOs. Off-chain applications such as wallets and exchanges rely on centralized indexing services to provide Bitcoin L1 asset information, significant risks of single point of failure链接 and double spending attack链接.

On December 27, 2023, the [Blockstream satellite transmitted a succinct zero-knowledge proof of the Bitcoin chain state](https://x.com/ZeroSync_/status/1740543975512232186?s=20) developed by the [Zerosync](https://zerosync.org/) team, enabling applications to verify the Bitcoin chain state without the need to validate each block individually.

This has inspired us for our endeavor to create a trustless Bitcoin data computing coprocessor, allowing dApps and smart contracts to access and compute over the data on Bitcoin in a trustless way.

## Protocol design
OTrack consists of two main technical pieces:

- OTrack edge network - a permissionless（staking可以算无需许可吗）edge computing network that carries block indexing & computing tasks
- OTrack ZK-oracle - a ZK-verified oracle that aggregates & outputs deterministic results to dApps & smart contracts  

The protocol is designed under these challenges:

1. Edge nodes have no capability to provide ZK-proof or execute heavy computation tasks.
2. ZK-oracle should provide ZK-proof of honest data aggregation within the 10-minute block time, the sooner the better.
3. The entire process remains trustless.
<img width="963" alt="image" src="https://github.com/OTrack-Oracle/Otrack/assets/86393764/2e3f54bc-c711-44fb-902c-f0e49a7d8785">

## OTrack edge network (DePIN)
Edge nodes are execution devices in the network, each node gets reward by executing tasks distributed by ZK-oracle and submitting correct results. Edge nodes mainly performs two processes：Bitcoin block indexing & computing task execution.
### One-block-only indexing
Different from traditional blockchain data indexing, which needs to sync nearly 500GB of historical blockchain data, edge nodes use "one-block-only" indexing. In the view of Bitcoin data tracking, edge node can be seen as a state processor rather than an indexer, since it is designed to only carrying light computing tasks in the protocol. In OTrack, edge nodes only needs to process the data of the latest Bitcoin block and compute over it, leaving the state transition process to happen in the OTrack oracle (we will talk about this in later section). 

There are different methods to retrieve blockchain data for the data source, the simplest and least secure or decentralized way is to obtain it from a trusted Bitcoin peer, the most secure or decentralized way is to obtain it from a node that provides ZK-Bitcoin chain state proof, a ZK-proof that verifies Bitcoin consensus for the given block headers, transactions or witness data. Project such as [Zerosync](https://zerosync.org/) has delivered demo of such proof.

Nevertheless, OTrack is designed to output deterministic data with secure data aggregation consensus algorithm (we will talk about this in later section), it means that the network will have its own way to find out "the correct answer" when edge nodes are submitting inconsistent answers, therefore using different data sources is allowed but trying to provide the correct answers is the way to maximize profits for edge nodes. OTrack will also provide ZK-Bitcoin chain state proof verification methods for edge node clients to enable trustless indexing.
### WASM VM
Edge node provides a WASM environment for executing computation tasks from various applications and languages. This allows developers to drop any program logic based on Bitcoin data to OTrack network and obtain desired results feeding back to their smart contracts or off-chain apps trustlessly. 
<img width="984" alt="image" src="https://github.com/OTrack-Oracle/Otrack/assets/86393764/06650312-2d61-4437-b95a-73b358a1d4c8">

如何区分程序计算量
提及物联网设备
## OTrack ZK-oracle
In essence, OTrack ZK-oracle is an off-chain data aggregator that provides detrministic pre-commit computation outputs and zk compute trace proof of the OTrack data aggregation consensus algorithm. （这个可以叫共识算法吗）As OTrack allows developers to put arbitrary program to compute over Bitcoin block data in the edge network, OTrack ZK-oracl does the jobs of data collection, dispute settlement, and data storage. 
### Sharding
Borrowing the concept "Sharding" from Ethereum, OTrack creates shards for different applications with different computing programs when recruiting edge nodes for task distribution. Nodes in the same shard will form a temporary team to perform the same task together. Redundant computation will occur to ensure that the oracle collects enough reports to reach consensus in time.
### ZK-rollup blockchain model
Since edge node only indexes the latest block of Bitcoin and is computing for data updates, an application programm will need a storage for keeping all history data states available and verifiable, and thence the oracle is also used for hosting the global state and history for all the state transitions. It inherits the Bitcoin ledger account model and uses UTXO to track the state transitions for each program, leversaging zk-proof to rollup the entire chain when adding each block to maintain data integrity and computational honesty. Edge node will sync with the oracle for the latest state to calculate state updates against the old state, so that the computing jobs for edge nodes can always remain light. Developers can query the lastest state for their programs in a trustless way.
（流程图）
### 1-of-N trust model (data aggregation consensus algorithm)
Since the data is sourced from the Bitcoin blockchain consensus, the "truth" is always deterministic, so the network is 0 fault-tolerant. The network defenses sybil attack with 1-of-N trust model in the sense that when at least one honest edge node in a team provides the correct answer the system will detect fraud and trigger arbitration. 
When two or more edge nodes in the same team provide inconsistent results, the task execution is disputed and arbitration will occur between the team and the oracle. The oracle will challenge each team member with a random homomorphically encrypted secret that is the distance from the previous consensus state to the new commit states, and let them to compute again. Honest node will always get an answer that is predictable and the same with any of the orginal commited results (from honest or malicious nodes), malicious node will 
（图解）
### Accessibility
For good accessibility, ZTrack should be able to feed data for on-chain and off-chain applications accordingly with on-chain smart contract outputs and off-chain RPC data query service. To make the OTrack data feed accessible for smart contracts on various Bitcoin layer 2 networks seamlessly, we will deploy OTrack state transition & zk verification service in various Bitcoin layer 2 networks as well with their supported smart contract functionalities.
## ZK proof of data aggregation
Zero-knowledge proofs let the oracle prove that it honestly aggregate the data reported by the assigned edge node team based on the data aggregation consensus algorithm without revealing the computation. They have two key properties that we rely on here:

1. The on-chain registry of each edge node's metadata (pub-key, reputation, so that we can confirm all reports are provided by qualified nodes)
2. The signature of each reporting node on their reports (for data integrity)

The ZK circuit veirifies the oracle output result against the OTrack data aggregation consensus, it makes sure each output is aligned with the consensus that the result is reported by at least x amount of OTrack edge nodes consistently and there was no 



## Demo:BRC20 token balance index
The OTrack team will develop the BRC20 token balance data feed for the first demo application of OTrack. Since the orginals protocol has opened the pandora box of issuing assets on Bitcoin, BRC20 token has become one of the most popular token stardard in Bitcoin L1, however these tokens with a market cap of $1.19B are traded on centralized exchanges and exchanges are relying on heavy trust of centralized indexing. An autonymous decentralized indexing of BRC20 token balance complying with the Ordinals protocol and BRC20 protocol can nicely address the problem and support decentralized exchange with deterministic token balance data. 
This app creates a blockchain account model for every BRC20 token holder with their original Bitcoin addresses, and record state transitions of each BRC20 token in blocks secured by the OTrack data aggregation consensus algorithm and ZK-proof. Edge nodes will contribute to index info of valid BRC20 token inscriptions in every new Bitcoin block and submit to the oracle for aggregation, dApps can simply send query for the user's balnce of a BRC20 token to the oracle and be responded with the latest (before the next Bitcoin block is out) account state of the user and so get the balance info in trustless manner.
(设计一个可部署到edge节点的程序)

