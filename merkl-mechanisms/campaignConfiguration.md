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

#### Understanding Distribution Settings Parameters

**rewardTokenPricing**:

- `false`: The campaign distributes a fixed amount of reward tokens per unit (or dollar) of liquidity provided
- `true`: The campaign distributes a fixed dollar value of rewards per unit (or dollar) of liquidity provided

**targetTokenPricing**:

- `false`: Rewards are calculated per unit of liquidity provided
- `true`: Rewards are calculated per dollar of liquidity provided

We recommend using `targetTokenPricing = true` for most campaigns to ensure consistent reward distribution regardless of liquidity value fluctuations.

When both `rewardTokenPricing = true` and `targetTokenPricing = true`, the campaign pays a fixed APR.

{% hint style="info" %}
For most campaign types (such as token holding campaigns), the `targetToken` address is automatically derived from the campaign configuration and doesn't need to be specified manually within the `distributionSettings`.
{% endhint %}

#### Understanding the APR Value Format

The `apr` value in `distributionSettings` represents different metrics depending on your pricing configuration:

**Fixed APR** (`targetTokenPricing = true` and `rewardTokenPricing = true`):

- The value represents the target APR as a decimal
- Example: `"apr": "0.01"` = 1% APR, `"apr": "0.08"` = 8% APR

**Token-per-Dollar Rate** (`targetTokenPricing = true` and `rewardTokenPricing = false`):

- The value represents the number of reward tokens earned per dollar of liquidity per year
- Example: `"apr": "1"` = 1 reward token per year per $1 of liquidity provided
  - For $1,000 in liquidity: ~2.74 tokens per day (1,000 ÷ 365)
- To reward 1 token per day per $1,000 provided, use: `"apr": "0.365"`
  - Calculation: 1 token/day × 365 days = 365 tokens/year ÷ 1,000 dollars = 0.365 tokens per year per dollar

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

**Encoding configurations for onchain campaign creation:**

These endpoints convert campaign configurations into transaction data for calling the Merkl Distribution Creator contract:

- [Encode single campaign from config](https://api.merkl.xyz/docs#tag/config/POST/v4/config/encode) - Generate transaction data for one campaign
- [Encode multiple campaigns from their respective configs](https://api.merkl.xyz/docs#tag/config/POST/v4/config/encode/batch) - Generate transaction data for multiple campaigns

If you're deploying campaigns from a Gnosis Safe, use these endpoints instead to get Safe-compatible transaction payloads:

- [Get Safe payload from a single campaign config](https://api.merkl.xyz/docs#tag/config/POST/v4/config/encode/safe) - Generate Safe transaction payload for one campaign
- [Get Safe payload from multiple campaigns config](https://api.merkl.xyz/docs#tag/config/POST/v4/config/encode/batch/safe) - Generate Safe transaction payload for multiple campaigns

**Decoding onchain data into configurations:**

These endpoints convert various types of onchain and encoded data back into readable configuration objects:

- [Decode from onchain campaign ID](https://api.merkl.xyz/docs#tag/config/GET/v4/config/decode/onchain/{distributionChainId}/{campaignId}) - Retrieve configuration for an existing campaign
- [Decode from Database ID](https://api.merkl.xyz/docs#tag/config/GET/v4/config/decode/onchain/{distributionChainId}/{campaignId}) - Retrieve configuration for an existing campaign using its `DatabaseId`
- [Decode from raw onchain campaign data](https://api.merkl.xyz/docs#tag/config/POST/v4/config/decode/{distributionChainId}) - Parse encoded campaign data
- [Decode from Gnosis Safe payload](https://api.merkl.xyz/docs#tag/config/POST/v4/config/decode/safe) - Extract configuration from Safe transaction payload
- [Decode from transaction data](https://api.merkl.xyz/docs#tag/config/GET/v4/config/decode/{distributionChainId}/{payload}) - Parse campaign creation transaction data

**Previewing campaigns before deployment:**

These endpoints allow you to simulate how your campaign will appear and estimate its metrics before deploying it onchain:

- [Preview opportunity details](https://api.merkl.xyz/docs#tag/config/post/v4configopportunity) - See how your campaign will be categorized and displayed (opportunity name, grouping, etc.)
- [Estimate TVL](https://api.merkl.xyz/docs#tag/config/post/v4configtvl) - Calculate the expected Total Value Locked for your campaign configuration
