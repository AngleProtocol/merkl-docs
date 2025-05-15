---
description: Important features of Merkl campaigns
---

# 🪶 Additional Features

Beyond campaign types, distribution methods, and hook customization, Merkl offers several advanced features to enhance campaign flexibility and reward management.

## 🔥 Run Multiple Campaigns on the Same Opportunity

Merkl allows multiple campaigns to be created simultaneously for the same pool or set of actions.

**Why is this useful?**

- Co-incentives: Different parties can independently add incentives to the same opportunity.
- Flexible Stacking: Multiple campaigns can run alongside each other on the same pool or token, supporting different incentive structures.

## ⏳ Campaigns of Any Duration

Merkl campaigns can run for any length of time, from as short as one hour to as long as six months (or more).

This flexibility allows for:

- Short-term boosts (e.g., flash incentives for new launches).
- Long-term, sustained incentive programs for ongoing ecosystem growth.

## 🎭 Infinite Customizability with Token Wrappers

Merkl campaigns can distribute tokens with custom properties, known as token wrappers, to introduce advanced incentive mechanisms.

**Examples of Token Wrapper Use Cases:**

- Vesting & Slashing Conditions: Add vesting schedules or penalties for early withdrawals.
- Non-Prefunded Campaigns: Instead of preloading tokens, rewards are pulled from a multisig when users claim.
- Time-Locked Transfers: Issue non-transferable tokens that unlock after a set period.
- Redeemable Tokens: Distribute placeholder tokens that can be redeemed later.

Merkl provides a suite of template contracts for token wrappers in the Merkl GitHub repository so anyone can build [its own token wrapper](https://github.com/AngleProtocol/merkl-contracts/tree/main/contracts/partners/tokenWrappers). Some templates have already been audited by Merkl partners!

{% hint style="info" %}
Got a custom use case? Let us know—we’re happy to collaborate and help build your solution.
{% endhint %}

## 🌍 Cross-Chain Campaigns

Merkl allows you to incentivize activity on one chain while distributing rewards on another.

**How It Works:**

- Activity is tracked on Chain A (e.g., a protocol running on Arbitrum).
- Rewards remain claimable on Chain B (e.g., distributed token stays on Ethereum): the chain where the token is claimable is the chain where the campaign was created

**Why is this useful?**

- Efficient token management: Keeps governance tokens on a single chain, reducing the need for bridging.
- Cross-chain flexibility: Supports protocols that operate on multiple chains without fragmenting incentives.

**Important considerations**:

Some smart contracts on the chain you are incentivizing activity may not exist on the chain where users can claim their reward (or may exist at a different address):

- Affected addresses will be unable to claim rewards in this case
- Solution: As an incentive provider, you should blacklist any ineligible addresses to prevent reward loss. Or you can reallocate rewards (more below) of addresses that cannot claim to an address controlled by the same provider that can claim its rewards

## 🤝 Referral Program Documentation

### Overview

The **Referral Program** feature allows you to create and manage multiple referral programs across the **Merkl Ecosystem**. This feature comes with a wide range of customization options to suit your needs, from user rewards to whitelist gates, all secured by blockchain technology.

### Key Features:

- **Create Unlimited Referral Programs**: Launch as many referral programs as you want.
- **Referral Code Generation**: Users can generate unique referral codes, share them with their friends, and earn rewards.
- **Whitelabel Integration**: Easily integrate the program into your front-end with whitelabel options (Contact Merkl for details).
- **Cross-Protocol Support**: Referral programs are compatible across the entire Merkl ecosystem, allowing creators to incentivize on any protocol/behaviour integrated with Merkl.
- **Customizable Rewards**: Tailor rewards to users, referrers, invited users, or even non-participating users.
- **Conditions to participate**: Add a whitelist restriction to the program, and optionally charge a fee to create a referral code. Or let anyone participate.
- **Blockchain Security**: Users need to sign a transaction to confirm their referral action, ensuring secure and verified participation.

### Next steps:

- Contact Merkl for details on how to implement

## 🔄 Campaign Reallocation

Once a campaign has ended, campaign creators can reallocate unclaimed rewards from any recipient to another address at their discretion.

Campaign creators can also reallocate all unclaimed rewards at once.

To give users time to claim their rewards, reallocation is only available after a defined window following the campaign’s end. Currently, this window ranges from 1 day to 7 months post-campaign.

When reallocation is triggered, the process can take up to 24 hours. This delay is due to required security checks and the need to publish a Merkle root onchain—an essential step for making the reallocated rewards claimable. Since Merkle root updates do not occur continuously or in real time, this contributes to the delay.

Once the reallocation is complete and the Merkle root is updated on the relevant chain, the new rewards can be claimed just like any other Merkl reward, using the address to which they were reassigned.

**Use Case:**

- Useful for redirecting rewards from addresses that cannot claim them (e.g., lost wallets, smart contracts without claim functions).
- No fees are charged for reallocations.

**Developer:**

To reallocate rewards, you need to call from the creator address [reallocateCampaignRewards](https://github.com/AngleProtocol/merkl-contracts/blob/1006c8ff64ba3eb4732a19da3cec92d4afc92eb8/contracts/DistributionCreator.sol#L285) with parameters:

- \_campaignId: the campaign ID you want to reallocate unclaimed rewards for
- froms: An array of addresses that you want to reallocate from
- to: the address that should receive the reallocated rewards

To reallocate all unclaimed rewards, you can set `froms` to `[0x0000000000000000000000000000000000000000]`.

## ✏️ Campaign Overrides

Merkl allows campaign creators to modify an active campaign by adding extra customization options, such as:

- Adjusting blacklists or whitelists to include or exclude certain addresses.
- Enabling additional features as needed.
- Ending a campaign early if necessary

❌ What you cannot do :

- Change the reward token
- Change the end date of the campaign to a date in the past
- For variable APR campaigns, you cannot change the total amount distributed.

## ◀️ Retroactive Campaigns

You can create campaigns in the past to reward OG users. It can start and end in the past or it can end in the future.

## 🏛️ Onchain Governance System Integrations

Merkl supports onchain governance-driven incentives, allowing protocols to automatically issue incentives through Merkl based on governance vote outcomes.

For protocols using gauge systems:

- Merkl provides ready-to-use smart contracts to plug into any gauge system.
- These connectors automatically translate governance votes into highly customizable Merkl campaigns. Our Governance Plug-in Templates are available [here](https://github.com/AngleProtocol/merkl-contracts/tree/main/contracts/partners/middleman).

{% hint style="info" %}
Need help integrating Merkl with your onchain reward system? We’re here to assist—reach out for guidance! We've also got a detailed guide for this available [here](../../distribute-with-merkl/deploy-your-campaign-from-dao.md).
{% endhint %}
