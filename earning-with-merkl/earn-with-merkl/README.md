---
description: Everything you need to know to earn on Merkl
---

# Earn with Merkl

All yield opportunities using Merkl are listed at [app.merkl.xyz](https://app.merkl.xyz).

This interface displays the different campaigns and their types. For every opportunity, you will find a link to deposit your funds directly on the respective protocol's app. Once you deposit your funds or perform the required actions for an opportunity incentivized by Merkl, no additional steps are required to start receiving your rewards. You can claim them directly from the [Merkl dashboard](https://app.merkl.xyz/user) or any other app that integrates with Merkl. In particular, **you will not need to stake your tokens anywhere else**.

The Merkl engine is compatible with multiple incentive providers incentivizing the same type of campaigns (e.g., the same pool on UniswapV3) with potentially different parameters. If you are eligible for a campaign on Merkl, you will claim rewards from all incentive providers who have incentivized your behavior when claiming your rewards. This means that many teams can incentivize a specific behavior at the same time with different tokens.

Additionally, you can claim all your tokens in one click per chain, making the process seamless and efficient.

## How to get started

Merkl is a one-stop shop for finding the best investment opportunities in DeFi and it only takes a few clicks to get started!

1. Visit the [Merkl App](https://app.merkl.xyz/).
2. Explore the various opportunities and find the one that suits you best.
3. Invest your liquidity directly on the protocol's app by following the provided links.
4. That's it! You're now earning rewards, no staking or other on-chain action is needed.

You can track your active positions and your rewards by visiting your [user dashboard](https://beta.merkl.xyz/user/).

Once you've earned rewards, you can claim all your earned tokens with a single click.

Happy farming!

## Rewards on Merkl

Rewards on Merkl do not increase block by block, but can be claimed at a frequency which depends on the chain. You can check the claim frequency at this [link](https://app.merkl.xyz/status).

It is important to note that the Merkl engine script cuts off rewards for users who receive less than 1/10,000,000th (or 0.00001 %) of the campaign rewards per engine run across all active campaigns. This anti-DoS (Denial of Service) filter helps maintain the platform's integrity and therefore ensures the continuity of fair distribution of rewards.

&#x20;Example on a V2 Liquidity Pool:

* Campaign Duration: 14 days
* Total Value Locked of the V2 Liquidity Pool: $1,000,000 on Arbitrum
* Merkl Engine Script Frequency: Every 4 hours (6 times per day)

For this campaign:

* Each distribution will release 1/84th of the total rewards (1 / (14 days \* 6 times per day)).
* If a user has $10 of liquidity, it represents 1/100,000th (or 0.001%) of the total TVL ($10 / $1,000,000 = 1/100,000 = 0.001%).
* Per script run, the user's potential reward would be 1/8,400,000th (approximatively 0.000012%) of the total rewards (1/100,000 \* 1/84), which is more than 1/10,000,000th (or 0.00001 %). Therefore, the reward would be distributed.

However, if the TVL were $2,000,000 and users had $10 of liquidity, the user's liquidity would represent 1/200,000th (or 0.0005%) of the total TVL. Per run, the potential reward would be 1/16,800,000th (1/200,000 \* 1/84, or approximatively 0.0000059%), which is in that case below the threshold (1/10,000,000th or 0.00001 %) and thus would not be distributed.

Note that, by default, rewards can only be claimed by the address that earned them. If you need to claim rewards on behalf of another address, please [open a tech ticket in our Discord ](https://discord.com/channels/1209830388726243369/1210212731047776357)so that the team can whitelist your address.

## Learn More

In the following pages, we explain in more detail how to take advantage of the [Concentrated Liquidity (CLAMM)](earn-on-concentrated-liquidity-campaigns.md) campaigns on Merkl to earn rewards.

If you want to learn  the Other ways to earn on Merkl both actively and passively, check the [Other Ways to Earn on Merkl page](other-ways-to-earn-on-merkl.md).&#x20;



