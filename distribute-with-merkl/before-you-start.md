---
description: >-
  You can launch an incentive campaign on your own in just a few minutes using
  Merkl Studio. However, there are a few prerequisites to complete beforehand.
---

# Before you start

## ⚙️ Merkl’s core principles

Before setting up your campaign on Merkl, make sure you have read and understood:

* [**Merkl’s reward distribution process**](../merkl-mechanisms/technical-overview.md) (reward computation vs. reward update, dispute period, etc.). 
TLDR: 
  - Reward computations (process by which the Merkl Engine calculates reward allocations) run on average every 2 hours. After this step, users can view the rewards they’ve earned, which are marked as "Claimable soon" or "Pending"
  - Rewards updates (process by which the Merkl Engine compresses computed campaign data into a Merkle root and pushes it onchain) run on average every 8 hours. After this step, users can claim their rewards.
* [**The different incentive mechanisms**](../merkl-mechanisms/incentive-mechanisms.md) (campaign types, distribution types)
* [**Merkl forwarding system**](../merkl-mechanisms/reward-forwarding.md)
* How to personalize your campaign using [**customization options**](../merkl-mechanisms/customization-options.md)
* [**Additional features**](../merkl-mechanisms/features.md) that can make your campaign unique

{% hint style="info" %}
If you're looking for some incentive mechanism for which you're not sure about whether it's supported, or looking to add incentive features or customization options that are not displayed on the platform, please contact us!
{% endhint %}

Some Merkl features (e.g., new distribution types, campaign types, customization options) may not be directly available in the campaign creation flow. If a required configuration option is not available in the frontend, we can:

* Help you generate the correct campaign payload.
* Provide dedicated API endpoints so you can structure your campaign as needed.

## 📝 Token whitelisting

Before starting your campaign, you need to ensure that the token you want to use as reward with Merkl has been whitelisted.

We usually process token whitelisting requests once a day, and it's a way for us to ensure that we will be able to properly compute APRs and that the token will be safe to use for our users. **Campaigns must distribute at least ~$1/hour in total rewards to avoid dusting.**

You can check whether your token is already whitelisted by setting it as the reward token in the [Merkl studio](https://studio.merkl.xyz/create-campaign/clamm). If your token is not whitelisted, please submit the reward token’s price source and logo by filling out this [whitelisting form](https://anglemoney.notion.site/1aecfed0d48c808a8194fe2befd50bdb?pvs=105).

{% hint style="success" %}
Make sure you have all the tokens you want to distribute in your wallet when creating a campaign
{% endhint %}

## 📃 Merkl terms

You must also make sure that you have read and understood [Merkl's Terms & Conditions](https://app.merkl.xyz/terms). You will be required to agree to these terms during the campaign setup process.

## 💰 What budget should I set for my campaign?

The ideal campaign budget depends on your specific objectives and the distribution method you choose. Campaign budgets follow a natural supply-and-demand dynamic: liquidity providers want to maximize their rewards, while campaign creators aim to minimize costs. As a result, TVL typically scales proportionally with the reward budget you allocate.

Depending on your campaign type, you need to account for fees differently (see our [fees section](../distribute-with-merkl/fee-model.md) for details).


**Distribution methods matter**: If you're concerned about over-distributing rewards early in your campaign, consider using a [capped APR campaign](../merkl-mechanisms/distributions.md#-capped-reward-rate-campaigns). This approach ensures you don't exhaust your budget too quickly if TVL starts low, as rewards are distributed at a fixed rate relative to TVL rather than depleting a fixed token amount.

**Benchmarking APRs**: To gauge appropriate reward levels, you can:

* Browse existing campaigns on the [Merkl app](https://app.merkl.xyz) to see typical APRs in your category
* Use Merkl's [APR simulation tool](https://docs.google.com/spreadsheets/d/1suPzoiFfZP3_XdXNu9qh63gNUbbXaWoKuhy17FRCEIc/edit?usp=sharing) to model how different reward budgets translate to APRs at various TVL levels. To use this tool, you may simply duplicate the sheet and input your own numbers to simulate.
* Adjust your budget based on competitive rates and your growth targets

{% hint style="success" %}
**Start small, iterate often**: We recommend launching shorter campaigns initially (e.g., 2 weeks) to test performance and gather data. You can then create follow-up campaigns with optimized parameters based on actual TVL, user engagement, and APR effectiveness. This iterative approach minimizes risk and maximizes ROI.
{% endhint %}

