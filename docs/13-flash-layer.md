# Flash Layer

## The Problem

The blockchain has a scalability issue. On the one hand, for security and stability, you want as many nodes as possible in the network. On the other hand, reaching a network-wide consensus requires every update on the blockchain to be broadcasted to every node. This is a serious challenge to reaching credit card transaction throughput levels that can reach tens of thousands per second.

The main problem with scaling blockchain is the fact that every transaction is public and must be validated by all network nodes. But there is a way around that.

## The Solution

The solution to the blockchain scalability challenge is to not make every transaction public. Frequently in business, two parties will do multiple transactions between them before they need to terminate that piece of business. Being able to take a multitude of transactions “off-chain” and onto layer 2 is a way to dramatically improve the transaction speed, and therefore scalability of the blockchain. And that is precisely how Bosagora addresses the scalability issue.

Bosagora uses *payment channels* which enable two parties to carry out everyday transactions between them without requiring approval from the blockchain. Then they only do net settlement of all their transactions on chain when needed.

Payment channels use a  second-layer payment protocol built on top of the Bosagora network. They are opened with an on-chain transaction and remain open until one or both parties decide to close the channel. While the channel is open, all communication happens only between the two parties.

At any point, either party has the means to close the channel with the most recent balance they mutually agreed on. In case of a dispute, the network is used as an arbiter to resolve the dispute. Payment channels only allow payments between two parties.

The benefits of these payment channels include the following:

+ Users do not need to open channels to every merchant individually
+ Fees are significantly smaller than on-chain fees
+ Payments are almost instantaneous. No waiting time for block confirmations.
+ Scales to millions of users, and near infinite transactions/second

The idea of payment channels lead to the Bosagora Flash Layer.

## Flash Layer

The Bosagora Flash Layer implements an improved version of Eltoo channels. [Eltoo]( https://bitcoinops.org/en/topics/eltoo/) is an update and settlement layer for second-layer payment channels like that used on Bosagora. The following sections explain how the Flash Layer works

### Opening a Channel

Channels are opened by the peer that provides the initial funds. This peer is referred to as which *funding peer*. Channels are marked as open with an on-chain transaction called the *funding transaction*. With funding transactions, the funds are locked with a multisig. The funds can only be consumed in a manner that both peers mutually agree on and provide the signatures for.

The steps are as follows:

1. The funding peer first creates the funding transaction and proposes opening a new channel to its peer, because publishing the funding transaction without any communication with the peer may result in losing the allocated funds forever.
2. The peer either rejects the channel according to its own configuration or accepts it, which triggers a set of additional steps before the channel is actually opened.
3. Before the funding peer can publish the funding transaction, two off-chain transactions are created and the peers exchange their signatures for them. These transactions are called *trigger* and *settlement*. They give both parties the means to close the channel at any point. The trigger transaction is an off-chain transaction which spends the funds from the funding transaction. If published to the blockchain, it begins a *non-collaborative channel close*.
4. Once the funding peer collects the required signatures for trigger and settlement transactions, it publishes the funding transaction to the Bosagora network.
5. Both peers continuously monitor the blockchain for externalization of the funding transaction. Externalization of the funding transaction marks the opening of the channel on the blockchain and the flash nodes start accepting payments on the newly opened channel after that.

Since the funding transaction is no different than any other transaction, no nodes other than the parties involved are aware of the new channel. Similar to layer-1, the Flash Layer utilizes a *gossip protocol* to ensure the optimal operation of the network. Nodes gossip the necessary information about the newly opened channel to their known network peers.

### Payments

*Payments* are the exchange of off-chain transactions that happen between the channel peers. Payments update the *channel balance*, which is the distribution of funds between parties. Each payment creates an *update* and a settlement transaction pair.

*Update transactions* attach to the trigger transaction or to a previous update transaction. These update transactions are only published to the blockchain in case of an uncollaborative channel close.

*Settlement transactions* may only attach to exactly the previous trigger or update transaction.  Settlement transactions contains the current balance distribution between the channel parties. Settlement transactions cannot be externalized until their relative time lock expires. Once they are externalized, that marks close of the channel. See the figure below for a visual depiction of a representative transaction.

![funding transaction](funding-transaction.png)

### Sequence IDs

In other layer-2 implementations like Lightning Network of Bitcoin, if one of the parties tries to cheat and close the channel with an outdated balance, the other peer has the right to penalize them and get all the funds in the channel. Bosagora solves this using an improved Eltoo implementation with sequence IDs. Each trigger and update transaction has a lock script like the one below.

OP.IF

    <update-tx-key> <min-sequence-id> OP.VERIFY_SEQ_SIG 
OP.ELSE

    <relative-timelock> OP.VERIFY_UNLOCK_AGE
    <settlement-tx-key> <sequence-id> OP.VERIFY_SEQ_SIG
    
OP.ENDIF

The IF branch ensures that only an update transaction with newer sequence id can consume the given trigger/update transaction, without any timelock. The ELSE branch allows the corresponding settlement transaction to consume the trigger/update transaction after the timelock expires. With each update/trigger transaction, a new timelock starts, allowing the other party to override it with a newer update transaction if they have one.

This scheme ensures that nodes have enough time to overrule any non-collaborative channel close attempt with a newer update transaction. In the current version of the Bosagora Flash Layer, timelocks are set for 16 blocks, which on average gives little less than 3 hours of time for nodes to react to a non-collaborative channel close.

