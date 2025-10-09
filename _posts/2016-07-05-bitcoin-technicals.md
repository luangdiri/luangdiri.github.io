---
title: "Bitcoin Technicals"
date: 2016-07-05 12:57:35
updated_at: 2020-05-24 13:08:29
tags:
- blockchain
---

### Everything
https://www.reddit.com/r/Bitcoin/comments/4t9pq6/what_are_some_good_books_for_cryptography_and/

### Bitcoin programming
http://chimera.labs.oreilly.com/books/1234000001802/index.html

### Controlled supply:
https://en.bitcoin.it/wiki/Controlled_supply

### Understanding bitcoin (core) source code:
https://bitcointalk.org/index.php?topic=41718.0
http://bitcoin.stackexchange.com/questions/41692/how-to-understand-bitcoin-source-code
http://bitcoin.stackexchange.com/questions/37684/where-to-find-help-understanding-bitcoins-source-code-in-c

### Protocol
https://en.bitcoin.it/wiki/Protocol_documentation

### Hash functions and shit
https://www.gwern.net/Bitcoin%20is%20Worse%20is%20Better

### Steemit paper (for content reference)
https://steem.io/SteemWhitePaper.pdf

### Segwit
https://bitcoincore.org/en/2016/06/24/segwit-next-steps/
https://en.bitcoin.it/wiki/Segregated_Witness

### Books and journals
http://richtopia.com/emerging-technologies/top-blockchain-books-whitepapers

### History
https://queue.acm.org/detail.cfm?id=3136559

### CONCEPTUALIZING BLOCKCHAINS:  CHARACTERISTICS & APPLICATIONS 
https://arxiv.org/ftp/arxiv/papers/1806/1806.03693.pdf

### Script on txes
https://medium.com/@ismailakkila/my-notes-on-bitcoin-transactions-part-1-a4edc871f705
https://medium.com/@ismailakkila/my-notes-on-bitcoin-transactions-part-2-c9d39d5ae326

### Script
https://bitcore.io/api/lib/script
https://en.bitcoin.it/wiki/Script
http://learnmeabitcoin.com/glossary/script

### Bitcoin technical details
https://www.cs.tut.fi/kurssit/ELT-53206/lecture11.pdf
https://en.bitcoin.it/wiki/Protocol_documentation
https://medium.com/s/story/how-does-the-blockchain-work-98c8cd01d2ae
https://www.ashurst.com/en/news-and-insights/insights/blockchain-101/
https://www.doc.ic.ac.uk/~ma7614/topics_website/tech.html
https://marmelab.com/blog/2016/04/28/blockchain-for-web-developers-the-theory.html
http://learnmeabitcoin.com/glossary/coinbase-transaction - coinbase tx
http://royalforkblog.github.io/2014/08/11/graphical-address-generator/#brettonwoods_7_1_1944 - address creation
https://en.bitcoin.it/w/images/en/e/e1/TxBinaryMap.png - transaction structure map

### encryption algorithms

- dsa (ecdsa predecessor)
- ecdsa
- secp256k1
- schnorr (multisig)

### Lightning network

https://lightning.network/lightning-network-summary.pdf
https://lightning.network/lightning-network-paper.pdf
https://bitcoinmagazine.com/articles/understanding-the-lightning-network-part-building-a-bidirectional-payment-channel-1464710791/
https://medium.com/@argongroup/bitcoin-lightning-network-7-things-you-should-know-604ef687af5a
https://bitfalls.com/2018/04/15/lightning-network-work/
https://btcmanager.com/lightning-how-payment-channels-build-up-a-network/

### Chain reorganization
binance hack cz calls for [chain reorg](https://cointelegraph.com/news/binance-ceo-addresses-concerns-live-after-40-mln-btc-hack-rejects-blockchain-reorg-idea) event.

https://boards.4channel.org/biz/thread/13557877/lmao-cz-trusts-talking-with-jihan-on-btc-over

> The transaction happened on block 575012
> BTC is on block 575090
> You need to rewrite 78 blocks at a difficulty of 6,702,169,884,349.
> Every block that goes by, the harder it is.
> The 49.9% of other miners will be mining the actual chain which would increase the real chain's block count.
> So in order to make this "reorg" possible:
> You need to go back 78 blocks (as of this post, probably more if you're reading this later)
> then you have to take 50.1% of the hashing power away (why would miners agree to this? they can mine the actual btc chain and get the block reward but assume they do)
> meanwhile, the 49.9% of the miners are mining the real chain.
> So let me do the math for you. Right now it's 78 blocks so I'm going to use 78 blocks. Again, if you're reading this in the future it'll probably be more blocks and the numbers will be higher
> 78 blocks * 2 (because you only have 50% of the hashing power) * 10 minutes (time on average per block) gives you 1560 minutes or 26 hours. Now, that only brings you to block #575090. You need to account for the other 49% of miners mining the real chain while you're trying to catch up.
> You're literally asking 50% of the miners to mine nothing for a minimum of 26 hours.
> These miners aren't mining for free, they have electricity cost and other cost. To "bribe" these miners, you'll have to give them a lot more than $40 million.
> This is if the attack starts right now. If you start the attack tomorrow, it's 144 + 78 blocks you have to rewrite.

---

> So another analogy would be
> you're going 50.1 mph (bad miner)
> the other car is going 49.9 mph (good miner)
> Except the car that's going 49.9 mph has a 26 hour head start (because it's 78 blocks ahead)
> every 10 minutes, the car going 49.9 mph is getting more than $12.5 dollars for gas (block reward)
> you have to pay your own gas every 10 minutes
> The question is, who would you bet to win?
> Again, this is if you start the race right now. If you wait 24 hours, it'll be 4440 minutes which is 74 hours. The 49.9 mph car is getting a 74 hour head start if you wait just 24 hours.

---

> 26 hours of mining = 6 * 26 = 156 blocks missed. That's 1950 BTC just in block rewards but you also have to include transaction fees. Looking at the latest block right now, the miner got a total of 14.05735655 btc for mining that block so it's more than just 12.5 BTC.
> So 1950 BTC is the minimum you're going to lose. That also brings you to just block #575090, not the latest block. You'll have to mine for more than 26 hours (I just don't know how much because I don't know how fast the good chain will grow).
> On top of that, you're not mining BTC's chain but you're wasting all the electricity for 26 hours, you need to consider the electrical cost of 1 million+ antminers (I believe there's around 2 million miners).
> So instead of you earning 1950 BTC, you're losing money.
> Then on top of that, if someone just flips 0.1% hash rate (or add new hash rate) of just 0.2%, you will never catch up. If you're in a pool and your miner isn't mining the real BTC chain, you're going to switch pools and go to pool that's mining the real BTC chain. You literally have to own 1 million miners and mine at a loss for a long time (at minimum, 26 hours as of this post, longer if the blockchain is longer) just to re-org a transaction.
> This is much bigger than a $40 million gamble.

---

> I should also add, if you own 1 million miners (those things didn't come for free)
> Once this attack happens, BTC price will go down (dunno by how much) so you literally shot yourself in the foot because a 1 million miner investment + all the electricity contracts you have, you will lose a lot of money.
> So if you're a bad miner (50%) you lose money if the attack fails (electricity & time wasted when you could've mined the real BTC chain)
> you lose money if the attack succeeds (all your equipment loses value as btc price falls)
> It's a lose-lose situation for miners if they collude. This is why proof of work is important & BTC is the most immutable chain that exists. It's designed to be that way.

