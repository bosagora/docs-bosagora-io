One of the chief components of any blockchain is the consensus algorithm. The consensus algorithm is the method by which all the nodes in the blockchain come to an agreement on what is in a block before it gets permanently stored there.

Consensus algorithms try and balance two requirements: they should be robust and efficient. In other words, you want the algorithm to ensure consensus using the least amount of resources. There are three main prototocols used in consensus algorithms for blockchains today.

The first protocol used—the one used for Bitcoin—is *Proof of Work* (PoW). It operates by having every node do a mathematically difficult computation. It’s a very safe (i.e., robust) algorithm, but very inefficient as it requires a considerable amount of computation power (from each node) and therefore requires a lot of electrical power.

The next generation of protocol is *Proof of Stake* (PoS). Here each node stakes some cryptocurrency (called *tokens*) for the right to vote on a transaction. Unlike PoW, PoS is not computationally intensive. It is however, not as robust as PoW.

The third protocol is *Delegated Proof of Stake* (DPoS). Like PoS, each node stakes some tokens for the right to vote. But in this case, the nodes don't vote on trasactions. Instead, they vote *delegates* and the delegates vote on the transactions. Delegates are a subset of trusted nodes which are elected by the other nodes. Since only a subset of nodes has to validate transactions, consensus can be reached faster with DPoS. Fewer nodes voting however makes it less robust.

Obviously, requiring unanimous consensus like PoW and PoS

Bosagora uses a newer consensus algorithm, *Delegated Proof of Stake* (DPoS). With DPoS, nodes still have a stake of tokens, but rather than vote on the blocks in the blockchain, they delegate their voting power to a small subset of trusted nodes. Then these trusted nodes (called *delegates*) vote on the blocks. Since the system has only 20 or so delegates that verify the transactions, DPoS is exceptionally efficient.
