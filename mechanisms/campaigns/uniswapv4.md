---
description: Everything you need to know about concentrated liquidity campaigns on Uniswap V4
---

# 🦄 Concentrated Liquidity Campaigns on Uniswap V4

## Overview

Uniswap V4 introduces hooks and singleton architecture, enabling advanced liquidity management strategies. Concentrated liquidity campaigns allow incentive providers to reward Liquidity Providers (LPs) for their participation in Uniswap V4 pools. Unlike traditional liquidity incentives, these campaigns reward LPs based on how they provide liquidity, not just their deposit amount.

With Uniswap V4’s Hooks, LPs can leverage automated strategies to optimize liquidity provision within specific price ranges, enhancing capital efficiency and maximizing fee earnings. The incentive structure ensures that LPs providing liquidity within optimal ranges earn higher rewards.

## 🔢 Reward Formula

When an incentive provider creates a concentrated liquidity campaign, they specify:

- The Uniswap V4 pool to incentivize (using its `poolId`)
- A time period for rewards
- Incentive parameters determining reward allocation

Rewards are assigned based on:

- **Fees Earned**: The fees generated by a position during the period, representing its contribution to liquidity.
- **Token 0 Holding**: The proportion of token 0 held by a position during swaps in the pool.
- **Token 1 Holding**: The proportion of token 1 held by a position during swaps in the pool.

Each factor is assigned a weight (`w_fees`, `w_0`, `w_1`), defined by the incentive provider. This allows flexibility in tailoring incentives to specific liquidity provision goals.

### 📊 Example Calculation

If incentive weights are set as:

- Fees = 50%
- Token 0 = 25%
- Token 1 = 25%

then:

- A user earning 40% of total pool fees receives 20% of total rewards (40% × 50%).
- A user holding 30% of Token 0 receives 7.5% of total rewards (30% × 25%).
- A user holding 20% of Token 1 receives 5% of total rewards (20% × 25%).

## 🎯 Reward Strategy Examples

Incentive providers can fine-tune parameters to match their objectives:

- **Encouraging Deep Liquidity** – Increasing the weight of fee-based rewards encourages deep liquidity in high-volume trading ranges.
- **Stablecoin Peg Protection** – Allocating more weight to Token 0 (e.g., USDC) rewards LPs who provide more stable liquidity.
- **Tight Range Incentives** – Using Uniswap V4 Hooks, LPs can implement automated rebalancing strategies to optimize rewards for tight-range liquidity.

## 🚀 Sampling and Anti-DOS

Uniswap V4 incentive distribution leverages Merkl's `LogProcessor` algorithm, which continuously tracks the pool's state at any given moment. As a result, each campaign calculates an average value for each position, ensuring that users are rewarded based on both their position and the duration they keep it open.

## ⛔ Out of Range Liquidity

Incentive providers can choose whether to reward out-of-range liquidity. By default, only in-range positions receive rewards to ensure active liquidity provision.

## 📈 APRs in Concentrated Liquidity Campaigns

Coming soon

## Providing Liquidity for Concentrated Liquidity Campaigns

### 1️⃣ Providing Liquidity Directly

To maximize rewards:

- Choose optimal price ranges – Tighter ranges increase earnings but risk going out of range.
- Adjust token balances – Consider campaign parameters for Token A and Token B.

### 2️⃣ Using Uniswap V4 Hooks

Uniswap V4’s Hooks enable automated liquidity management strategies:

- Hooks can rebalance liquidity dynamically to stay within incentivized ranges.
- Incentive providers can create custom Hooks to optimize LP rewards.

By leveraging Uniswap V4’s new architecture, LPs can enhance capital efficiency while maximizing their campaign earnings.
