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
If you're interested in running a
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

In this case, points are most likely accounted for in **an offchain database that you control**. This means that Merkl cannot mint points for you, it can just run computations based on onchain activity. Here you simply need to define the logic of what you want Merkl to track/index, Merkl tracks user actions on the chains/protocols of your choice, and you use the results however you like to credit and show points to your users.

**How does it work?**

### (Optional) 0. Merkl deploys for you a point token

To instruct the Merkl system to track activity on a protocol or on a token of your choice, you need to create a campaign with Merkl. The Merkl system is configured such that campaigns need a token to distribute. This token can be any ERC20 token, including a mock non-transferable token with no value attached to it.

As such to run a point program on Merkl, you need a token to distribute that will be used to instruct the Merkl system to track what you need it to track.

Merkl offers a template [available here](https://github.com/AngleProtocol/merkl-contracts/blob/main/contracts/partners/tokenWrappers/PointToken.sol) for non-transferable mock tokens. For any point campaign running on Merkl, we can of course deploy and mint for you a mock point token on the chain of your choice (usually a cheap chain).

Remember that with Merkl, where you create your campaigns is independent of the chain where activity is being tracked.

### 1. Create your campaigns using mock tokens to launch computation

Once you've got your mock tokens, you can setup campaigns on Merkl to mirror your intended logic (e.g., “1 mock token per \$1,000 deposited on this pool”). Note that campaigns can be created in a fully programmatic way through easy scripts, and that just like any campaign [you can create on Merkl](./create-a-campaign.md), Merkl has a lot of [customization options](../merkl-mechanisms/customization-options.md) available for how results will be computed.

One restriction is that you'll have to set a maximum amount of mock tokens you can allocate for your campaign. If you don't want to have any limit, just make sure not to enter a too high amount (like trillions of tokens) because otherwise it may mess up with Merkl's invariants and prevent the campaigns from being correctly computed.

For every campaign, you also have to set a start and an end date. Our recommendation, even if your points program lasts for 6 months is to create shorter campaigns that you renew periodically. It's hard to change once created the parameters of a live Merkl campaign, and it's better to reduce campaign durations, to be agile when it comes to updating parameters.

Be aware that Merkl has [a forwarder system](../merkl-mechanisms/features.md#-forwarders) and so if you're running a campaign tracking holders of your stablecoin, given Merkl protocol's coverage, it's very likely that Merkl will be able to detect other protocols through which your users may be holding the stablecoin and reward them accordingly. Unless you want a higher reward rate for users among these protocols or Merkl does not integrate forwarding for this protocol, it means that you will not need to create a new campaign for every protocol on which your users are likely to hold your tokens.

<figure><img src="../.gitbook/assets/Group 25.png" alt=""><figcaption><p>Once created point campaigns are visible in the Merkl app on the Points section</p></figcaption></figure>

The campaigns you’re creating will effectively distribute mock tokens, but these are **non-transferable**, and **users do not need to claim them** (users will not be able to see these campaigns on their dashboard).

### 2. Get the results via Merkl's API

As explained above, Merkl cannot mint points for you, rather you need to parse the results of these campaigns to know how to allocate your rewards.

To retrieve the list of eligible addresses as well as their relative contribution to the pool or the protocol that you are tracking you can use Merkl API.

. Reward amounts are expressed in mock token units.

Here are some of the routes that will be helpful to fetch the results of your campaigns:

- Retrieve all campaigns created by your address at `https://api.merkl.xyz/v4/campaigns?creatorAddress=<YOUR_ADDRESS>&test=true`\
  (e.g. [https://api.merkl.xyz/v4/campaigns?creatorAddress=0xA9DdD91249DFdd450E81E1c56Ab60E1A62651701\&test=true](https://api.merkl.xyz/v4/campaigns?creatorAddress=0xA9DdD91249DFdd450E81E1c56Ab60E1A62651701&test=true))
- Get the reward breakdown in mock token units for all the campaigns you created at `https://api.merkl.xyz/v4/rewards?chainId=&campaignId=<YOUR_CAMPAIGN_ID>&test=true`\
  (e.g. [https://api.merkl.xyz/v4/rewards?chainId=100\&campaignId=0x83adc24c9644324beebd26e6e2a7b9ffc14ce40d1d7cde309854ef79c9485c4c\&test=true](https://api.merkl.xyz/v4/rewards?chainId=100&campaignId=0x83adc24c9644324beebd26e6e2a7b9ffc14ce40d1d7cde309854ef79c9485c4c&test=true))
- Alternatively, we also offer a route that gives the list of addresses that were ever rewarded with a given reward token with Merkl, you can simply use this route with the mock token that you distribute (e.g [https://api.merkl.xyz/v4/rewards/token/?chainId=999&address=0x0A04dc9cBf6cf3BB216f24a501994eFfB2Aa8F6f&items=100&page=0](https://api.merkl.xyz/v4/rewards/token/?chainId=999&address=0x0A04dc9cBf6cf3BB216f24a501994eFfB2Aa8F6f&items=100&page=0))

For more info on how you can track the results of existing campaigns, you may also refer to [our campaign management page](./campaign-management.md).

{% hint style="warning" %}
Please note that the rewards endpoint is paginated — be sure to fetch all pages using `&page=<NUMBER>`
{% endhint %}

### **3. Normalize and customize**

Keep in mind that all the results given by the Merkl API are given in mock token units, and before allocating points for your users you can renormalize the results to fit your specific needs.

For example, if you create one campaign that distributes 1 point per \$1,000 deposited in a protocol, and another campaign that does the same for a different protocol, you can apply different multipliers as needed when allocating points to your users.

If you want to apply a 5× multiplier for the second protocol afterwards, you can simply mint 5x more points to recipients of this campaign — if address A has earned 1 mock token in reward from the second campaign, you can assign 5 points to that address.

This approach offers complete control over point allocation, allowing you to exclude certain campaigns, selectively boost rewards for specific users, or customize the system however you like.

If you don't want to have to renormalize and map 1:1 the results of the Merkl campaigns to points to assign to your users, the way you can set your points multipliers is by simply increasing the reward rate when you create your campaigns with Merkl.

### 4. **Display points on your own UI**

In this setup, you are responsible for displaying points on their own UI. However, you can easily build a leaderboard or dashboard using the data from Merkl’s mock campaigns via the Merkl API as seen above.

### 5. Run your airdrop

Once you've got your list of users along with the amount of points that they earned, you can simply parse it in a JSON file and use this file to run [your airdrop with Merkl](../merkl-mechanisms/campaign-types/airdrop.md).
