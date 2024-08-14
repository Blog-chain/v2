---
title: Node
description: 블록체인 네트워크에서 노드의 역할과 중요성을 다룹니다.
aliases:
  - 노드
tags:
  - blockchain
  - node
date: 2024-08-10
---

## Summary

`Node`는 블록체인 네트워크에서 트랜잭션을 검증하고 새로운 블록을 생성하며, 네트워크의 무결성과 보안을 유지하는 역할을 합니다.

## Description

`Node`는 블록체인 네트워크를 구성하는 기본 단위로, 네트워크의 트랜잭션을 검증하고 새로운 블록을 생성하는 역할을 합니다. 각 노드는 전체 블록체인의 사본을 유지하며, 네트워크의 무결성과 보안을 유지하는 데 중요한 역할을 합니다.

### Full Node _(in Blockchain)_

- store **the current and most recent** blockchain states
- participate in validating newly added blocks
- process transactions, execute smart contracts, and query/serve blockchain data
- 블록체인에서 이뤄진 모든 거래(원장) 정보를 보유한 노드를 의미합니다. 블록체인 네트워크에서 최초로 생성되는 제네시스 블록(genesis block)부터 마지막 블록까지 모든 데이터를 보유하고 있습니다. 따라서 스스로 본인의 거래를 검증할 수 있는 것이 장점입니다. 하지만 풀노드는 모든 데이터를 보유하고 있다 보니 많은 저장 공간이 필요한 것이 단점입니다. ~~(비트코인과 이더리움 등 체인 별로 정의가 다릅니다.)~~

### Light Node (Light Client) _(in Blockchain)_

- **only store block headers**, giving them access to minimal blockchain data
- interface with full nodes to get necessary data and validate information
- requires the least investment in hardware, running costs, and technical expertise
- 블록체인의 데이터 일부(주로 블록헤더의 중요 데이터)만 가지고 있습니다. 이에 따라 라이트노드는 거래는 가능하지만 스스로 검증하는 것은 불가능하기 때문에 풀노드에 거래 데이터를 요청하여 개별 검증을 진행합니다. 데이터를 요청하고 확인하는 데 시간이 필요한 것은 단점이지만 데이터 저장 공간이 많이 필요하지 않은 것이 장점입니다.

### Mining Node _(in Blockchain)_

- 채굴만을 위한 노드입니다. 채굴은 퍼즐을 푸는 작업증명(PoW, Proof of Work) 방식에서 사용되기 때문에 작업증명 방식을 채택한 블록체인에서 사용됩니다. 채굴노드는 컴퓨팅 파워 자원을 공유해 채굴에 참여하거나 스트라텀 프로토콜(stratum protocol) 및 경량지갑(light weight wallet)과 연결하여 채굴에 참여하기도 합니다.

### Super Node _(in Blockchain)_

- 위임지분증명(DPoS) 방식에서 주로 사용하고 있는 노드입니다. 위임지분증명 방식에서는 디지털 자산 보유량에 따라 투표권을 행사하고 주요 안건에 대해 의사결정을 대리할 수 있는 노드를 선출합니다. 이렇게 선출된 노드는 여러 노드로부터 권한을 위임받아 블록 생성이나 거래 확정 등의 역할을 수행하는 등 네트워크 유지에 중요한 역할을 수행하게 됩니다. 이처럼 일반 노드들을 대리할 수 있도록 선출된 노드를 슈퍼노드라고 하며, 대표노드라고 부르기도 합니다.

### Random Node _(in Blockchain)_

- 이중위임지분증명 방식에서 사용되며 전체 블록체인의 무결성을 유지하기 개발되었으며, 임의로 선택된 랜덤노드가 무작위 순서에 따라 블록을 생성하도록 설계되었습니다.
- 위임지분방식에서 한 단계 검증 절차가 추가된 것이 이중위임지분증명(DDPoS) 방식입니다. 디지털 자산 보유량에 따라 투표권을 행사해 대표 노드를 선정하고 이 대표 노드가 의사결정을 내리는 방식을 사용하면서 임의의 노드인 랜덤노드를 추가해 데이터 조작을 예방하는 시스템입니다.

### Master Node _(in Blockchain)_

- 풀노드를 보완하기 위해 고안된 노드입니다. 풀노드는 전체 블록체인의 검증, 생성 확인 등의 임무를 수행하지만 이에 따른 비용이 상당하기 때문에 채굴이라는 적극적인 행위를 하지 않으면 노드의 유지 비용을 충당하기가 쉽지 않습니다. 마스터노드는 풀노드의 이러한 단점을 보완하기 위해 설계된 것으로서, 별도의 채굴행위를 하지 않더라도 임무를 수행할 경우 보상을 받을 수 있도록 고안되었습니다. 대표적으로 대시코인, 해피코인 등의 프로젝트에서 사용됩니다.

### Archive Node _(in Blockchain)_

- An archive node is an instance of an Ethereum client configured to build an archive of all historical states. It is a useful tool for certain use cases but might be more tricky to run than a full node.
- Archive nodes can store the complete historical data for the blockchain and serve it on request.
- These are different from full nodes that only store the recent blockchain state and light nodes that primarily request data from full nodes.
- 아카이브 노드는 풀 노드에 저장된 검증에 필요한 데이터 뿐만 아니라 과거 블록의 상태와 같이 검증에 필요하지 않은 데이터까지 모두 저장한다. 일반적으로 과거 데이터를 요청하는 것을 필요로 하는 이해관계자에 의해 운영된다.
  - 특정 사용자의 활동을 추적
  - 하지만 풀 노드를 운영하는 것보다 훨씬 높은 기술적 전문성과 비용이 필요하고, 데이터가 손상되지 않도록 하는 것이 중요하다.

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

## References

- [블록체인 노드 유형, 장애 허용 시스템과 가정들](https://medium.com/curg/블록체인-노드-유형-장애-허용-시스템과-가정들-fe74226f0526)
- [Ethereum Archive Node](https://ethereum.org/en/developers/docs/nodes-and-clients/archive-nodes/)
- [Archive Nodes - Everything You Need to Know](https://www.alchemy.com/overviews/archive-nodes)
- [노드의 종류와 특징](https://m.upbitcare.com/academy/education/blockchain/66)

## Related Keywords

- [[Blockchain]]
- [[Block]]
- [[Timestamp]]
- [[Merkle Tree]]
- [[Bitcoin]]
- [[Ethereum]]
