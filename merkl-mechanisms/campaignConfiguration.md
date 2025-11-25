---
description: Understanding Campaign Configurations on Merkl
---

# Campaign Configuration

A **campaign configuration** is the collection of all parameters—such as [campaign type](./campaign-types/README.md), [distribution method](./distributions.md), and [customization options](./customization-options.md)—that define how a campaign operates. It serves as the source of truth for the Merkl engine.

When a campaign is launched, an encoded version of this configuration is stored onchain in the Merkl Distribution Creator contract. The Merkl engine continuously monitors this contract to detect new campaigns, then parses each configuration to understand among others:

- What type of user activity to track (e.g., providing liquidity, holding tokens, lending assets)
- How to calculate user scores based on their participation
- How rewards should be distributed over time
- Which addresses are eligible or excluded from rewards

This configuration-driven approach allows Merkl to support diverse incentive mechanisms while maintaining a unified reward computation and distribution infrastructure.

## Creating and Retrieving Configurations

You can generate or retrieve campaign configurations in two main ways:

- **Via Merkl Studio**: When you create a campaign using [Merkl Studio](../distribute-with-merkl/create-a-campaign.md), the configuration is generated automatically.
- **Via Merkl API**: You can programmatically generate or retrieve configurations using the [Merkl API](../distribute-with-merkl/create-multiple-campaigns.md).

You can also retrieve the configuration of any existing campaign using its database ID via this [Merkl API endpoint](https://api.merkl.xyz/docs#tag/config/get/v4/config/{id}).

## Configuration Structure

A campaign configuration is typically represented as a JSON object containing various parameters.

### Common Parameters

These parameters are standard across most campaigns:

- `creator`: The address managing the campaign
- `rewardToken`: The address of the token distributed as rewards
- `distributionChainId`: The chain ID where rewards are distributed
- `computeChainId`: The chain ID where user activity is tracked (can differ from `distributionChainId` for cross-chain campaigns)
- `startTimestamp`: The start date of the campaign (Unix timestamp)
- `endTimestamp`: The end date of the campaign (Unix timestamp)
- `amount`: The total amount of rewards to be distributed
- `blacklist`: A list of addresses excluded from receiving rewards
- `whitelist`: A list of addresses allowed to receive rewards (if set, all others are excluded)
- `campaignType`: The [campaign type](./campaign-types/README.md)
- `computeScoreParameters`: The [scoring method](./scoring.md) used by the campaign
- `distributionMethodParameters`: The [distribution method](./distributions.md) for reward distribution

{% hint style="info" %}
You can use [this converter](https://www.unixtimestamp.com/) to convert dates to Unix timestamps.
{% endhint %}

### Distribution Methods

The `distributionMethodParameters` field defines how rewards are distributed over time.

**Variable Reward Rate (Dutch Auction):**

```json
{
  "distributionMethod": "DUTCH_AUCTION"
}
```

**Fixed APR:**

```json
{
  "distributionMethod": "FIX_APR",
  "distributionSettings": {
      "apr": "0.08",
      "targetToken": "0x...",
      "rewardTokenPricing": true,
      "targetTokenPricing": true
  }
}
```

**Capped APR:**

```json
{
  "distributionMethod": "MAX_APR",
  "distributionSettings": {
      "apr": "1",
      "targetToken": "0x...",
      "rewardTokenPricing": true,
      "targetTokenPricing": true
  }
}
```

### Campaign-Specific Parameters

Each campaign type has its own set of specific parameters in addition to the common parameters listed above.

**Examples by Campaign Type:**

- **ERC20 / Token Holding (Type 18)** - [Example config](https://api.merkl.xyz/v4/config/8270489034958466914)
  - `targetToken`: Address of the token to incentivize (or LP token for V2 pools)
- **Uniswap V3 (Type 2)** - [Example config](https://api.merkl.xyz/v4/config/9427880006586247706)
  - `poolAddress`: The pool address
  - `weightToken0`, `weightToken1`, `weightFees`: Weights for scoring liquidity
- **Uniswap V4 (Type 13)** - [Example config](https://api.merkl.xyz/v4/config/14050222419773482936)
  - Specific hook and pool parameters
- **Morpho (Type 57)** - [Example config](https://api.merkl.xyz/v4/config/3011317640800818752)
  - `targetToken`: Address of the supplied token on a Morpho Market
- **Euler Supply (Type 12)** - [Example config](https://api.merkl.xyz/v4/config/16912425279432080078)
  - `evkAddress`: Address of the incentivized vault
- **Euler Borrow (Type 12)** - [Example config](https://api.merkl.xyz/v4/config/17331543524323336682)
  - `evkAddress`: Address of the incentivized vault

## Encoding and Decoding Configurations

The Merkl API provides endpoints to convert between campaign configurations and the encoded format used onchain.

**Encoding configurations for onchain deployment:**

- [Encode single campaign from config](https://api.merkl.xyz/docs#tag/config/post/v4configencode)
- [Encode multiple campaigns from their respective configs](https://api.merkl.xyz/docs#tag/config/post/v4configencodebatch)

**Decoding onchain campaign data into configurations:**

- [Decode from onchain campaign ID](https://api.merkl.xyz/docs#tag/config/get/v4configdecodeonchaindistributionchainidcampaignid)
- [Decode from onchain campaign data](https://api.merkl.xyz/docs#tag/config/post/v4configdecodedistributionchainid)
