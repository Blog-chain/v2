---
title: Block
description: 블록체인에서 블록의 개념, 구조, 및 역할을 다룹니다.
aliases: [block, blockchain block]
tags: [technology, blockchain, block]
date: 2024-07-22
---

## Block

### Summary

[https://bitcoinwiki.org/wiki/block](https://bitcoinwiki.org/wiki/block)

a permanently recorded file at Bitcoin containing information on occurred transactions. Block is the record of the every recent transaction or its part that has not been recorded in the previous blocks. Practically in all cases blocks are added to the end of the chain, which contains all transactions and is called blockchain. When a block is added to the end of the chain, it cannot be changed. Each block contains information about everything that happened in previous blocks before it was created.

---

[https://developer.bitcoin.org/glossary.html#term-Block](https://developer.bitcoin.org/glossary.html#term-Block)

One or more transactions prefaced by a block header and protected by proof of work. Blocks are the data stored on the block chain.

---


[https://ethereum.org/en/developers/docs/blocks/](https://ethereum.org/en/developers/docs/blocks/)

Blocks are batches of transactions with a hash of the previous block in the chain. This links blocks together (in a chain) because hashes are cryptographically derived from the block data. This prevents fraud, because one change in any block in history would invalidate all the following blocks as all subsequent hashes would change and everyone running the blockchain would notice.                                                                                               
 New blocks are added to Ethereum every 12 seconds (unless a block proposer misses its turn), so a near-constant stream of data gets added to block explorers.
 
 
 Blocks contain a lot of important data that you may find useful:    
### Standard data

[https://ethereum.org/en/developers/docs/data-and-analytics/block-explorers/#execution-data](https://ethereum.org/en/developers/docs/data-and-analytics/block-explorers/#execution-data) |

- Block height
  - The block number and length of the blockchain (in blocks) on creation of the current block
- Timestamp
  - The time at which a block was proposed
- Transactions
  - The number of transactions included within the block
- Fee recipient
  - The address that received gas fee tips from transactions
- Block Reward
  - The amount of ETH awarded to the validator who proposed the block
- Size
  - The size of the data within the block (measured in bytes)
- Gas used
  - The total units of gas used by the transactions in the block
- Gas limit
  - The total gas limits set by the transactions in the block
- Base fee per gas
  - The minimum multiplier required for a transaction to be included in a block
- Burnt fees
  - How much ETH is burned in the block
- Extra data
  - Any extra data the miner has included in the block

### Advanced data

- Hash
  - The cryptographic hash that represents the block header (the unique identifier of the block)
- Parent hash
  - The hash of the block that came before the current block
- StateRoot
  - The root hash of Merkle trie which stores the entire state of the system

[https://ethereum.org/en/developers/docs/data-and-analytics/block-explorers/#execution-data](https://ethereum.org/en/developers/docs/data-and-analytics/block-explorers/#execution-data) |

### References

- [블록의 개념과 역할](https://bitcoinwiki.org/wiki/block)
- [Ethereum 블록의 구조](https://ethereum.org/en/developers/docs/blocks/)
- [블록 헤더의 구성 요소](https://www.investopedia.com/terms/b/block-bitcoin-block.asp)
- [블록체인의 불변성 원리](https://www.investopedia.com/news/what-genesis-block-bitcoin-terms/)

### Related Keywords

- [[Blockchain]]
- [[Timestamp]]
- [[Node]]
- [[Merkle Tree]]
- [[Bitcoin]]
- [[Ethereum]]
