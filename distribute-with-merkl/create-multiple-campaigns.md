---
description: Everything you need to know to create multiple campaigns in batch on Merkl
---

# Create Multiple Campaigns

While [Merkl Studio](https://studio.merkl.xyz) is ideal for creating standard campaigns, Merkl also provides tooling for campaign creators who need to create multiple (and potentially advanced) campaigns at once.

This feature enables campaign creators to launch multiple campaigns sharing the same base parameters (creator address, reward token, reward chain, start and end time) in a single transaction.

All batch campaign creation methods involve generating [campaign configurations](../merkl-mechanisms/campaignConfiguration.md) and their corresponding payloads. This guide walks you through configuration and payload generation. Once you have a payload, refer to [Create a Campaign from a Multisig or Gnosis Safe](./create-your-campaign-from-a-multisig-or-gnosis-safe.md) for execution instructions.

## Method 1: Using Campaign Configurations (Recommended method)

This method allows you to generate a campaign payload using full [campaign configurations](../merkl-mechanisms/campaignConfiguration.md).

### 1. Retrieve Campaign Configurations

Use this [endpoint](https://api.merkl.xyz/docs#tag/config/get/v4/config/{id}) to retrieve campaign configurations from existing campaigns.

### 2. Adjust Parameters

Modify the parameters in the configurations according to your needs. Refer to the [campaign configuration documentation](../merkl-mechanisms/campaignConfiguration.md) for details on available parameters.

{% hint style="info" %}
Before generating your payload, you can preview how your campaigns will appear using the [campaign preview endpoints](../merkl-mechanisms/campaignConfiguration.md#previewing-campaigns-before-deployment) to validate your configurations and catch potential issues early.
{% endhint %}

### 3. Generate the Payload

Use [this endpoint](https://api.merkl.xyz/docs#tag/config/POST/v4/config/encode/batch/safe) to generate your Safe-compatible transaction payload.

In the request body, provide your configurations as a JSON array:

```json
[{
    "distributionChainId": 56,
    "campaignId": "0xcadb252f36c79aacbd6c96ce2af6cee374c2b0a0929ef6878525bee24987ed5e",
    "amount": "5000000000000000000000000",
    "computeChainId": 56,
    "creator": "0x67C06896Efb9Ce0A14C32B597Fa8bAa3f2659e7D",
    "startTimestamp": 1763636400,
    "rewardToken": "0xEdBeBe204Ef070B6880E07A28b55edc7748C24BA",
    "distributionMethodParameters": {
        "distributionMethod": "DUTCH_AUCTION",
        "distributionSettings": {}
    },
    "campaignType": 18,
    "endTimestamp": 1764068400,
    "blacklist": [],
    "whitelist": [],
    "forwarders": [],
    "targetToken": "0x5029f49585D57ed770D2194841B5A0bE06BFc2ED"
}]
```

{% hint style="info" %}
For additional encoding options to create your campaigns onchain (direct transaction data, single campaign encoding, etc.), see the [encoding and decoding documentation](../merkl-mechanisms/campaignConfiguration.md#encoding-and-decoding-configurations).
{% endhint %}

## Method 2: Override Parameters from Existing Template Campaigns

This method uses **existing campaigns as templates**. Instead of specifying complete campaign configurations (as in Method 1), you only need to override specific parameters from template campaigns. Simply select existing campaigns that closely match your needs, modify the parameters you want to change, generate a payload, and execute it with a Safe.

### 1. Retrieve Campaign Templates

Select existing campaigns that most closely match what you plan to launch. We strongly recommend matching both the [campaign type](../merkl-mechanisms/campaign-types/README.md) and the [distribution type](../merkl-mechanisms/distributions.md) to minimize the number of parameters you need to modify and reduce the chance of errors.

Next, find the `DatabaseId` of the template campaign(s) you'll use as the base for your new campaigns:

1. Go to the [Merkl app](https://app.merkl.xyz/)
2. Select an opportunity and open the "Advanced" tab
3. Copy the `DatabaseId` value

### 2. Prepare the Campaign Payload

Use this [endpoint](https://api.merkl.xyz/docs#tag/campaigns/post/v4/campaigns/generate-payload) to generate your batch payload by providing your template `DatabaseId` value(s) (from Step 1) along with the parameters you want to override for each campaign.

**Request body structure:**

```json
{
  "creatorAddress": "0x...", // address in checksum format
  "rewardToken": "0x...", // checksum format
  "distributionChainId": 1, // chain where rewards will be distributed
  "startTimestamp": 1756512000, // Unix seconds
  "endTimestamp": 1756598400, // Unix seconds
  "campaignsParams": {
    "9427880006586247706": [// <-- template DatabaseId from Step 1
      {
        "amount": "85372895000000000000000", // in decimals
        "targetToken": "0x...", // or poolAddress / poolId / market / evkAddress
        "blacklist": [] // addresses to exclude (checksum)
      // ... override more parameters
      }
      // ...add more campaigns for this same template DatabaseId if needed
    ]
    // Add more DatabaseId, each with its own array of campaigns.
  }
}
```

#### Understanding Parameters

The endpoint uses two types of parameters:

**1. Base Parameters** (apply to all campaigns in the batch)

These parameters are shared across all campaigns. If you need different values for some campaigns (e.g., different reward token), create a separate payload.

- `creatorAddress`: The address creating the campaigns
- `rewardToken`: The token used for rewards across all campaigns
- `distributionChainId`: The chain ID where campaigns will run
- `startTimestamp`: The start date of the campaigns (in Unix timestamp format)
- `endTimestamp`: The end date of the campaigns (in Unix timestamp format)

{% hint style="info" %}
You can use [this converter](https://www.unixtimestamp.com/) to convert dates to Unix timestamps.
{% endhint %}

**2. Per-Campaign Parameters** (specific to each campaign)

These parameters go under `campaignsParams` and let you customize each campaign. The available fields correspond to the [campaign-specific parameters](../merkl-mechanisms/campaignConfiguration.md#campaign-specific-parameters) in each campaign configuration.

You can view all available parameters for a specific campaign using this [endpoint](https://api.merkl.xyz/docs#tag/config/get/v4/config/{id}).

Example: Campaign ID `13756496363331257690` - [https://api.merkl.xyz/v4/config/13756496363331257690](https://api.merkl.xyz/v4/config/13756496363331257690)

For simple use cases where you only want to change the campaign amount, you'll only need to modify the `amount` field.

#### Example

You want to generate a payload for the following campaigns:

- 2 Uniswap V3 campaigns (template `DatabaseId`: 9427880006586247706)
- 1 Morpho (supply at the market level) campaign with a fixed rate of 5% APR (template `DatabaseId`: 6937583984928148176)
- 1 Aave (supply side) campaign with a capped APR of 10% (template `DatabaseId`: 711211603263558496)

When using the API endpoint, the body will look like this:

```json
{
  "creatorAddress": "0xfC6C8e98c381d2320A12822be01F45307266ff5d",
  "rewardToken": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
  "distributionChainId": 1,
  "startTimestamp": 1761951600,
  "endTimestamp": 1764543600,
  "campaignsParams": {
    "9427880006586247706": [
      {
        "amount": "10000000000000000000000",
        "poolAddress": "0x2A2C512beAA8eB15495726C235472D82EFFB7A6B",
        "weightToken0": 1500,
        "weightToken1": 1500,
        "weightFees": 7000,
        "blacklist": [],
        "forwarders": [],
        "hooks": []
      },
      {
        "amount": "10000000000000000000000",
        "poolAddress": "0x2A2C512beAA8eB15495726C235472D82EFFB7A6B",
        "weightToken0": 1500,
        "weightToken1": 1500,
        "weightFees": 3000,
        "blacklist": [],
        "forwarders": [],
        "hooks": []
      }
    ],
    "3011317640800818752": [
      {
        "amount": "10000000000000000000000",
        "distributionMethodParameters": {
          "distributionMethod": "FIX_APR",
          "distributionSettings": {
            "apr": "0.05",
            "market": "0xE3190143Eb552456F88464662f0c0C4aC67A77eB",
            "rewardTokenPricing": true,
            "targetTokenPricing": true,
          }
        }
      }
    ],
    "711211603263558496": [
      {
        "amount": "10000000000000000000000",
        "distributionMethodParameters": {
          "distributionMethod": "MAX_APR",
          "distributionSettings": {
            "apr": "0.1",
            "targetToken": "0xE3190143Eb552456F88464662f0c0C4aC67A77eB",
            "rewardTokenPricing": true,
            "targetTokenPricing": true,
          }
        }
      }
    ]
  }
}
```

## Method 3: Pre-Set Campaign Keys (Deprecated)

{% hint style="warning" %}
This method is deprecated. We recommend using Method 1 or Method 2 instead.
{% endhint %}

This method requires the Merkl team to pre-define campaigns in the backend. You then generate a payload by specifying the amount, dates, and chain for each pre-defined campaign.

### Setup

To get started, provide:

- The assets you'd like to incentivize
- Any [customization options](../merkl-mechanisms/customization-options.md) you'd like to apply

We'll save these configurations and generate the corresponding **keys** needed to launch your campaigns in bulk, which we'll share via a GitHub Gist.

Once configured, you'll be able to create multiple campaigns at once, all sharing the same base parameters:

- `program`: Provided by us—the internal ID of your incentive program
- `creator`: The Safe address that will execute the campaign payload
- `rewardToken`: In checksum format
- `distributionChainId`: The chain where rewards will be distributed
- `startTimestamp`: Campaign start time (Unix timestamp)
- `endTimestamp`: Campaign end time (Unix timestamp)

### Payload Generation

Generate your campaign payloads using this [endpoint](https://api.merkl.xyz/docs#tag/programpayload/POST/v4/program-payload/program/withAmounts).

To use it:

1. Input the base parameters listed above.
2. In the request body, paste the JSON file with the **keys and placeholder amounts** we provided via GitHub Gist (see example below).
3. For each key:
   * Replace the placeholder amount with the number of tokens you want to allocate (in raw units).
   * Use the correct number of decimals (e.g., `5000000000000000000` for 5 tokens if the token has 18 decimals).
   * **If you do not plan to incentivize a specific key, remove it entirely from the JSON rather than setting the amount to `0`.**
4. Click Send. If the payload is successfully generated, you'll be able to download it. If there's an error, reach out to us and we'll help troubleshoot.
5. Download the generated payload

### Example

Let’s say you’re a chain and want to incentivize:

* 5 Uniswap pools
* 2 Euler vaults

You would send us the addresses of the pools and vaults you want to incentivize. Once received, we’ll configure your setup and return the associated keys via a GitHub Gist.

The Gist will follow this format:

```json
{
    "ProtocolName1 Asset_Incentivized_1 ProgramName Address": "100000000000000000000",
    "ProtocolName1 Asset_Incentivized_2 ProgramName Address": "100000000000000000000",
    "ProtocolName2 Asset_Incentivized_3 ProgramName Address": "100000000000000000000"
}
```

For example, it would look like this:

```json
{
    "Uniswap USDC/WETH ProgramName 0x88e6a0c2ddd26feeb64f039a2c41296fcb3f5640": "100000000000000000000",
    "Uniswap WETH/USDT ProgramName 0xc7bbec68d12a0d1830360f8ec58fa599ba1b0e9b": "100000000000000000000",
    "Uniswap WBTC/WETH ProgramName 0x4585fe77225b41b697c938b018e2ac67ac5a20c0": "100000000000000000000",
    "Uniswap wstETH/WETH ProgramName 0x109830a1aaad605bbf02a9dfa7b0b92ec2fb7daa": "100000000000000000000",
    "Uniswap WBTC/cbBTC ProgramName 0xe8f7c89c5efa061e340f2d2f206ec78fd8f7e124": "100000000000000000000",
    "Euler Supply WETH ProgramName 0xD8b27CF359b7D15710a5BE299AF6e7Bf904984C2": "100000000000000000000",
    "Euler Borrow USDC ProgramName 0xE62055e3f732AB523FB57dDfAbA873a19C8A2CF8": "100000000000000000000"
}
```

The values(`100000000000000000000`) are placeholder values. When creating your campaigns, you’ll need to replace them with the actual amounts you want to allocate. _All amounts must be entered in raw format using the correct token decimals._

Then proceed with the steps outlined in the Payload Generation section above.

### Considerations Before Generating a Payload

* **Minimum Rewards Threshold:** Each campaign must meet the minimum hourly token distribution (typically ≥ ~$1/hour). If a campaign in the payload falls below this threshold, the payload will not be generated and will return an error message.
* **Duplicate Campaigns:** If you're reusing the same keys or configuration to increase rewards for an existing campaign, you’ll need to modify the `startTimestamp` or `endTimestamp` slightly (e.g., by 1 second) to avoid a duplicate campaign error.
* **Payload Size Limitations:** If your payload is too large, Safe may fail to execute the transaction. In that case, split your campaigns into multiple smaller batches. Creating up to \~20 campaigns at once typically works fine.
* **Decimal Precision:** Ensure the amounts you distribute match the token's decimals. For example, for an 18-decimal token, `1 token = 1000000000000000000`.
