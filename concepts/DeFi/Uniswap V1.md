---
title: Uniswap V1
description: Uniswap V1의 개념, 작동 원리, 및 중요성을 다룹니다.
aliases: [uniswap v1, uniswap]
tags: [finance, uniswap, dex, amm]
date: 2024-07-22
---

## Uniswap V1

### Summary

`Uniswap V1`은 탈중앙화 거래소(DEX)로, AMM 방식을 사용하여 거래를 중개합니다.

### Description

`Uniswap V1`은 탈중앙화 거래소(DEX)로, 자동화 마켓 메이커(AMM) 방식을 사용하여 거래를 중개합니다. Uniswap V1은 이더리움 네트워크 위에서 작동하며, 유동성 풀을 통해 거래 쌍 간의 유동성을 제공합니다.

### Uniswap V1의 기술적 구현

1. **AMM 방식**: Uniswap V1은 x\*y=k 공식을 기반으로 하는 CPMM(Constant Product Market Maker) 방식을 사용합니다. 이는 두 자산의 곱이 일정하게 유지되는 방식으로, 유동성 공급자가 예치한 자산의 양에 따라 가격이 결정됩니다.
2. **유동성 풀**: 유동성 풀은 거래 쌍의 유동성을 제공하며, 각 거래는 풀의 상태를 업데이트합니다. 유동성 공급자는 자산을 예치하고 거래 수수료를 통해 보상을 받습니다.
3. **스마트 계약**: Uniswap V1은 이더리움 스마트 계약을 통해 운영되며, 모든 거래와 유동성 풀 관리는 스마트 계약에 의해 자동으로 이루어집니다.

### References

- [Uniswap V1 설명](https://en.wikipedia.org/wiki/Uniswap)
- [Uniswap V1의 작동 원리](https://www.investopedia.com/terms/u/uniswap.asp)
- [Gemini의 Uniswap V1 설명](https://www.gemini.com/cryptopedia/search?query=uniswap)

### Related Keywords

- [[Finance]]
- [[DeFi]]
- [[DEX]]
- [[AMM]]
- [[Liquidity Pool]]
- [[Ethereum]]
