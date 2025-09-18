---
description: Everything you need to know to manage a point program with Merkl
---

# Manage a point program

## 🔀 Two ways to set up a point program

There are two main setups for running points programs with Merkl:

- **Tokenized points** (Pre-TGE tokens): Points are issued as ERC20 tokens that start out non-transferable, and later become transferable at TGE.
- **Non-tokenized points**: Points exist purely within Merkl’s indexing system (and optionally your own database), without requiring any token representation from the end user’s perspective.

{% hint style="success" %}

- Want a **fast setup** with Merkl’s UI showing APRs? Choose tokenized points and distribute them as non-transferable tokens before TGE.
- Want **maximum flexibility** and retain the custom logic enabled by Merkl campaigns? Go with non-tokenized points.

{% endhint %}

{% hint style="info" %}
To explore running a tokenized or non-tokenized point campaign with Merkl, please contact us to discuss support on the initial setup and pricing options.
{% endhint %}

Let’s break down each setup.

## Tokenized points (Pre TGE tokens)

In this model, **points are ERC20 tokens that are initially non-transferable**. From Merkl’s perspective, this works just like any regular token reward campaign—the key difference is that the distributed token represents points, which automatically become your project’s token once transferability is enabled.

At TGE, each point automatically converts **1:1 into real tokens**, making the transition seamless for users.

**How it works:**

1. Whitelist Merkl’s Creator and Distributor contracts so Merkl can distribute your points (non-transferable tokens).
2. Create campaigns using Merkl’s standard flow: define multipliers as reward rates and specify which onchain actions to track.
3. Users participate as usual and claim their non-transferable points through their Merkl dashboard.

