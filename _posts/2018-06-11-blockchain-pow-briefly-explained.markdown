---
layout: post
title: Proof-of-Work, Explained!
date: 2018-06-11 121:02:20 +0300
description: 这个帖子的目的主要是解释一下区块链技术中那个非常关键的技术“Proof-of-Work“. # Add post description (optional)
img: blockchain-pow-1.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Blockchain,Bitcoin,Tech]
---
### * _What is Proof-of-Work?_
Proof-of-Work, or PoW, is the original consensus algorithm in a Blockchain network.

In Blockchain, this algorithm is used to confirm transactions and produce new blocks to the chain. With PoW, miners compete against each other to complete transactions on the network and get rewarded.

In a network users send each other digital tokens. A decentralized ledger gathers all the transactions into blocks. However, care should be taken to confirm the transactions and arrange blocks.

This responsibility bears on special nodes called miners, and a process is called mining.

The main working principles are a complicated mathematical puzzle and a possibility to easily prove the solution.
![Chain of Blocks]({{site.baseurl}}/assets/img/blockchain-pow-confirmation.jpg)

### * _What do you mean a “mathematical puzzle?”_
It’s an issue that requires a lot of computational power to solve.

There are a lot of them, for instance:

* hash function, or how to find the input knowing the output.
* integer factorization, in other words, how to present a number as a multiplication of two other numbers.
* guided tour puzzle protocol. If the server suspects a DoS attack, it requires a calculation of hash functions, for some nodes in a defined order. In this case, it’s a ‘how to find a chain of hash function values’ problem.

The answer to the PoW problem or mathematical equation is called hash.

As the network is growing, it is facing more and more difficulties. The algorithms need more and more hash power to solve. So, the complexity of the task is a sensitive issue.

### *_How come?_
Accurate work and speed of Blockchain system depend on it.

But the problem shouldn’t be too complicated. If it is, the block generation takes a lot of time. The transactions are stuck without execution and as a result, the workflow hangs for some time. If the problem cannot be solved in a definite time frame, block generation will be kind of a miracle.

But if the problem is too easy it is prone to vulnerabilities, DoS attacks and spam.

The solution needs to be easily checked. Otherwise, not all nodes are capable of analyzing if the calculations are correct.

Then you will have to trust other nodes and it violates one of the most important features of Blockchain - transparency.

### * _How is this algorithm implemented in Blockchain?_
Miners solve the puzzle, form the new block and confirm the transactions.

How complex a puzzle is depends on the number of users, the current power and the network load. The hash of each block contains the hash of the previous block, which increases security and prevents any block violation.
![Chain of Blocks]({{site.baseurl}}/assets/img/blockchain-pow-blocks.jpeg)

If a miner manages to solve the puzzle, the new block is formed. The transactions are placed in this block and considered confirmed.
![Chain of Blocks]({{site.baseurl}}/assets/img/blockchain-pow-puzzle.jpeg)

### * _And where PoW is usually implemented?_
Proof-of-Work is used in a lot of cryptocurrencies.

The most famous application of PoW is Bitcoin. It was Bitcoin that laid the foundation for this type of consensus. The puzzle is Hashcash. This algorithm allows changing the complexity of a puzzle based on the total power of the network. The average time of block formation is 10 minutes. Bitcoin-based cryptocurrencies, such as Litecoin, have the similar system.

Another large project with PoW is Ethereum. Given that almost three of four projects are implemented on Ethereum platform, it’s safe to say that the majority of Blockchain applications use PoW consensus model.

### * _Why use a PoW consensus algorithm in the first place?_
The main benefits are the anti-DoS attacks defense and low impact of stake on mining possibilities.

Defense from DoS attacks.  PoW imposes some limits on actions in the network. They need a lot of efforts to be executed. Efficient attack requires a lot of computational power and a lot of time to do the calculations. Therefore, the attack is possible but kind of useless since the costs are too high.

Mining possibilities. It doesn’t matter how much money you have in your wallet. What matters is to have large computational power to solve the puzzles and form new blocks. Thus, the holders of huge amounts of money are not in charge of making decisions for the entire network.

### * _Any flaws in the PoW consensus algorithm?_
The main disadvantages are huge expenditures, “uselessness” of computations and 51 percent attack.

Huge expenditures. Mining requires highly specialized computer hardware to run the complicated algorithms. The costs are unmanageable Mining is becoming available only for special mining pools. These specialized machines consume large amounts of power to run that increase costs. Large costs threaten centralization of the system since it benefits. It is easy to see in the case of Bitcoin.
![Chain of Blocks]({{site.baseurl}}/assets/img/blockchain-pow-flaws.png)

“Uselessness” of computations. Miners do a lot of work to generate blocks and consume a lot of power. However, their calculations are not applicable anywhere else. They guarantee the security of the network but cannot be applied to business, science or any other field.

### * _51% attack, what are you talking about?_
A 51 percent attack, or majority attack, is a case when a user or a group of users control the majority of mining power.

The attackers get enough power to control most events in the network.

They can monopolize generating new blocks and receive rewards since they’re able to prevent other miners from completing blocks.

They can reverse transactions.

Let’s assume Alice sent Bob some money using Blockchain. Alice is involved in the 51 percent attack case, Bob is not. This transaction is placed in the block. But the attackers don’t let the money be transferred. There is a fork happening in the chain.
![Chain of Blocks]({{site.baseurl}}/assets/img/blockchain-pow-attacks.jpg)

Further, miners join one of the branches. And as they have the majority of the computational power, their chain contains more blocks.
![Chain of Blocks]({{site.baseurl}}/assets/img/blockchain-pow-attack-2.jpg)

In the network, a branch that lasts longer remains, and shorter one is rejected. So the transaction between Alice and Bob does not take place. Bob doesn’t receive the money.
![Chain of Blocks]({{site.baseurl}}/assets/img/blockchain-pow-attack-3.jpg)

Following these steps, the attackers can reverse transactions.

51 percent attack is not a profitable option. It requires an enormous amount of mining power. And once it gets public exposure, the network is considered compromised, which leads to the outflow of users. This will inevitably move the cryptocurrency price down. All consequently, the funds lose their value.
