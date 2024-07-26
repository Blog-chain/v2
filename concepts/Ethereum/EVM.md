---
title: EVM
description: 이더리움 네트워크에서 이더리움 가상 머신(EVM)의 개념, 역할, 및 중요성을 다룹니다.
aliases: [evm, ethereum virtual machine, evm ethereum]
tags: [technology, blockchain, ethereum, evm]
date: 2024-07-22
---

## EVM

### Summary

`EVM (Ethereum Virtual Machine)`은 이더리움 네트워크 내에서 트랜잭션을 수행하기 위한 런타임 환경입니다.

### Description

`EVM (Ethereum Virtual Machine)`은 이더리움 네트워크 내에서 트랜잭션을 수행하기 위한 런타임 환경으로, 이더리움 네트워크에서 스마트 계약을 실행하고 블록체인 상의 모든 트랜잭션을 처리하는 역할을 합니다. EVM의 주요 특징은 다음과 같습니다:

EVM은 Solidity 코드 기반의 스마트 계약과 상호작용하며, 이더리움 노드와의 상호작용을 통해 트랜잭션을 처리합니다. 스마트 계약은 Solidity 언어로 작성되며, 컴파일러를 통해 EVM 바이트코드로 변환됩니다. 이 바이트코드는 EVM에서 실행됩니다.

### EVM의 구조와 동작 원리

1. **스택 기반 구조**: EVM은 스택 기반으로 동작하며, 연산은 주로 스택을 통해 수행됩니다. 각 연산은 스택에서 피연산자를 가져오고, 결과를 다시 스택에 저장합니다.
2. **저장소(Storage)**: 컨트랙트 주소와 연결된 영구적인 저장소로, 상태 변수를 저장합니다. 이는 블록체인에 영구적으로 기록됩니다.
3. **메모리(Memory)**: 트랜잭션 실행 중에만 유효한 일시적인 저장 공간으로, 함수 호출 간의 데이터를 저장합니다.
4. **스택(Stack)**: 연산을 수행하기 위해 데이터를 저장하는 LIFO 구조로, 최대 깊이는 1024입니다.
5. **1 word와 주소의 바이트 스펙**: EVM에서 1 word는 256비트(32바이트)이며, 주소는 160비트(20바이트)로 구성됩니다.

### EVM과 스마트 계약의 상호작용

Solidity로 작성된 스마트 계약은 컴파일러에 의해 EVM 바이트코드로 변환됩니다. 이 바이트코드는 EVM에서 실행되며, 트랜잭션에 의해 호출됩니다. 스마트 계약은 EVM의 저장소, 메모리, 스택을 사용하여 상태 변수를 관리하고, 함수 호출을 처리합니다.

### EVM과 노드의 상호작용

이더리움 네트워크의 각 노드는 EVM을 실행하여 트랜잭션을 처리하고 블록을 생성합니다. 노드는 트랜잭션을 수신하고, EVM을 통해 이를 실행하며, 결과를 블록체인에 기록합니다.

### References

- [EVM 설명](https://en.wikipedia.org/wiki/Ethereum#Virtual_machine)
- [EVM의 작동 원리](https://ethereum.org/en/developers/docs/evm/)
- [Gemini의 EVM 설명](https://www.gemini.com/cryptopedia/search?query=evm)

### Related Keywords

- [[Ethereum]]
- [[Smart Contract]]
- [[Solidity]]
- [[Blockchain]]
- [[Node]]
- [[Transaction]]
