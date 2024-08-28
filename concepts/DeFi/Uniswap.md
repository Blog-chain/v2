---
title: Uniswap V4
description: Uniswap V4의 개념
aliases:
  - uniswap
tags:
  - uniswap
  - dex
  - amm
  - defi
date: 2024-07-22
---

# Summary

### Description

## Uniswap V1

> **AMM의 도입 (CPMM과 유동성 공급자)**

- **AMM 방식**:
     - Uniswap V1은 `x * y = k (k는 상수)` 공식을 기반으로 하는 [[CPMM]](Constant Product Market Maker) 방식을 사용합니다.
     - 이 공식은 두 자산의 곱이 항상 일정하게 유지되도록 하여, 유동성 공급자가 예치한 자산의 양에 따라 가격이 결정됩니다.
     - 예를 들어, ETH와 DAI의 거래 쌍에서, ETH의 양이 증가하면 DAI의 양이 감소하여 가격이 조정됩니다.

- **슬리피지**:
     - 슬리피지는 거래 시 발생할 수 있는 가격 변동을 의미합니다.
     - 유동성 풀의 크기와 거래량에 따라 슬리피지의 정도가 달라집니다. 유동성이 클수록 슬리피지가 줄어들며, 이는 AMM 기반의 다양한 DEX들이 유동성을 확보하기 위해 혈안이 된 것과 큰 연관이 있습니다. 또한 큰 거래는 슬리피지를 증가시킬 수 있으며, 이는 거래자가 예상보다 불리한 가격에 거래를 완료하게 만들 수 있습니다.
     - 다음 예시에서 유동성 풀의 크기가 `[1000, 100]`인 시점에서 1000 DAI로 50 ETH만 교환할 수 있었지만, 유동성이 1000배 증가하여 `[1000*1000, 100 * 1000]`이 된 상황에서는 503 DAI 만으로 50 ETH를 교환할 수 있습니다.
      ![[Pasted image 20240812114113.png]]

- **유동성 풀 ([[Liquidity Pool]])**:
     - 유동성 풀은 특정 거래 쌍의 유동성을 제공하는 자산의 집합입니다.
     - 유동성 공급자는 ETH와 DAI 같은 자산을 풀에 예치하고, 그 대가로 거래 수수료의 일부를 보상으로 받습니다.
     - 각 거래를 통해 DEX는 유동성 풀의 상태를 업데이트하며, 풀의 자산 비율에 따라 AMM 알고리즘에 의해 가격이 자동으로 조정됩니다.

- **가스 최적화**:
     - 이더리움 네트워크에서 모든 트랜잭션은 [[Gas]]라는 수수료를 필요로 합니다.
     - Uniswap V1은 더 낮은 가스 비용으로 거래를 수행할 수 있도록 스마트 계약을 최적화 했습니다. 이는 Uniswap V1의 경쟁력이 되어, 보다 많은 유동성을 확보하는데 기여했습니다.
      ![[Pasted image 20240812113840.png]]

## Uniswap V2

> **사용성 개선 (ERC20 페어 지원, Flash Swap 지원)**

- **ERC20 Trade**: Uniswap V2는 ERC-20 토큰과 ERC-20 토큰 간의 직접 교환을 지원합니다. 이는 V1에서 반드시 Native ETH를 거쳐 교환해야 했던 불편함을 개선한 것입니다. V2부터는 Wrapped ETH(ERC 20)를 사용합ㅓㅠ니다.
- **UNI Token, Protocol Fee**: 거버넌스 토큰인 UNI 토큰을 발행하고, 거래 수수료의 일부를 프로토콜 수수료로 이전하여 UNI 토큰 보유자에게 지급할 수 있도록 하는 기능을 추가했습니다.
- **Flash Swap**: 일정 수수료를 포함한 금액을 동일한 트랜잭션 내에서 상환하는 조건으로 사용자가 유동성 풀에서 자산을 빌릴 수 있도록 지원합니다.

## Uniswap V3

> **집중화된 유동성 도입**

