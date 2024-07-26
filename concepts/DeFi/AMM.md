---
title: AMM
description: 자동화 마켓 메이커(AMM)의 개념, 작동 원리, 및 주요 유형들을 다룹니다.
aliases: [amm, automated market maker]
tags: [finance, amm, exchange]
date: 2024-07-22
---

# AMM (Automated Market Maker)

- DeFi 혁신 사례 중 가장 영향력 있는 기술 중 하나는 자율적으로 기능하는 마켓 메이커(Automated Market Maker, AMM) 방식의 탈중앙화 거래소(DEX) 입니다. AMM 덕분에 온체인에서 다양한 토큰에 대한 유동성이 제공되어 DEX에서의 거래가 가능해졌습니다.

## CPMM (Constant Product Market Maker)

- CPMM은 `x*y=k` 공식을 바탕으로 각 토큰의 수량(유동성)에 따라 두 토큰의 가격대를 형성하는 것입니다. K라는 상품을 일정하게 유지하기 위해서는 X 토큰의 공급이 늘어나면 Y 토큰의 공급은 줄어들 수밖에 없고 반대의 경우도 마찬가지입니다. 이를 고려하면 늘 유동성이 있는 쌍곡선이 이뤄지지만 양 끝에서 무한대에 수렴하는 가격 고점에 다가가게 됩니다.

![[Pasted image 20240707160133.png]]

## CSMM (Constant Sum Market Maker)

- 제로 슬리피지 거래에 최적화되어있지만 무한 유동성을 제공해 주진 않습니다. CSMM은 `x + y = k` 공식을 따르기 때문에 그래프로 보면 일직선의 형태를 띕니다. 이로 인해 차익거래자들이 토큰의 오프체인 레퍼런스 가격이 1:1이 아니면 토큰 하나의 준비금을 완전히 바닥낼 수 있습니다. 이런 상황이 발생하면 한 쪽의 유동성 풀을 파괴시킬 수 있으며 유동성 공급자들에게 손실을 입혀 거래하는 사람들을 위한 유동성을 남기지 못할 수 있습니다. 이러한 이유 때문에 CSMM는 잘 사용되지 않는 AMM 모델입니다.

![[Pasted image 20240707160226.png]]

## CMMM (Constant Mean Market Maker)

- AMM에서 두 가지 이상의 토큰을 보유할 수 있고 기존 50/50 비율 외 다른 가중치 설정도 가능합니다. 이 모델에서는 각 보유금의 가중치의 산술적 평균값(mean)이 일정하게 유지됩니다. 세 가지 자산에 대한 유동성 풀에 대한 공식은 다음과 같습니다. (x_y_z)^(⅓)=k. 이를 통해 다양한 다른 자산이 풀에 노출이 될 수 있고 풀 내 자산간 스와프가 가능합니다.

## CFMM (Constant Function Market Maker)

- AMM 기반 유동성이 진화하면서 더 진보된 형태의 **하이브리드 CFMM**가 등장하게 되었고 유동성 공급자들에게는 리스크 노출 조정 또는 거래자들에게는 더 낮은 슬리피지를 위한 것과 같은 특정 행동을 달성하기 위해 몇 가지 기능 및 파라미터를 결합합니다.
- 예를 들어 커브 AMM은 CPMM과 CSMM을 결합해 더 촘촘하게 유동성을 제공해 거래 시 슬리피지를 낮추려 노력합니다. 결과적으로 파랑색 라인인 직각쌍곡선이 대부분의 거래시에는 직선의 형태를 띄고 대규모 거래 발생 시에만 급격한 커브 형태 보이고 있습니다.
- ![[Pasted image 20240707160242.png]]

| Feature              | CSMM                                                       | CMMM | CPMM (e.g., Uniswap V2)                                       | CFMM (e.g., Uniswap V3)                                             |
| -------------------- | ---------------------------------------------------------- | ---- | ------------------------------------------------------------- | ------------------------------------------------------------------- |
| Definition           | AMM where the sum of assets in the pool remains constant.  |      | AMM where the product of assets in the pool remains constant. | AMM where the pricing function is a curve (not necessarily linear). |
| Example              | Balancer                                                   |      | Uniswap V2                                                    | Uniswap V3                                                          |
| Formula/Equation     | Prices determined by asset weights.                        |      | Prices determined by x \* y = k                               | Customizable curve equation.                                        |
| Assets Handling      | Allows for pools with multiple assets and varied weights.  |      | Pools are paired with one token in each pair.                 | Pools can handle multiple tokens and liquidity ranges.              |
| Liquidity Management | Static balance of assets with varying weights.             |      | Dynamic balance maintained by traders.                        | Adjusts liquidity across a curve.                                   |
| Use Case             | Complex liquidity management with different asset weights. |      | Simple token swapping and liquidity provision.                | Fine-tuned liquidity provision with customizable curves.            |
| Flexibility          | More control over asset allocation.                        |      | Less flexible in asset weighting.                             | Highly customizable liquidity management.                           |
| Popularity           | Less common, used in specific AMM designs.                 |      | One of the most popular AMM designs.                          | Advanced and newer concept in AMM design.                           |
| Example Projects     | Balancer, SnowSwap                                         |      | Uniswap V2                                                    | Uniswap V3                                                          |

### References

- [AMM 설명](https://en.wikipedia.org/wiki/Automated_market_maker)
- [AMM의 작동 원리](https://www.investopedia.com/terms/a/automated-market-maker.asp)
- [Gemini의 AMM 설명](https://www.gemini.com/cryptopedia/search?query=amm)
- [Automated Market Maker 설명](https://blog.chain.link/automated-market-maker-amm-korean/)

### Related Keywords

- [[Finance]]
- [[Exchange]]
- [[Liquidity Pool]]
- [[Algorithmic Trading]]
- [[Decentralization]]
