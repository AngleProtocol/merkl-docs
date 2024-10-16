---
description: Everything you need to know to earn on Merkl
---

# 💰 Earn with Merkl

All yield opportunities using Merkl are listed at [app.merkl.xyz](https://app.merkl.xyz).

This interface displays the different campaigns and their types. For every opportunity, you will find a link to deposit your funds directly on the respective protocol's app. Once you deposit your funds or perform the required actions for an opportunity incentivized by Merkl, no additional steps are required to start receiving your rewards. You can claim them directly from the [Merkl dashboard](https://app.merkl.xyz/user) or any other app that integrates with Merkl. In particular, **you will not need to stake your tokens anywhere else**.

The Merkl engine is compatible with multiple incentive providers incentivizing the same type of campaigns (e.g., the same pool on UniswapV3) with potentially different parameters. If you are eligible for a campaign on Merkl, you will claim rewards from all incentive providers who have incentivized your behavior when claiming your rewards. This means that many teams can incentivize a specific behavior at the same time with different tokens.

Additionally, you can claim all your tokens in one click per chain, making the process seamless and efficient.

## How to get started

Merkl is a one-stop shop for finding the best investment opportunities in DeFi and it only takes a few clicks to get started!

1. Visit the [Merkl App](https://app.merkl.xyz/).
2. Explore the various opportunities and find the one that suits you best.
3. Before getting on any Merkl opportunity, make sure that you understand how it works and how users can be rewarded as part of this opportunity. For this, we encourage to:

- look into [the pages of the docs](../../mechanisms/types-of-campaign.md) that cover the campaign type in question
- look into the page associated to the opportunity. Every opportunity listed on Merkl has its dedicated page, and on each page you may find the details of the different campaigns that were created for the opportunity. It's important to look into the cards associated to the live campaigns so you understand whether [specific hooks](../../mechanisms/hooks/README.md) apply to them.
- understand the protocol where you're depositing liquidity and DYOR before putting funds somewhere. The main risk with Merkl lies with the underlying protocols being incentivized, not with Merkl, as you do not need to stake any assets to be eligible and claim your rewards. Your funds on underlying protocols could be at risk due to the inherent risks associated with those protocols, such as smart contract vulnerabilities or operational failures. While we do our best to whitelist every project we work with to ensure they are reliable and safe for our users, we cannot fully audit every project using the platform, and are not responsible for the potential issues of the protocols incentivized on Merkl.

4. Invest your liquidity directly on the protocol's app by following the provided links.
5. That's it! You're now earning rewards, no staking or other onchain action is needed.
6. You should normally be able to track your active positions and your rewards by visiting your [user dashboard](https://app.merkl.xyz/user/). Note that while the majority of protocols we support are accurately tracked, there may be some situations where the Merkl frontend is unable to detect your position in a given protocol. The source of truth in this case remains the frontend of the protocols where you deposited funds. It's not because your position is not detected on the user page of the Merkl frontend that you cannot be eligible to rewards for this position.
7. Once you've earned rewards, you can claim all your earned tokens with a single click.

Happy farming!

## Rewards on Merkl

Rewards on Merkl do not increase block by block, but can be claimed at a frequency which depends on the chain. You can check the claim frequency at this [link](https://app.merkl.xyz/status).

It is important to note that the Merkl engine script cuts off rewards for users who receive less than 1/10,000,000th (or 0.00001 %) of the campaign rewards per engine run across all active campaigns. This anti-DoS (Denial of Service) filter helps maintain the platform's integrity and therefore ensures the continuity of fair distribution of rewards.

Example on a Constant Product Liquidity Pool on UniswapV2:

- Campaign Duration: 14 days
- Total Value Locked of the UniswapV2 Liquidity Pool: \$1,000,000 on Arbitrum
- Merkl Engine Script Frequency: Every 4 hours (6 times per day)

For this campaign:

- Each distribution will release 1/84th of the total rewards (1 / (14 days \* 6 times per day)).
- If a user has $10 of liquidity, it represents 1/100,000th (or 0.001%) of the total TVL ($10 / \$1,000,000 = 1/100,000 = 0.001%).
- Per script run, the user's potential reward would be 1/8,400,000th (approximatively 0.000012%) of the total rewards (1/100,000 \* 1/84), which is more than 1/10,000,000th (or 0.00001 %). Therefore, the reward would be distributed.

However, if the TVL were $2,000,000 and users had $10 of liquidity, the user's liquidity would represent 1/200,000th (or 0.0005%) of the total TVL. Per run, the potential reward would be 1/16,800,000th (1/200,000 \* 1/84, or approximatively 0.0000059%), which is in that case below the threshold (1/10,000,000th or 0.00001 %) and thus would not be distributed.

{% hint style="info" %}
Note that, by default, rewards can only be claimed by the address that earned them. If you need to claim rewards on behalf of another address, please [open a tech ticket in our Discord ](https://discord.com/channels/1209830388726243369/1210212731047776357)so that the team can whitelist your address.
{% endhint %}

## Campaign specific rewards

Some campaign types have their specificities and it's important to be aware of them before entering a campaign.

### Earn on Concentrated Liquidity Campaigns

When you participate in [a concentrated liquidity campaign](../../mechanisms/campaigns/concentrated-liquidity-mechanisms.md) with Merkl, it is important to look into the reward parameters set by the incentivizer when creating the campaign. This will determine how you should shape your liquidity for an optimal reward amount.

The APR values displayed on Merkl frontend are also average APRs, and every position earns something different based on how it provides liquidity with respect to the reward parameters that were set. Typically, if the fee parameter is set at a big value (like 99%), and if you provide a full range position, then you may be providing a significant amount of liquidity in the pool without receiving a lot of rewards.

If the value of the parameter for one token are higher than the other token (e.g 60% token A, 10% token B and 30% fees), then you are better off skewing your position so it has more of token A than of token B.

Globally on concentrated liquidity campaigns, what you earn is tied to how others have provided liquidity. If you had a concentrated liquidity position, but someone with less liquidity had a more concentrated position, then this user may earn more rewards than you.

People who provide liquidity through the same automated liquidity management solution all earn the same APR though.

In summary, to receive rewards from a concentrated liquidity campaign you can:

1. **Provide liquidity directly in one of the incentivized pools.** As a Liquidity Provider, Merkl lets you customize your concentrated liquidity position to optimize your returns. The main choices you need to make when adding liquidity are:

- **Range tightness:** Decide how tight the position's range should be. For example, a tighter range provides more virtual liquidity and earns more fees and rewards but has a higher risk of becoming out-of-range and suffering from impermanent loss.
- **Token Split:** Determine the split between the two tokens. You can look into the parameter for token A and token B in the campaign you're targeting to see if there is an optimal ratio to find

{% embed url="https://youtu.be/8tpGSHglVlc?feature=shared" %}

2. **Or provide liquidity in one of the supported Automated Liquidity Management (ALM) protocols that has position(s) in the incentivized concentrated liquidity pool.** If you are using an Automated Liquidity Management (ALM) protocol, be careful to understand how they rebalance liquidity. If you started providing liquidity in a concentrated liquidity pool (either directly on the AMM or through an ALM) before the incentive was created on Merkl, and your positions are still active and meet the parameters set by the incentive provider, you will be eligible to earn rewards when they are distributed. There is no need to recalibrate or exit and re-enter the liquidity pool with your position(s).

#### Disclaimer for Concentrated Liquidity Campaigns

Concentrated liquidity pools offer the potential for higher returns but come with specific risks. Snapshots for rewards are made based on swap activities, and the engine collects data from subgraphs to measure user participation. If your liquidity position is out of range during a swap, you won't be eligible for that period. Ensure your positions are actively managed to stay within the effective range to maximize your rewards.

### Earn from airdrops and token snapshots

For these campaigns, the positions that lead you to earn rewards will not be tracked in the Merkl [user dashboard](https://app.merkl.xyz/user), but you can still see your accrued rewards on the same page.