- **집중화된 유동성(Concentrated Liquidity)**: 이전까지는 유동성을 모든 가격대에 걸쳐서 공급했다면, V3부터는 특정 가격 범위에 대해서만 유동성을 공급할 수 있습니다. 이러한 가격의 범위를 설정할 때, 최소 단위인 틱(tick) 개념이 들어가게 됩니다. 자세한 내용은 [Uniswap V3 Core](https://uniswap.org/whitepaper-v3.pdf) 문서를 참고하세요.
  - **높은 자본 효율성(Higher Capital Efficiency)**: ETH 가격이 1,500~2,500 달러 내로 유지된다고 가정해봅시다. 이 때, 모든 가격대에 걸쳐 10,000 달러를 공급한 경우와 해당 범위에 1,200 달러의 유동성을 공급한 경우가 동일한 효과를 가지게 됩니다. 따라서, 주황 투자자는 남은 8,800 달러로 추가 투자가 가능하고, 자본 효율성을 높일 수 있습니다.

    ![[Pasted image 20240812123138.png]]

  - **고유한 유동성(Non-Fungible Liquidity)**: 이렇게 어떤 토큰 페어에 대해, 어떤 가격 범위로 어느 정도의 지분을 갖는지 ERC-721 NFT의 형태로 발급하여, NFT 판매 플랫폼을 통해 해당 권리에 대한 위임 또는 투자가 손쉽게 이루어질 수 있도록 합니다.
  - **유효한 유동성(Active Liquidity)**: 앞서 언급했듯 제한된 범위 내에서만 유동성을 공급한다면, 유동성 또한 가격 변동에 따라 변화하게 됩니다. 기존 CPMM 모델에서 상수였던 k가 active해졌다는 의미로 이해하면 되겠습니다.
- **유연한 수수료 체계(Flexible Fees)**: 기존의 수수료는 어떤 페어든 일괄적으로 0.30%가 거래마다 부과되었습니다. 하지만 스테이블 코인 간 교환에 대해서는 너무 높고, 교환 수요가 낮은 코인 간 교환에 대해서는 너무 낮다는 문제가 있었습니다. V3에서는 기본적으로 0.05%, 0.30%, 1.0% 세 종류의 다른 교환 수수료가 책정된 풀을 초기에 제공하고, Uniswap 운영진에 의해 추가로 다른 수수료 체계를 갖는 풀이 추가될 수 있다고 밝혔습니다. 유동성 공급자들은 자신의 선호에 따라 풀에 유동성을 공급하게 되며, 이는 `유동성 파편화`라는 내재적인 문제점을 가지고 있습니다. 또한 이용자들이 직접 최적의 교환 결과를 제공하는 풀을 선택하여 교환해야 한다는 UX 측면의 단점이 있지만, Uniswap이 자체 개발한 [Smart Order Router](https://github.com/Uniswap/smart-order-router)을 이용하면 가장 효율적인 스왑 경로를 찾을 수 있습니다.
  ![[Pasted image 20240812144958.png]]
- **라이센스 도입(License)**: 스시스왑의 [뱀파이어 어택(https://www.youtube.com/watch?v=UFjXwrCGuog)]과 같은 일련의 사건으로 V3는 오픈소스 라이센스가 아닌 Business Source Licence 1.1를 2년 간 유지할 예정입니다. 오픈소스에 기조를 두는 블록체인이라 외부의 반발은 많았지만, UNI 홀더들로부터는 지지를 받아 결과적으로 정당성을 얻게 되었습니다.

## Uniswap V4 | _in-progress_

- [# Uniswap V4 Deep Dive | 1 — 코드를 통해 알아보는 V4](https://medium.com/decipher-media/uniswap-v4-deep-dive-1-%EC%BD%94%EB%93%9C%EB%A5%BC-%ED%86%B5%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-v4-2881fa0358aa)
- [# Uniswap V4 Deep Dive | 2 — Uniswap V4의 현 상황과 전망](https://medium.com/decipher-media/uniswap-v4-deep-dive-2-uniswap-v4%EC%9D%98-%ED%98%84-%EC%83%81%ED%99%A9%EA%B3%BC-%EC%A0%84%EB%A7%9D-7205d4bcdabb)

## References

- [[UNISWAP SERIES] 1. 유니스왑 이해하기](https://medium.com/@aiden.p/uniswap-series-1-%EC%9C%A0%EB%8B%88%EC%8A%A4%EC%99%91-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-e321446623c7)
- - [# Uniswap V4란 무엇인가요?](https://academy.gopax.co.kr/uniswap-v4ran-mueosingayo/)
- [Uniswap V3 Core](https://uniswap.org/whitepaper-v3.pdf)
- https://uniswapv3book.com/
- [wiki/Uniswap](https://en.wikipedia.org/wiki/Uniswap)

## Related Keywords

- [[Finance]]
- [[DeFi]]
- [[DEX]]
- [[AMM]]
- [[Liquidity Pool]]
- [[Ethereum]]
