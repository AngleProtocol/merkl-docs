---
description: Everything you need to know about concentrated liquidity campaigns
---

# Concentrated Liquidity Mechanisms

## Merkl Script

<figure><img src="https://docs.merkl.xyz/~gitbook/image?url=https%3A%2F%2F3295124503-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FKRQdTiHhBGLRKeCCOqdc%252Fuploads%252Fgit-blob-68540f716bcdabc987398505f00046ce9ba3d529%252Fdocs-merkl-script.png%3Falt%3Dmedia&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=e6d29aef&#x26;sv=1" alt=""><figcaption></figcaption></figure>

## Reward Distribution Formula

For a given pool with two tokens (A and B), the script analyzes the swaps that occurred in the pool during the specified period and computes a reward score for each position based on the following factors:

* **Fees Earned**: The fees earned by the position during the period, which represent the liquidity of the position used by the pool.
* **Token A Holding**: The amount of token A held by the position during swaps in the pool, compared to the total amount of token A in the pool.
* **Token B Holding**: The amount of token B held by the position during swaps in the pool, compared to the total amount of token B in the pool.

Each parameter is assigned a different weight, chosen by the incentivizer. Additionally, incentivizers can further customize the reward distribution for the pool by allowing addresses holding a specific token (e.g., veANGLE or veCRV) to earn boosted rewards.

The exact distribution formula for a position in such a pool during a specified time period is as follows:

\[𝑤fees×fees by positionfees by pool+𝑤A×A in positionA in pool+𝑤B×B in positionB in pool]×optional gov token boost\[wfees​×fees by poolfees by position​+wA​×A in poolA in position​+wB​×B in poolB in position​]×optional gov token boost

**For large pools with numerous swaps, the script may not analyze all the swaps that occurred during the specified period but instead sample the largest ones.**

**While Merkl can incentivize any type of liquidity provider, the system includes an anti-DoS filter that only rewards addresses meeting the following criteria:**

* **Positions with more than $20 worth of liquidity.**
* **Users earning more than 1/10,000,000th (or 0.00001%) of the campaign rewards per engine run.**

**For more information on Merkl rewards, check this** [**page**](../../earn-with-merkl/earn-with-merkl/earn-on-concentrated-liquidity-campaigns.md)**.**

## Concentrated Liquidity Forwarders (ALMs)

Merkl is compatible with Automated Liquidity Management (ALMs) protocols. These protocols actively maintain positions for LPs on concentrated liquidity AMMs. The script does not differentiate managers from other "normal" addresses when computing rewards. It then splits the rewards going to the position manager address proportionally among all holders of its token

With Merkl, if you incentivize a pool that is compatible with one of the liquidity managers supported by the system, it will be automatically detected by the script, and users indirectly providing liquidity through those position managers will be able to directly claim their rewards from Merkl contracts.

Since the system is off-chain, new types of position managers can easily be added into the system. For instance, it would be possible to reward users of protocols that use position manager tokens on other contracts, such as collateral for borrowing.

The list of liquidity position managers supported for each AMM and chain can be found directly on the incentivized pools on[ Merkl app](https://app.merkl.xyz/integrations). If you want to add support for a type of liquidity position manager that is not currently supported or to directly reward the underlying users of a smart contract that indirectly controls AMM liquidity, fill out [this form](https://tally.so/r/w4JYLr) and [open a BD ticket on our Discord](https://discord.com/channels/1209830388726243369/1210212731047776357).&#x20;
