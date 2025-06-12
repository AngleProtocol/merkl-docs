---
description: Everything you need to know about concentrated liquidity campaigns on Merkl
---

# 🦄 Concentrated Liquidity Campaigns

## Overview

Merkl’s Concentrated Liquidity Campaigns allow incentive providers to reward Liquidity Providers (LPs) on concentrated liquidity AMMs like Uniswap V3, Quickswap, and Sushiswap. Unlike traditional one-size-fits-all incentive models, these campaigns reward LPs based on how they provide liquidity rather than just their total deposit amount.

Concentrated liquidity enables LPs to allocate capital within specific price ranges, enhancing capital efficiency and maximizing fee earnings. Merkl’s mechanism preserves these benefits, allowing LPs to earn more rewards when providing liquidity in narrower ranges.

Merkl also integrates Automated Liquidity Management solutions (ALMs), meaning LPs who provide liquidity through ALMs can still receive rewards. These ALMs function as forwarders, automatically redistributing rewards to depositors.

{% hint style="info" %}
The concentrated liquidity DEX and ALM solutions currently supported by the Merkl engine can be found [here](https://app.merkl.xyz/integrations). If you want to integrate your AMM or ALM within Merkl, please contact us on the [Merkl Discord by opening a BD ticket](https://www.google.com/url?q=https://discord.gg/jnYfrGxDbe\&sa=D\&source=docs\&ust=1714726869927696\&usg=AOvVaw1loOKjqz9IGEdpNjWsvrmD) to discuss the integration process.
{% endhint %}

## 🔢 Reward formula

When an incentive provider creates a Concentrated Liquidity Campaign, they specify:

* A pool to be incentivized
* A time period for rewards
* Incentive parameters, which determine how rewards are allocated

Merkl analyzes swaps within the pool during the incentive period and assigns rewards based on:

* **Fees Earned**: The fees earned by the position during the period, which represent the liquidity of the position used by the pool.
* **Token 0 Holding**: The amount of token 0 held by the position during swaps in the pool, compared to the total amount of token 0 in the pool.
* **Token 1 Holding**: The amount of token 1 held by the position during swaps in the pool, compared to the total amount of token 1 in the pool.

Each factor is assigned a weight (`w_fees`, `w_0`, `w_1`), which the incentive provider defines.

With this setup, this is as if the overall incentive budget was split in 3, with a proportion being shared by LPs based on how much fees they've earned, a proportion shared based on the overall amount of token 0 they've held and a last portion based on the relative token 1 balance they've had in their position during the time period.

### 📊 Example Calculation

If the incentive weights are set as:

* Fees = 40%,
* Token 0 = 30%,
* Token 1 = 30%,

then:

* A user earning 50% of total pool fees receives 20% of total rewards (50% × 40%).
* A user holding 30% of Token 0 receives 9% of total rewards (30% × 30%).
* A user holding 20% of Token 1 receives 6% of total rewards (20% × 30%).

{% hint style="info" %}
On the Merkl frontend, these parameters can set by incentive providers on the page where they create their concentrated liquidity campaigns.
{% endhint %}

## 🎯 Reward strategy examples

Incentive providers can fine-tune parameters to match their objectives:

* Stablecoin Peg Protection – To prevent USDC dumps, an incentive provider might increase the weight of Token 0 (USDC), rewarding LPs who hold more USDC in the pool.
* Encouraging Tight Ranges – A Liquid Restaking Token (LRT) provider might allocate 98% of rewards to fee generation, incentivizing super-concentrated liquidity around the tick.

## 🚀 Sampling and Anti-DOS

For large pools with high swap activity, Merkl samples the largest swaps instead of analyzing every transaction.

* Out-of-range positions detected during sampled swaps are excluded from rewards.
* Users earning less than 0.0000001% of total rewards are filtered out.
* Positions with less than $10 liquidity are disqualified.

To maximize rewards, LPs should actively manage their positions to stay in-range.

## ⛔ Out of range liquidity

Incentive providers can choose whether to reward out-of-range liquidity.

* By default, only in-range positions receive rewards.
* We strongly recommend incentivizing in-range liquidity, as this ensures active liquidity provision and avoids rewarding idle capital.

## 🔎 Fake Volume Attacks detection

Merkl automatically detects and blacklists users attempting to game the system via:

* Wash trading: Creating tight positions and self-trading to earn rewards.
* Frequent rebalancing: If mistaken for wash trading, may trigger blacklisting.

{% hint style="info" %}
If you were wrongfully blacklisted, open a support ticket on [Discord](https://discord.gg/tZPwmgqH).\
Blacklisted addresses cannot recover lost rewards.
{% endhint %}

## Automated Liquidity Management Solutions

Merkl supports ALMs, which actively manage liquidity positions.

* ALMs are treated like normal LPs, and their rewards are automatically forwarded to depositors.
* New ALMs can be integrated offchain, making it easy to add support for new types of position managers
* Incentive providers can blacklist/whitelist ALMs if needed.

{% hint style="info" %}
For more details on forwarding, blacklisting and whitelisting with Merkl, you can check [this page](../hooks.md) on customizability hooks.
{% endhint %}

## 📈 APRs in Concentrated Liquidity Campaigns

Merkl displays average APRs for campaigns. However, actual APR varies per position based on:

* How tightly liquidity is concentrated.
* The weight assigned to fees vs. token balances.
* How others are providing liquidity.

Example:

* If fees = 99%, a full-range position may earn minimal rewards despite providing high liquidity.
* If Token A = 60% and Token B = 10%, LPs holding more Token A will earn disproportionately higher rewards and you may be better off skewing your position so it has more of token A than of token B.

## Providing Liquidity for Concentrated Liquidity Campaigns

### 1️⃣ Providing Liquidity Directly

To maximize rewards, consider:

* Range tightness – Tighter ranges increase earnings but risk going out-of-range.
* Token split – Adjust holdings based on campaign parameters for Token A and Token B.

{% embed url="https://youtu.be/8tpGSHglVlc?feature=shared" %}

### 2️⃣ Using an Automated Liquidity Manager (ALM)

Providing liquidity via supported ALMs automatically qualifies for rewards if the ALM is incentivized. Be careful though to understand how they rebalance liquidity.

If you already had liquidity in a pool before a Merkl incentive launched (directly or through an ALM), you will still earn rewards if your position remains active and in-range—no need to re-enter.