With this setup, you get **all of Merkl’s built-in features**, including leaderboards and public campaign visibility in [the Merkl app](https://app.merkl.xyz/?tokenType=PRETGE&sort=tvl-desc).
.
Merkl can also assign an estimated value to your points directly in the UI, so users see APRs when farming. Typically, this estimate is based on your latest fundraising valuation or the price agreed with a CEX for an upcoming listing.

<figure><img src="../.gitbook/assets/Group 26.png" alt=""><figcaption><p>Tokenized point programs appear in the Merkl app under the Pre-TGE section</p></figcaption></figure>

{% hint style="success" %}
This is the most straightforward way to unlock Merkl’s full feature set while keeping control over point transferability and your future token.
{% endhint %}

## Non-tokenized points

In this case, points are most likely recorded in an off-chain database that you manage. This means Merkl cannot mint points directly for you—it can only compute rewards based on on-chain activity. You simply define the logic you want Merkl to track or index, Merkl monitors user actions across the chains and protocols you select, and you can then use the results however you like to credit and display points to your users.

**How does it work?**

### (Optional) 0. Deploy a point token with Merkl

To have Merkl track activity on a protocol or token of your choice, you need to create a campaign. The Merkl system is configured such that campaigns need a token to distribute. This token can be any ERC20, including a mock, non-transferable token with no monetary value.

Therefore, to run a point program with Merkl, you’ll need a token that serves as the distribution unit when instructing the system what to track.

Merkl provides a template [PointToken.sol](https://github.com/AngleProtocol/merkl-contracts/blob/main/contracts/partners/tokenWrappers/PointToken.sol)for such mock tokens. For any point campaign, we can also deploy and mint a mock point token for you on the chain of your choice (typically a low-cost chain). For your mock token, you can also configure:

- The token’s name and symbol
- The logo you’d like to use

Keep in mind that the chain where you deploy campaigns is independent of the chain where activity is being tracked.

### 1. Create campaigns using mock tokens to launch computation

Once your mock tokens are ready, you can set up campaigns on Merkl that mirror your intended logic (e.g., “1 mock token per \$1,000 deposited in this pool”). Campaigns can be created programmatically with simple scripts, and—just like any other Merkl campaign
—you’ll have access to extensive customization options
for how results are computed.

Once you've got your mock tokens, you can setup campaigns on Merkl that mirror your intended logic (e.g., “1 mock token per \$1,000 deposited on this pool”). Campaigns can be [created programmatically](./create-a-campaign.md) with simple scripts, and—just like any other Merkl campaign, you have access to extensive [customization options](../merkl-mechanisms/customization-options.md) for how results are computed.

**Allocations.** Each campaign requires a maximum allocation of mock tokens. If you don’t want a hard cap, avoid entering an excessively large number (e.g., trillions of tokens), as this can break Merkl’s invariants and prevent proper computation.

**Duration.** When setting up your campaigns, you’ll also need to set a start and end date. Even if your points program is meant to last 6 months, we recommend creating shorter campaigns and renewing them periodically. Campaign parameters are difficult to change once live, so shorter durations give you flexibility to adjust as needed.

**Forwarders.** Merkl has [a forwarder system](../merkl-mechanisms/features.md#-forwarders). For example, if your campaign tracks holders of a stablecoin, Merkl can often detect and reward users who hold that stablecoin indirectly through other protocols. This means you won’t need to create a separate campaign for each protocol—unless you want different reward rates or the protocol isn’t yet integrated with forwarding.

<figure><img src="../.gitbook/assets/Group 25.png" alt=""><figcaption><p>Once created point campaigns appear in the Points section of the Merkl app.</p></figcaption></figure>

The campaigns you create will distribute **non-transferable mock tokens**. Users do not need to claim them, and these campaigns will **not appear** on their dashboards.

### 2. Retrieve results via Merkl's API

As noted earlier, Merkl does not mint points directly. Instead, you parse the results of your campaigns to determine how to allocate rewards.

Using the Merkl API, you can fetch:

- The list of eligible addresses across one or all your campaigns
- Each address’s relative contribution to the pool or protocol being tracked

Reward amounts are expressed in units of the mock token you configured.

Below are some useful API routes for accessing your campaign results:

- Retrieve all campaigns created by your address at `https://api.merkl.xyz/v4/campaigns?creatorAddress=<YOUR_ADDRESS>&test=true`\
  (e.g. [https://api.merkl.xyz/v4/campaigns?creatorAddress=0xA9DdD91249DFdd450E81E1c56Ab60E1A62651701\&test=true](https://api.merkl.xyz/v4/campaigns?creatorAddress=0xA9DdD91249DFdd450E81E1c56Ab60E1A62651701&test=true))
- Get the reward breakdown in mock token units for all the campaigns you created at `https://api.merkl.xyz/v4/rewards?chainId=&campaignId=<YOUR_CAMPAIGN_ID>&test=true`\
   (e.g. [https://api.merkl.xyz/v4/rewards?chainId=100\&campaignId=0x83adc24c9644324beebd26e6e2a7b9ffc14ce40d1d7cde309854ef79c9485c4c\&test=true](https://api.merkl.xyz/v4/rewards?chainId=100&campaignId=0x83adc24c9644324beebd26e6e2a7b9ffc14ce40d1d7cde309854ef79c9485c4c&test=true))
  <<<<<<< HEAD
- # Alternatively, Merkl also provides a route that returns the list of all addresses that have ever been rewarded with a specific reward token. You can use this endpoint with your mock token to retrieve the full set of participants across all your campaigns along with their relative contribution. (e.g [https://api.merkl.xyz/v4/rewards/token/?chainId=999&address=0x0A04dc9cBf6cf3BB216f24a501994eFfB2Aa8F6f&items=100&page=0](https://api.merkl.xyz/v4/rewards/token/?chainId=999&address=0x0A04dc9cBf6cf3BB216f24a501994eFfB2Aa8F6f&items=100&page=0))
- Alternatively, we also offer a route that gives the list of addresses that were ever rewarded with a given reward token with Merkl, you can simply use this route with the mock token that you distribute (e.g [https://api.merkl.xyz/v4/rewards/token/?chainId=999&address=0x0A04dc9cBf6cf3BB216f24a501994eFfB2Aa8F6f&items=100&page=0](https://api.merkl.xyz/v4/rewards/token/?chainId=999&address=0x0A04dc9cBf6cf3BB216f24a501994eFfB2Aa8F6f&items=100&page=0))
  > > > > > > > 246ace8 (improve-points)

For more info on how you can track the results of existing campaigns, you may also refer to [our campaign management page](./campaign-management.md).

{% hint style="warning" %}
<<<<<<< HEAD
<<<<<<< HEAD
Please note that Merkl's rewards endpoint are paginated — be sure to fetch all pages using `&page=<NUMBER>`
=======
Please note that the rewards endpoint is paginated — be sure to fetch all pages using `&page=<NUMBER>`

> > > > > > > 246ace8 (improve-points)
> > > > > > > {% endhint %}

### **3. Normalize and customize**

<<<<<<< HEAD
All results from the Merkl API are expressed in mock token units. Before allocating points to your users, you can renormalize these results to fit your specific rules.

For example, if one campaign distributes 1 point per \$1,000 deposited in Protocol A, and another does the same for Protocol B, you can apply different multipliers for each protocol. Suppose you want a 5× multiplier for Protocol B: if address A earned 1 mock token from the second campaign, you can assign 5 points to that address.

This approach gives you full control over point allocation, letting you exclude certain campaigns, selectively boost rewards for specific users, or otherwise customize your system.

If you prefer not to renormalize later, you can set your multipliers directly when creating campaigns in Merkl by increasing the reward rate at creation.

### 4. **Display points on your own UI**

You are responsible for displaying points in your own interface. Using data from Merkl’s mock campaigns via the API, you can easily build a leaderboard, dashboard, or other visualizations.

### 5. Run your airdrop

# Once you have your list of users and their earned points, you can export it to a JSON file and use it to run your [your airdrop with Merkl](../merkl-mechanisms/campaign-types/airdrop.md).

# Keep in mind that all the results given by the Merkl API are given in mock token units, and before allocating points for your users you can renormalize the results to fit your specific needs.

Please note that Merkl's rewards endpoint are paginated — be sure to fetch all pages using `&page=<NUMBER>`
{% endhint %}

### **3. Normalize and customize**

All results from the Merkl API are expressed in mock token units. Before allocating points to your users, you can renormalize these results to fit your specific rules.

> > > > > > > afac97f (clean useless files)

For example, if one campaign distributes 1 point per \$1,000 deposited in Protocol A, and another does the same for Protocol B, you can apply different multipliers for each protocol. Suppose you want a 5× multiplier for Protocol B: if address A earned 1 mock token from the second campaign, you can assign 5 points to that address.

This approach gives you full control over point allocation, letting you exclude certain campaigns, selectively boost rewards for specific users, or otherwise customize your system.

If you prefer not to renormalize later, you can set your multipliers directly when creating campaigns in Merkl by increasing the reward rate at creation.

### 4. **Display points on your own UI**

You are responsible for displaying points in your own interface. Using data from Merkl’s mock campaigns via the API, you can easily build a leaderboard, dashboard, or other visualizations.

### 5. Run your airdrop

<<<<<<< HEAD
Once you've got your list of users along with the amount of points that they earned, you can simply parse it in a JSON file and use this file to run [your airdrop with Merkl](../merkl-mechanisms/campaign-types/airdrop.md).

> > > > > > > # 246ace8 (improve-points)
> > > > > > >
> > > > > > > Once you have your list of users and their earned points, you can export it to a JSON file and use it to run your [your airdrop with Merkl](../merkl-mechanisms/campaign-types/airdrop.md).
> > > > > > > afac97f (clean useless files)
