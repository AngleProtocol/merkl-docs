---
description: Guide for Liquidity Providers on concentrated liquidity AMMs to enjoy Merkl
---

# Earn on Concentrated Liquidity Campaigns

As a Liquidity Provider, Merkl lets you customize your concentrated liquidity position to optimize your returns.

## Getting Started

To receive rewards from a concentrated liquidity campaign on Merkl, you can:

1. **Provide liquidity directly in one of the incentivized pools.** To invest your liquidity and start having positions, you need to go to the protocol's app. Merkl provides direct links to these apps, making it easy to get started.&#x20;

The main choices you need to make when adding liquidity are:

* **Range tightness:** Decide how tight the position's range should be. For example, a tighter range provides more virtual liquidity and earns more fees and rewards but has a higher risk of becoming out-of-range and suffering from impermanent loss
* **Token Split**: Determine the split between the two tokens.

{% embed url="https://youtu.be/8tpGSHglVlc?feature=shared" %}

2. **Or provide liquidity in one of the supported Automated Liquidity Management (ALM) protocols that has position(s) in the incentivized concentrated liquidity pool.** If you are using an Automated Liquidity Management (ALM) protocol, be careful to understand how they rebalance liquidity.

**If you started providing liquidity in a concentrated liquidity pool (either directly on the AMM or through an ALM) before the incentive was created on Merkl, and your positions are still active and meet the parameters set by the Incentive Provider (IP), you will be eligible to claim your rewards when they are distributed. There is no need to recalibrate or exit and re-enter the liquidity pool with your position**

**All Automated Market Makers (AMMs) and Automated Liquidity Management (ALMs) protocol shown on Merkl's app have been whitelisted by Merkl, ensuring that you can safely invest your liquidity across various platforms.** For a comprehensive list of AMMs and ALMs supported by Merkl, check this [link](https://app.merkl.xyz/integrations).

<figure><img src="https://docs.merkl.xyz/~gitbook/image?url=https%3A%2F%2F3295124503-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FKRQdTiHhBGLRKeCCOqdc%252Fuploads%252Fgit-blob-33d4b8e8fec2d21212bdce431024a7ab8cc3b12f%252Fdocs-merkl-for-lps.png%3Falt%3Dmedia&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=d0c4c738&#x26;sv=1" alt=""><figcaption></figcaption></figure>

## Disclaimer for Concentrated Liquidity

Concentrated liquidity pools offer the potential for higher returns but come with specific risks. Snapshots for rewards are made based on swap activities, and the engine collects data from subgraphs to measure user participation. If your liquidity position is out of range during a swap, you won't be eligible to receive rewards for that period. Ensure your positions are actively managed to stay within the effective range to maximize your rewards.

## Anti-DoS Filter in Concentrated Liquidity

**Note that for concentrated liquidity, in addition to the Merkl anti-DoS filter that cuts off rewards for users who receive less than 1/10,000,000th (0.00001%) of the campaign rewards per engine run across all active campaigns, positions with less than $20 of liquidity are not eligible for incentives.**

## Fake Volume Attacks detection

Merkl automatically detects and blacklist users who try to game the reward system by creating very tight positions and trading against themselves (i.e., users engaging in wash trading). If you have been blacklisted and want to understand why or request removal from the blacklist, please open a support ticket in our [Discord](https://discord.gg/tZPwmgqH). Note that we can only un-blacklist an address; we cannot recover funds that should have been earned by the position while the address was blacklisted.

