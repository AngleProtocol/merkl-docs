---
description: Everything you need to know about concentrated liquidity campaigns on Uniswap V4
---

# 🦄 Concentrated Liquidity Campaigns on Uniswap V4

## Overview

Uniswap V4 introduces hooks and singleton architecture, unlocking powerful new possibilities for liquidity management.
Building on this, Merkl’s incentive layer enables providers to reward Liquidity Providers (LPs) not just for participating, but for how they provide liquidity.
Unlike traditional incentives that focus purely on deposit size, Merkl’s approach evaluates LP behavior—emphasizing precision, strategy, and capital efficiency.

With Uniswap V4 Hooks, LPs can implement automated, dynamic strategies to concentrate liquidity within specific price ranges, boosting both capital efficiency and fee generation. Merkl’s incentive framework is designed to complement this flexibility, ensuring that LPs who provide liquidity in optimal, high-impact ranges are rewarded more generously.

## 🔢 Reward Formula

When an incentive provider creates a concentrated liquidity campaign, they specify:

- The Uniswap V4 pool to incentivize (using its `poolId`)
- A time period for rewards
- A set of incentive parameters that govern how rewards are allocated

The main incentive parameters are the following:

- **Liquidity Contribution**: This measures the time weighted contribution of a position to the overall liquidity in the pool.
  Positions that are more concentrated around the active tick contribute more liquidity—given equal TVL—than those spread out. Unlike in Merkl’s Uniswap V3 implementation, this parameter does not consider the fees earned by a position. For example, a position might earn high fees during a short burst of volume, but if it wasn’t consistently concentrated over the entire reward period, it won’t receive substantial rewards. What matters here is sustained concentration over time, not short-term volume or swap activity.
- **Token 0 Holding**: The share of token 0 held by a position relative to the total token 0 in the pool.
- **Token 1 Holding**: The share of token 1 held by a position relative to the total token 1 in the pool.

Each of these parameters is assigned a weight (`w_liquidity`, `w_0`, `w_1`) by the incentive provider. Conceptually, the total reward budget is divided among these three components, with each parameter determining how its portion is distributed.

This structure offers fine-grained control for customizing incentives to align with specific liquidity goals.

{% hint style="info" %}
Building on its Uniswap V3 foundation, Merkl's integration with Uniswap V4 brings major enhancements—delivering a more performant, stable, and JIT attack-resistant reward mechanism. The system enables precise measurement of liquidity over time, resulting in more deterministic and reliable attribution of LP contributions while minimizing the influence of short-term volatility.
{% endhint %}

### 📊 Example Calculation

If incentive weights are set as:

- Liquidity Contribution = 50%
- Token 0 = 25%
- Token 1 = 25%

Then:

- A user representing 40% of the total liquidity in the pool (and hereby earning ~40% of the fees accruing to the pool) receives 20% of total rewards (40% × 50%).
- A user holding 30% of Token 0 receives 7.5% of total rewards (30% × 25%).
- A user holding 20% of Token 1 receives 5% of total rewards (20% × 25%).

## 🎯 Reward Strategy Examples

Incentive providers can fine-tune parameters to match their objectives:

- **Encouraging Deep Liquidity** – Increasing the weight of liquidity contribution rewards encourages deep liquidity in high-volume trading ranges.
- **Stablecoin Peg Protection** – Allocating more weight to Token 0 (e.g., USDC) rewards LPs who provide more stable liquidity.
- **Tight Range Incentives** – Using Uniswap V4 Hooks, LPs can implement automated rebalancing strategies to optimize rewards for tight-range liquidity.

## 🚀 Sampling and Anti-DOS

Uniswap V4 incentive distribution leverages Merkl's `LogProcessor` algorithm, which continuously tracks the pool's state at any given moment. As a result, each campaign calculates an average value for each position, ensuring that users are rewarded based on both their position and the duration they keep it open.

## ⛔ Out of Range Liquidity

Incentive providers can choose whether to reward out-of-range liquidity. By default, only in-range positions receive rewards to ensure active liquidity provision.

## Providing Liquidity for Uniswap V4 Campaigns

### Providing Liquidity Directly Or Through ALMs

To maximize your rewards in Uniswap V4 campaigns:

- Choose optimal price ranges: Narrower ranges offer higher potential rewards but carry risks—such as going out of range or increased impermanent loss. Full-range or out-of-range positions typically earn significantly less, especially in campaigns where the liquidity contribution parameter carries a high weight.
- Adjust token balances strategically: Check the campaign’s weighting for Token 0 and Token 1. Aligning your position accordingly can boost your rewards.
- Consider automated liquidity management: If supported by the pool, tools like Gamma, Arrakis, or Beefy can help maintain your position within optimal ranges through automated rebalancing.

Most importantly, always review the specific incentive parameters of the campaigns running on the pools you’re participating in—these directly influence how rewards are distributed.

### Restrictions Rules

Restriction rules are optional and must be set by the campaign creator.

#### Tick Price Limits for Reward Eligibility

To qualify for rewards under a campaign with this rule, a liquidity position must meet both of the following conditions:
	•	(if set by the campaign creator) Upper Tick Price must be below the specified `threshold_0`:
Upper Tick Price < `threshold_0`
	• (Resp.) Lower Tick Price must be above the specified threshold:
Lower Tick Price > `threshold_1`

Here, price refers to the ratio of token 1 to token 0. For example, if token 1 is ETH and token 0 is BTC, the price reflects the ETH/BTC exchange rate.

This rule ensures that only positions providing liquidity within a targeted price range are eligible for rewards. Positions exceeding these limits on either side are not eligible.

_Example_:

Suppose the thresholds are:
	•	Upper Price Threshold = 1,050 token 1 per token 0
	•	Lower Price Threshold = 950 token 1 per token 0

Then:
	•	A position from 960 to 1,040 qualifies (upper < 1,050, lower > 950).
	•	A position from 940 to 1,020 does not qualify (lower < 950).
	•	A position from 990 to 1,080 does not qualify (upper > 1,050).

This setup encourages liquidity within a narrow, strategic price corridor, optimizing depth and efficiency around critical price levels.

Note: The thresholds are set independently — it is possible to define a limit for the upper tick price without setting one for the lower tick price (and vice versa).

### How are APRs calculated?

The APRs shown on the Merkl interface for Uniswap V4 campaigns represent average values. They are calculated by dividing the annualized reward emissions for a pool by its current TVL.

However, actual APRs vary per position. A position may earn more or less than the displayed average depending on factors like:

- How concentrated the liquidity is compared to others
- Whether the position holds more Token 0 or Token 1, and how that aligns with the campaign’s parameters
