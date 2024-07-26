---
title: Node
description: 블록체인 네트워크에서 노드의 역할과 중요성을 다룹니다.
aliases: [node, blockchain node]
tags: [technology, blockchain, node]
date: 2024-07-22
---

## Node

### Summary

`Node`는 블록체인 네트워크에서 트랜잭션을 검증하고 새로운 블록을 생성하며, 네트워크의 무결성과 보안을 유지하는 역할을 합니다.

### Description

`Node`는 블록체인 네트워크를 구성하는 기본 단위로, 네트워크의 트랜잭션을 검증하고 새로운 블록을 생성하는 역할을 합니다. 각 노드는 전체 블록체인의 사본을 유지하며, 네트워크의 무결성과 보안을 유지하는 데 중요한 역할을 합니다.

노드는 주로 다음과 같은 유형으로 나눌 수 있습니다:

1. **풀 노드 (Full Node)**: 블록체인의 모든 트랜잭션 데이터를 저장하고 검증합니다. 풀 노드는 네트워크의 무결성을 보장하며, 새로운 블록이 추가될 때마다 이를 검증하고 다른 노드에 전파합니다. Bitcoin 네트워크에서의 풀 노드는 블록체인의 모든 데이터를 저장하고, 이를 통해 네트워크의 보안을 유지합니다.
2. **라이트 노드 (Light Node)**: 전체 블록체인의 데이터를 저장하지 않고, 필요한 데이터만 다운로드하여 검증하는 노드입니다. 라이트 노드는 빠른 속도로 트랜잭션을 검증할 수 있지만, 풀 노드에 비해 보안이 약할 수 있습니다. Ethereum 네트워크에서는 라이트 노드가 스마트 계약을 실행하기 위해 필요한 데이터를 빠르게 다운로드하여 검증합니다.
3. **아카이브 노드 (Archive Node)**: 블록체인의 모든 역사적 데이터를 저장하는 노드로, 특정 용도에 유용합니다. 아카이브 노드는 블록체인의 모든 상태를 저장하며, 이는 블록체인의 과거 데이터를 필요로 하는 작업에 필수적입니다.

노드는 블록체인 네트워크의 탈중앙화를 유지하고, 데이터를 안전하게 보호하며, 네트워크의 신뢰성을 보장하는 데 중요한 역할을 합니다.

### How?

- [Terms: **Consensus**](https://www.notion.so/Terms-Consensus-0c56ca02cff44a5b9b4562f370a9eccc?pvs=21)

### Full Node _(in Blockchain)_

- store **the current and most recent** blockchain states
- participate in validating newly added blocks
- process transactions, execute smart contracts, and query/serve blockchain data

### Light Node (Light Client) _(in Blockchain)_

- **only store block headers**, giving them access to minimal blockchain data
- interface with full nodes to get necessary data and validate information
- requires the least investment in hardware, running costs, and technical expertise

### Archive Node

- An archive node is an instance of an Ethereum client configured to build an archive of all historical states. It is a useful tool for certain use cases but might be more tricky to run than a full node.
- Archive nodes can store the complete historical data for the blockchain and serve it on request.
- These are different from full nodes that only store the recent blockchain state and light nodes that primarily request data from full nodes.

### Comparison of Full Node and Light Node

| Feature             | Full Node                                            | Light Node (Light Client)                                |
| ------------------- | ---------------------------------------------------- | -------------------------------------------------------- |
| Storage             | Stores the current and most recent blockchain states | Stores only block headers                                |
| Validation          | Participates in validating newly added blocks        | Interfaces with full nodes to validate information       |
| Processing          | Processes transactions, executes smart contracts     | Does not process transactions or execute smart contracts |
| Data Access         | Queries and serves comprehensive blockchain data     | Accesses minimal blockchain data via full nodes          |
| Hardware Investment | 📈▲ Requires significant investment in hardware      | 📉▼ Requires the least investment in hardware            |
| Running Costs       | 📈▲ Higher running costs                             | 📉▼ Lower running costs                                  |
| Technical Expertise | 📈▲ Requires substantial technical expertise         | 📉▼ Minimal technical expertise needed                   |

### References

- [Node 개념 및 역할](https://ethereum.org/en/glossary/#node)
- [Full Node와 Light Node 비교](https://developer.bitcoin.org/glossary.html#term-Node)
- [아카이브 노드의 역할](https://ethereum.org/en/developers/docs/nodes-and-clients/archive-nodes/)
- [노드의 종류](https://www.alchemy.com/overviews/archive-nodes)

### Related Keywords

- [[Blockchain]]
- [[Block]]
- [[Timestamp]]
- [[Merkle Tree]]
- [[Bitcoin]]
- [[Ethereum]]
