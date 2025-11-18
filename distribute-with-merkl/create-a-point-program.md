---
description: Everything you need to know to create and manage a point program with Merkl
---

# Create and Manage a Point Program

Merkl serves as a powerful indexing and computation engine for points programs, tracking onchain activity and calculating time-weighted user contributions. However, **Merkl does not mint or allocate points directly**—it provides the raw data and calculations that you can use however you see fit.

Think of Merkl as a flexible indexing tool: you define the logic you want to track (such as liquidity provision, token holdings, or protocol usage), Merkl monitors and computes user actions across the chains and protocols you select, and you then consume these results in whatever way best suits your program—whether that's crediting points 1:1, applying custom multipliers, filtering specific users, or combining data from multiple campaigns.

**You maintain full control over your points system.** Merkl simply provides the underlying tracking infrastructure and calculations. You're free to:
- Renormalize the data with custom multipliers
- Exclude or include specific campaigns or users
- Apply additional logic or filters before allocating points
- Display points in your own UI exactly as you envision

**Because Merkl is your indexing layer for your point program, your frontend should be the source of truth for points!**

In this page, we break down how to set up points tracking campaigns with Merkl and how to consume the results for your points system.

## Step by step process

### (Optional) 0. Deploy a point token with Merkl

To have Merkl track activity on a protocol or token of your choice, you need to create a campaign. The Merkl system is configured such that campaigns need a token to distribute. This token can be any ERC20, including a mock, non-transferable token with no monetary value.

Therefore, to run a point program with Merkl, you’ll need a token that serves as the distribution unit when instructing the system what to track.

