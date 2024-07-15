---
description: Everything you need to know about concentrated liquidity campaigns
---

# Concentrated Liquidity Mechanisms

## Merkl Script

<figure><img src="../../.gitbook/assets/docs-merkl-script.png" alt=""><figcaption></figcaption></figure>

## Reward Distribution Formula

For a given pool with two tokens (A and B), the script analyzes the swaps that occurred in the pool during the specified period and computes a reward score for each position based on the following factors:

* **Fees Earned:** The fees earned by the position during the period, which represent the liquidity of the position used by the pool.
* **Token A Holding:** The amount of token A held by the position during swaps in the pool, compared to the total amount of token A in the pool.
* **Token B Holding:** The amount of token B held by the position during swaps in the pool, compared to the total amount of token B in the pool.

Each parameter is assigned a different weight, chosen by the incentivizer. Additionally, incentivizers can further customize the reward distribution for the pool by allowing addresses holding a specific token (e.g., veANGLE or veCRV) to earn boosted rewards.

The exact distribution formula for a position in such a pool during a specified time period is as follows:

\[
\left[ \frac{w_{\text{fees}} \times \text{fees by position}}{\text{fees by pool}} + \frac{w_A \times A \text{ in position}}{A \text{ in pool}} + \frac{w_B \times B \text{ in position}}{B \text{ in pool}} \right] \times \text{optional gov token boost}
\]


**For large pools with numerous swaps, the script may not analyze all the swaps that occurred during the specified period but instead sample the largest ones.**

**While Merkl can incentivize any type of liquidity provider, the system includes an anti-DoS filter that only rewards addresses meeting the following criteria:**

* **Positions with more than $20 worth of liquidity.**
* **Users earning more than 1/10,000,000th (or 0.00001%) of the campaign rewards per engine run.**

**For more information on Merkl rewards, check this** [page](../../earning-with-merkl/earn-with-merkl/earn-on-concentrated-liquidity-campaigns.md).

## Concentrated Liquidity Forwarders (ALMs)

Merkl is compatible with Automated Liquidity Management (ALMs) protocols. These protocols actively maintain positions for LPs on concentrated liquidity AMMs. The script does not differentiate managers from other "normal" addresses when computing rewards. It then splits the rewards going to the position manager address proportionally among all holders of its token

With Merkl, if you incentivize a pool that is compatible with one of the liquidity managers supported by the system, it will be automatically detected by the script, and users indirectly providing liquidity through those position managers will be able to directly claim their rewards from Merkl contracts.

Since the system is offchain, new types of position managers can easily be added into the system. For instance, it would be possible to reward users of protocols that use position manager tokens on other contracts, such as collateral for borrowing.

The list of liquidity position managers supported for each AMM and chain can be found directly on the incentivized pools on[ Merkl app](https://app.merkl.xyz), or at this [link](https://app.merkl.xyz/integrations). If you want to add support for a type of liquidity position manager that is not currently supported or to directly reward the underlying users of a smart contract that indirectly controls AMM liquidity, fill out [this form](https://tally.so/r/w4JYLr) and [open a BD ticket on our Discord](https://discord.com/channels/1209830388726243369/1210212731047776357).
