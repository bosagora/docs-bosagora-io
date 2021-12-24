# Introduction

## Features

The ability to register multiple accounts is provided.  
The ability to use multiple outgoing accounts for one transaction is provided.  
The freezing function required to create a validator is provided.  
The ability to manage contacts is provided.  

## Monetary unit

The monetary unit of coins used in BOSAGORA blockchain is BOA.  
BOA's decimal place is 7.

## Components of accounts

The account consists of public key, secret key, and name.  
The public key is also used as the address of the account.  
The secret key is used when funds are transferred to another account.  
The secret key starts with S, its length consists of 56 alphabet and numbers.  
Example of a secret key) SDMCPWCYD4KAOOC3QNVSAWXYFBEORX7OADHARDSNJBKVIDO2M7GQ6DVX  
The public key starts with boa, its length consists 63 of lowercase alphabet and numbers.    
Example of a public key) boa1xraym009knpsxzguqz2mzjq78x0n06l9c2m3683346xd4vv3crq5qy7kx2y  

## Balance of accounts

The balance can be subdivided into the following.  

* Total balance : This is the sum of all UTXO held by the account.  
* Spendable balance : This is the sum of UTXO that can be spent on the account.  
* Frozen balance : The sum of the frozen UTXO.  
* Locked balance : This is the sum of UTXO that cannot be used for a certain period.  
There are two types of locked balances.  
The first is that the frozen UTXO is unfrozen, 
and the newly created UTXO is locked until 2016 new blocks are created. 
The second is the amount of UTXO used for the pending transaction. 
The UTXO consumed in the transaction is locked until the block containing it is externalized.

## Add an account

You can add multiple accounts. 
You can register a public key or a secret key when creating an account. 
When you attempt to withdraw funds from an account with only a public key registered, 
the web wallet presents a screen that allows you to enter the secret key. 
This also applies equally to the login screen. 
When you register a public key on the login screen, 
the balance and transactions history will be visible. 
If you register a secret key, all functions will be accessible to use.
If you inquire only about the transactions history or balance without depositing, 
the risk of the security being leaked can be prevented because there is no need to register the secret key.


## Manage accounts

When you close your browser or go to another site, all the secret keys registered in the wallet are removed.
Therefore, you must record and keep the secret key of the account created in this wallet.
If not, it should be noted that the loss of funds may occur.  
All the secret keys are removed when you reconnect,
but the public key that was included in the secret key remains.
Therefore, you can check your balance and transaction history without entering a separate key.
However, since all accounts have only public keys,
the web wallet presents you with a screen where you have to enter a secret key in case of withdrawal of funds.
You can complete the transfer of funds after entering the secret key you have recorded separately.  
<span style="color:red">**If you haven't backed up your secret key, you won't be able to use the funds transferred to the account. Please be extra careful.**</span>

## Transaction type

Transaction Type has three categories. It is **Payment**, **Freezing**, and **Coinbase**.   
Payment is the type of transaction used when funds are transferred.
Freezing is a type of transaction used when the funds required to create a validator are frozen.
Coinbase is a type of transaction used when node operation compensation and fee compensation are paid to node operators.

## Transaction transmission and completion

When you create a transaction in the wallet and send it to the node, the node checks whether the received transaction is valid.  
Transactions that pass the validation check wait until they are stored in the blockchain.
These transactions are called pending transactions.
Nodes then preferentially select transactions with high 
[transaction fee rates](#transaction-fee-rate) among pending transactions and store them in the blockchain.
Transactions stored in the blockchain are completed and cannot be changed.

## Transaction fee

In order for a transaction to be successfully stored in the blockchain, a transaction fee must be paid.
The wallet servers aggregate past transaction fees and suggest optimal fees according to the size of transaction data.  
The proposed fee options consist of three types: **High**, **Medium**, and **Low**.  
Transactions set at high fees are stored in the blockchain early, and transactions set at low fees are stored in the blockchain late.

## Transaction fee rate

Transaction fees increase in proportion to the size of the transaction.  
When this happens, the proportional constant is called the fee rate.  

## Payload and payload fee

BOSAGORA blockchain provides the ability to store the data you want to store in the blockchain.
The name of this storage is called payload. In order for data to be stored here, a fee must be paid.
The size of this fee increases exponentially with the size of the data, 
and the maximum size of the data is 1024Bytes. 
Transactions with payloads larger than the maximum size are rejected.

## Validator

Nodes select transactions, go through an agreement process, 
make them into one same block, and store them in the blockchain. 
Nodes that can participate in the process of consensus on block creation are called validators. 
In order to become a validator, more than 40,000 BOA of funds must be frozen in the validator's account.
