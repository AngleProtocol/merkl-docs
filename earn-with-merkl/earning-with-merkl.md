---
description: Everything you need to know to earn on Merkl
---

# 💰 Earn with Merkl

All yield opportunities using Merkl are listed at [app.merkl.xyz](https://app.merkl.xyz)

<figure><img src="../.gitbook/assets/Capture d’écran 2025-06-10 à 11.47.52 1.png" alt=""><figcaption><p>The Merkl App</p></figcaption></figure>

This interface displays the different campaigns and their types. For every opportunity, you will find a link to deposit your funds directly on the respective protocol's app. Once you deposit your funds or perform the required actions for an opportunity incentivized by Merkl, no additional steps are required to start receiving your rewards. You can claim them directly from the [Merkl dashboard](https://app.merkl.xyz/user) or any other app that integrates with Merkl. In particular, **you will not need to stake your tokens anywhere else**.

The Merkl engine is compatible with multiple incentive providers incentivizing the same type of campaigns (e.g., the same pool on Uniswap V3) with potentially different parameters. If you are eligible for a campaign on Merkl, you will claim rewards from all incentive providers who have incentivized your behavior when claiming your rewards. This means that many teams can incentivize a specific behavior at the same time with different tokens.

Additionally, you can claim all your tokens in one click per chain, making the process seamless and efficient.

## How to get started

Merkl is a one-stop shop for finding the best investment opportunities in DeFi and it only takes a few clicks to get started!

1. Visit the [Merkl App](https://app.merkl.xyz/).
2. Explore the various opportunities and find the one that suits you best.
3. Before getting on any Merkl opportunity, make sure that you understand how it works and how users can be rewarded as part of this opportunity. For this, we encourage you to:
   1. check the different [campaign types](../merkl-mechanisms/campaign-types/). All campaign have their specificities and it's crucial to understand them before trying to earn rewards from it
   2.  explore the page associated with each opportunity in the Merkl App. Every opportunity listed on Merkl has a dedicated page where you can find details about the different campaigns linked to it. It's important to review the information for each live campaign to check if any specific rules or eligibility criteria apply.\


       <figure><img src="../.gitbook/assets/Group 1.png" alt=""><figcaption><p>Opportunity page in the Merkl App with two active incentive campaigns</p></figcaption></figure>
   3. understand the protocol where you're depositing liquidity and DYOR before investing. Merkl is non-custodial — you don’t need to deposit any assets on Merkl to be eligible or to claim your rewards. All interactions happen directly on the protocols. The main risk with Merkl lies in the underlying protocols being incentivized and with which you interact. Your funds on these underlying protocols may be at risk due to inherent issues, such as smart contract vulnerabilities or operational failures. While we do our best to whitelist every project we work with to ensure they are reliable and safe for our users, we cannot fully audit every project using the platform, and are not responsible for the potential issues of the protocols incentivized on Merkl.
4. Deposit your liquidity directly on the protocol's app by following the provided links on the opportunity page.
5. That's it! You're now earning rewards, no staking or other onchain action is needed.
6.  The next day, your [user dashboard](https://app.merkl.xyz/user/) will start showing the rewards you've earned. All you need to do now is claim all your earned tokens with a single click. If gas is expensive, you can wait a few more days/weeks to claim the rewards.\


    <figure><img src="../.gitbook/assets/Capture d’écran 2025-06-10 à 12.20.32 1.png" alt=""><figcaption><p>The Merkl dashboard to claim rewards in the Merkl App</p></figcaption></figure>
7. Make sure that you claim all your rewards within a year of receiving them!

Happy farming!

{% hint style="info" %}
The reward reports stored in the bucket for a given chain can be accessed from [the status page](https://app.merkl.xyz/status) on the Merkl app.
{% endhint %}

{% hint style="info" %}
Before aping in a campaign, do not get lured by APR values: it could be the case the only whitelisted addresses can access rewards, or that only highly concentrated positions earn the majority of what's available for rewards
{% endhint %}

## Rewards on Merkl

Rewards on Merkl do not increase block by block, but can be claimed at a frequency which depends on the chain. You can check the claim frequency on the [Status page](https://app.merkl.xyz/status).

Note that, by default, rewards can only be claimed by the address that earned them. You can however approve an operator to claim on your behalf by calling the function `toggleOperator` on the [distributor smart contract](https://app.merkl.xyz/status). However, rewards will still be sent to the original address that earned them.

So to sum up, assuming Alice earned the rewards:

* by default only Alice can claim and rewards are sent to Alice.
* by calling `toggleOperator`, Alice can allow Bob to claim on her behalf. Then, Bob can claim for Alice by sending Alice's proof to the contract, and rewards are then sent to Alice.

If you can't call `toggleOperator` and are stuck, please [open a tech ticket in our Discord ](https://discord.com/channels/1209830388726243369/1210212731047776357), the team may be able to call it on your behalf.