Merkl provides a template [PointToken.sol](https://github.com/AngleProtocol/merkl-contracts/blob/main/contracts/partners/tokenWrappers/PointToken.sol) for such point tokens. For any point campaign, we can also deploy and mint a mock point token for you on the chain of your choice (typically a low-cost chain). For your point token, you can also configure:

- The token’s name and symbol
- The logo you’d like to use

Keep in mind that the chain where you deploy campaigns is independent of the chain where activity is being tracked.

Once deployed and whitelisted, this point token will be equivalent to an API key you can use to start tracking campaigns on Merkl.

### 1. Create campaigns using point tokens to launch computation

Once you've got your mock point token, you can setup campaigns on Merkl that mirror your intended logic (e.g., “1 point token per \$1,000 deposited on this pool”). Campaigns can be [created programmatically](./create-a-campaign.md) with simple scripts, and, just like any other Merkl campaign, you have access to extensive [customization options](../merkl-mechanisms/customization-options.md) for how results are computed.

**Allocations.** Each campaign requires a maximum allocation of point tokens. If you don’t want a hard cap, avoid entering an excessively large number (e.g., trillions of tokens), as this can break Merkl’s invariants and prevent proper computation. **We recommend setting a cap of 3 billion points**, this will ensure that:
- everyone with more than $1 will earn points
- you have a big enough budget to not have your campaign end early. 
You can set a higher budget, but only do so if you know that your campaign will distribute more than 3 billion points.

**Duration:** When setting up your campaigns, you’ll also need to set a start and end date. Even if your points program is meant to last 6 months, we recommend creating shorter campaigns and renewing them periodically. Campaign parameters are difficult to change once live, so shorter durations give you flexibility to adjust as needed.

**Forwarders:** Merkl has [a forwarder system](../merkl-mechanisms/reward-forwarding.md). For example, if your campaign tracks holders of a stablecoin, Merkl can often detect and reward users who hold that stablecoin indirectly through other protocols. This means you won’t need to create a separate campaign for each protocol—unless you want different reward rates or the protocol isn’t yet integrated with forwarding.

**Fixed reward rate campaigns:** Please make sure that Merkl knows how to price the asset you are incentivizing if you're configuring campaigns with a fixed point reward rate (e.g 1 point token per $1,000 deposited). To do so, either look for existing campaigns on our app, or check if all the assets involved in you campaign are priced by Merkl. When in doubt, reach out to the sales team on Telegram.

{% hint style="info" %}
**Understanding the associated virtual APR for points**: If you are reasoning in terms of points APR (meaning the APR in points if a point was worth $1):
 - Rewarding 1 point per $1 per day is equivalent to a 36500% APR (100% per day multiplied by 365)
 - Rewaring 1 point per $1000 per day is equivalent to 35.5% APR (0.1% per day multiplied by 365)
{% endhint %}

**CLAMM campaigns**: Fixed reward rates are not used for Concentrated Liquidity AMM campaigns because CLAMM distribution models are based on token0/token1 and fees (v3) or liquidity contribution (v4), not dollar values. For CLAMM campaigns, use variable reward rates instead: create a large budget and handle renormalization yourself based on the TVL during that period to properly distribute rewards. 

<figure><img src="../.gitbook/assets/Group 25.png" alt=""><figcaption><p>Once created point campaigns appear in [the Points section](https://app.merkl.xyz/?tokenType=POINT&sort=tvl-desc) of the Merkl app.</p></figcaption></figure>

The campaigns you create will distribute **non-transferable point tokens**. Users cannot claim these tokens through the Merkl UI—they exist purely for tracking purposes.

{% hint style="warning" %}
**Important**: All points data displayed in the Merkl frontend is informational only. Leaderboard rankings and point totals shown in the app should not be considered final, as projects retain full control to adjust allocations based on their own criteria before conducting airdrops or distributing rewards.

**Creator address filtering**: Fixed reward rate campaigns credit all remaining points to the creator address at the end of a campaign. Make sure to filter out creator addresses from your leaderboards when using API data to avoid showing them in rankings.
{% endhint %}

### 2. Retrieve results via Merkl's API

As noted earlier, Merkl does not mint points directly. Instead, you need to parse the results of your campaigns to determine how to allocate rewards.

Using the Merkl API, you can fetch:

- The list of eligible addresses across one or all your campaigns
- Each address's relative contribution to the pool or protocol being tracked

Reward amounts are expressed in units of the point token you configured.

{% hint style="info" %}
**API Update Frequency**

Once you create a campaign, API values update approximately **every 2 hours** when there are no delays. The schedule for API updates follows Merkl's [reward computation cycle](../merkl-mechanisms/technical-overview.md#reward-computation).

If there is a delay in the computation process or any other issue with the campaign, the campaign will not move forward and its associated API values will not be updated until the issue is resolved. You can monitor computation status for any given campaign on the [Merkl status page](https://app.merkl.xyz/status).
{% endhint %}

**Calculating total points**: When retrieving rewards from the API, each user's total points are calculated by adding the `amount` and `pending` fields together. Both fields represent computed values that you can use immediately for your points system.

{% hint style="info" %}
**Note**: The `amount` and `pending` fields are inherited from Merkl's token distribution model, but for points programs you don't need to distinguish between them - both are ready to use. To get a user's total points, use: `totalPoints = amount + pending`
{% endhint %}

[The Merkl API integration page](../integrate-merkl/api.md.md) provides all the resources you could need to fetch data related to your points campaigns on Merkl.

Below are some of the most useful API routes for accessing your campaign results:

- Retrieve all campaigns created by your address at `https://api.merkl.xyz/v4/campaigns?creatorAddress=<YOUR_ADDRESS>`\
  (e.g. [https://api.merkl.xyz/v4/campaigns?creatorAddress=0xA9DdD91249DFdd450E81E1c56Ab60E1A62651701](https://api.merkl.xyz/v4/campaigns?creatorAddress=0xA9DdD91249DFdd450E81E1c56Ab60E1A62651701))
- Get the reward breakdown in point token units for all the campaigns you created at `https://api.merkl.xyz/v4/rewards?chainId=&campaignId=<YOUR_CAMPAIGN_ID>`\
   (e.g. [https://api.merkl.xyz/v4/rewards?chainId=100&campaignId=0x83adc24c9644324beebd26e6e2a7b9ffc14ce40d1d7cde309854ef79c9485c4c](https://api.merkl.xyz/v4/rewards?chainId=100&campaignId=0x83adc24c9644324beebd26e6e2a7b9ffc14ce40d1d7cde309854ef79c9485c4c))
- Alternatively, Merkl also provides a route that returns the list of all addresses that have ever been rewarded with a specific reward token. You can use this endpoint with your point token to retrieve the full set of participants across all your campaigns along with their relative contribution. (e.g [https://api.merkl.xyz/v4/rewards/token/?chainId=999&address=0x0A04dc9cBf6cf3BB216f24a501994eFfB2Aa8F6f&items=100&page=0](https://api.merkl.xyz/v4/rewards/token/?chainId=999&address=0x0A04dc9cBf6cf3BB216f24a501994eFfB2Aa8F6f&items=100&page=0)). Beware that if you created campaigns that you then cancelled with a given reward token, the results of these campaigns will also be factored in the output of this API route.

{% hint style="info" %}
**Important: Understanding the `chainId` parameter**

When calling Merkl's API endpoints, the `chainId` parameter refers to **the chain where your point token is deployed**, not the chain where your campaigns are computing activity.

For example, if your point token is deployed on Gnosis (chainId: 100), use `chainId=100` in your API calls, even if your campaigns track activity on other chains.

This is because Merkl's API organizes rewards by the reward token's deployment chain, not by the chain where the tracked activity occurs.
{% endhint %}

### **3. Normalize and customize**

All results from the Merkl API are expressed in point token units. Before allocating points to your users, you can renormalize these results to fit your specific rules.

For example, if one campaign distributes 1 point per \$1,000 deposited in Protocol A, and another does the same for Protocol B, you can apply different multipliers for each protocol. Suppose you want a 5× multiplier for Protocol B: if address A earned 1 point token from the second campaign, you can assign 5 points to that address.

This approach gives you full control over point allocation, letting you exclude certain campaigns, selectively boost rewards for specific users, or otherwise customize your system.

If you prefer not to renormalize later, you can set your multipliers directly when creating campaigns in Merkl by increasing the reward rate at creation.

{% hint style="info" %}
For cancelled campaigns, you may need to renormalize the affected points or exclude the campaign’s results from your totals.
{% endhint %}

### 4. **Display points on your own UI**

You are responsible for displaying points in your own interface. Using data from Merkl’s mock campaigns via the API, you can easily build a leaderboard, dashboard, or other visualizations.

### 5. Run your airdrop

Once you have your list of users and their earned points, you can export it to a JSON file and use it to run your [your airdrop with Merkl](../merkl-mechanisms/campaign-types/airdrop.md).

## Pricing

Points tracking campaigns follow a straightforward pricing model based on the number of opportunities (point sources) you're monitoring:

**Standard Campaigns**:

- **$30 per week** per point source that can be created through the Merkl UI
- **Additional fee**: $0.015 per recipient if your campaign has more than 500 recipients

**Advanced Campaigns**:

- **One-time setup fee**: $300 for complex campaigns (e.g., Net Lending on Aave) that require custom development and testing by our team
- **Ongoing cost**: $30 per week after setup (same as standard campaigns)

{% hint style="info" %}
**What is a point source?**

A point source is the specific opportunity or asset that Merkl monitors to calculate points. Examples include:

- A Uniswap liquidity pool
- A Morpho lending market
- A vault or staking contract
{% endhint %}

### Cost optimization strategies

**Create longer-duration campaigns**: Pay the user fee once by creating campaigns with longer durations instead of multiple short campaigns.

**Enable forwarding to reduce campaign count**: Merkl's forwarding system can automatically detect and reward users who hold your tokens indirectly through other protocols (e.g., forwarding points earned on your token to users in Pendle). This reduces the number of campaigns you need to create.

**Free TGE distribution**: If you use Merkl post-TGE to incentivize your token, you get a free TGE distribution included.

### Invoicing

**Billing cycle**: You will be invoiced retroactively at the end of each month for all campaigns created during that period. If you cancel a campaing shortly after creating it, we usually waive it as a goodwill gesture but if it’s near a full week it would be billed.

**Payment terms**: Invoices must be paid within 7 days. If payment is not received within this timeframe, all ongoing campaigns will be paused and your project will be temporarily blocked from Merkl until payment is resolved.