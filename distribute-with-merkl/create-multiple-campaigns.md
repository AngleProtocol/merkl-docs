---
description: Everything you need to know to create multiple campaigns in batch on Merkl
---

# Create multiple campaigns

A multiple campaigns payload lets you create multiple campaigns sharing the same base parameters (creator address, reward token, reward chain, start and end time) in a single transaction. Batching will save you both time and gas.

## 🔀 Step-by-step process for multiple campaigns creation

This method uses **existing campaigns as templates**. You select past campaigns that are as close as possible to what you want, tweak a few parameters, generate a payload, and execute it with a Safe.

### 1. Retrieve the campaign templates you want to use

Choose past campaigns as close as possible to what you plan to launch. We strongly recommend matching both the [campaign type](https://docs.merkl.xyz/merkl-mechanisms/campaign-types) and the [distribution type](https://docs.merkl.xyz/merkl-mechanisms/distributions). This minimizes edits and reduces the chance of errors.

Next, find the `DatabaseId` of the template campaign(s) you will be using to derive your payload(s). To do this, go to the [Merkl app](https://app.merkl.xyz/), select an opportunity and open the "Advanced Tab". Then copy your template `DatabaseId`.

### 2. Prepare the multiple campaigns payload

This [endpoint](https://api.merkl.xyz/docs#tag/campaigns/post/v4/campaigns/generate-payload) will be how you’ll be able to build a batch using your selected template `DatabaseId` value(s) (from Step 1) and base parameters across all your campaign.

Request body structure (annoted)
```json
{
  "creatorAddress": "0x...", // address in checksum that will execute the payload (base parameter)
  "rewardToken": "0x...", // checksum format (base parameter)
  "distributionChainId": 1, // chain where rewards will be distributed (base parameter)
  "startTimestamp": 1756512000, // Unix seconds (base parameter)
  "endTimestamp": 1756598400, // Unix seconds (base parameter)
  "campaignsParams": {
    "9427880006586247706": [// <-- template DatabaseId from Step 1
      {
        "amount": "85372895000000000000000", // in decimals
        "targetToken": "0x...", // or poolAddress / poolId / market / evkAddress
        "blacklist": [], // addresses to exclude (checksum)
        "whitelist": [] // addresses to include (checksum)
      // ... override more parameters
      }
      // ...add more campaigns for this same template DatabaseId if needed
    ]
    // Add more DatabaseId, each with its own array of campaigns.
  }
}
```
#### Base parameters 
**Base parameters apply to the entire batch**. If one campaign needs a different base parameter (e.g., different reward token), you'd need to create a separate payload. 

The base parameters are:
- `creatorAddress`: the address creating the campaigns
- `rewardToken`: the token used for rewards for all campaigns
- `distributionChainId`: the chainId where campaigns will run
- `startTimestamp`: the start date of the campaigns (in Unix timestamp format)
- `endTimestamp`: the end date of the campaigns (in Unix timestamp format)

Note: you can go to this [page](https://www.unixtimestamp.com/) to retrieve the right Unix timestamps.

##### Per-campaign override parameters
Per-campaign overrides go under `campaignsParams`. Supported fields include:
 - `amount`: amount of rewards to be distributed in a campaign
 - `computeChainId`: input the id of a chain while making sure it's a chain we already support. Supported chains can be found on our [status page](https://app.merkl.xyz/status)
 - `blacklist`: the list of addresses you want to exclude from the campaign
 - `whitelist`: the list of addresses you want to include in the campaign (excluding all others)
 - `distributionMethodParameters`: depending on your distribution type
  - Variable reward rate:
```json
"distributionMethodParameters": {
                    "distributionMethod": "DUTCH_AUCTION"
                  } 
```
  - Fixed APR rate: 
 ```json
 "distributionMethodParameters": {
                    "distributionMethod": "FIX_APR",
                    "distributionSettings": {
                        "apr": "0.1",
                        "rewardTokenPricing": true,
                        "targetTokenPricing": true
                    }
                  }  
```

  - Capped APR rate: 
 ```json
 "distributionMethodParameters": {
                    "distributionMethod": "MAX_APR",
                    "distributionSettings": {
                        "apr": "0.1",
                        "rewardTokenPricing": true,
                        "targetTokenPricing": true
                    }
                  }  
```
**The best practice is to use a template campaign with the same distribution type as the one you plan to use**. Yet, even though we don’t recommend doing so, you can still edit the distributionType (variable, fixed, capped) of the template campaign. If you wish to do so, please **reach out to us directly to ensure your parameters are correct!**


For Fixed/Capped APR, please contact us to confirm when rewardTokenPricing and targetTokenPricing should be true or false.

#### Per-campaign override parameters (campaign-specific)
Here is the list of the relevant parameters depending on the campaign type used as a template:

**ERC20LOGPROCESSOR campaigns - CampaignType: 18** (e.g., `DatabaseId`: 8270489034958466914)
- `targetToken`: the 0x address of the token you want to incentivize holding (for token holding campaigns), or the 0x address of the LP token for the underlying incentivized v2 pool

**UniV3 campaigns - CampaignType: 2** (e.g., `DatabaseId`: 9427880006586247706)
- `poolAddress`: the 0x address of the incentivized pool
- `weightFees`: the weight of rewards based on total fees generated, in 10,000 basis points (e.g., 5000 = 50%)
- `weightToken0`: the weight of rewards for holding token0, in 10,000 basis points (e.g., 2500 = 25%)
- `weightToken1`: the weight of rewards for holding token1, in 10,000 basis points (e.g., 2500 = 25%)
- `isOutOfRangeIncentivized`: a boolean parameter. Set to true if you want to reward out-of-range positions

**UniV4 campaigns - CampaignType: 13** (e.g., `DatabaseId`: 14050222419773482936)
- `poolId`: the 0x address of the incentivized pool
- `weightFees`: the weight of rewards based on total fees generated, in 10,000 basis points (e.g., 5000 = 50%)
- `weightToken0`: the weight of rewards for holding token0, in 10,000 basis points (e.g., 2500 = 25%)
- `weightToken1`: the weight of rewards for holding token1, in 10,000 basis points (e.g., 2500 = 25%)
- `isOutOfRangeIncentivized`: a boolean parameter. Set to true if you want to reward out-of-range positions

**Morpho single token campaigns - CampaignType: 57** (e.g., `DatabaseId`: 3011317640800818752)
- `targetToken`: the 0x address of the token supplied on any Morpho Market

**Euler supply campaigns - CampaignType: 12** (e.g., `DatabaseId`: 16912425279432080078)
- `evkAddress`: the 0x address of the incentivized vault

**Euler borrow campaigns - CampaignType: 12** (e.g., `DatabaseId`: 17331543524323336682)
- `evkAddress`: the 0x address of the incentivized vault

Once you have completed all the parameters, you can generate the payload.

#### Example
You want to generate the payload for the following campaigns:
-  2 UniswapV3 campaigns (template `DatabaseId`: 9427880006586247706)
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

### 3. Generate the multiple campaigns payload

Then, call the endpoint to generate the Safe payload containing all campaigns. After downloading it, drag and drop that payload into Gnosis Safe Transaction Builder and execute it to create the campaigns. You can read more of this process in the [Create a campaign from a multisig or Gnosis Safe section](https://docs.merkl.xyz/distribute-with-merkl/create-your-campaign-from-a-multisig-or-gnosis-safe).

After executing it, you're done! The campaigns typically appear in the Merkl app within ~1 hour. You can view the latest created campaigns here: https://app.merkl.xyz/?sort=lastCampaignCreatedAt-desc&tokenType=all

## ⏭️ Create batch campaigns or multiple campaigns at once (deprecated method)

If you're planning to launch several campaigns simultaneously — for example, as part of a protocol-level program or a chain-wide initiative — please reach out to us on Telegram so we can better support you and fast-track the operational setup and onboarding. If we’re not already in contact, you can open a BD ticket on our [Discord](https://discord.gg/kZVG3T6Z)

### Set up

To get started, you’ll need to provide:

* The assets you'd like to incentivize
* Any customization options you'd like to apply (you can read more about supported customization options [here](https://docs.merkl.xyz/merkl-mechanisms/hooks))

Once provided, we’ll save this configuration on our end and generate the corresponding **keys** needed to launch your campaigns in bulk. We will share with you these keys via a GitHub Gist.

Once your configuration is set, you’ll be able to create multiple campaigns at once, all sharing the same following base parameters:

* `program`: Provided by us – the internal ID of your incentive program
* `creator`: The Safe address that will execute the campaign payload
* `rewardToken`: In checksum format
* `distributionChainId`: The chain where the rewards will be distributed
* `startTimestamp`: Campaign start time (Unix)
* `endTimestamp`: Campaign end time (Unix)

This setup is particularly useful for protocols or chains running recurring or large-scale programs. For example, a chain running a coordinated incentive program may want to incentivize its DEXes, lending protocols, vaults, and more — all with aligned campaign durations and launch timing.

### Payload Generation

You can generate your campaign payloads using this endpoint: [https://api.merkl.xyz/docs#tag/programpayload/POST/v4/program-payload/program/withAmounts](https://api.merkl.xyz/docs#tag/programpayload/POST/v4/program-payload/program/withAmounts).

To use it:

1. Input the base parameters listed above.
2. In the request body, paste the JSON file with the **keys and placeholder amounts** we provided via GitHub Gist. You can find an example below in the example section.
3. For each key:
   * Replace the placeholder amount with the number of tokens you want to allocate (in raw units).
   * Use the correct number of decimals (e.g., `5000000000000000000` for 5 tokens if the token has 18 decimals).
   * **If you do not plan to incentivize a specific key, do not set the amount to `0`. Instead, remove the key entirely from the JSON.**
4. Click Send. If the payload is successfully generated, you’ll be able to download it. If not, there may be an error, feel free to reach out to us — we’ll help troubleshoot.
5. Download the generated payload and drag it into the Safe Transaction Builder to execute it.

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

* **Minimum Rewards Threshold:** Each campaign must meet the minimum hourly token distribution (typically ≥ \~$1/hour). If a campaign in the payload falls below this threshold, the payload will not be generated and will return an error message.
* **Duplicate Campaigns:** If you're reusing the same keys to increase rewards for an existing campaign, you’ll need to modify the `startTimestamp` or `endTimestamp` slightly (e.g., by 1 second) to avoid a duplicate campaign error.
* **Payload Size Limitations:** If your payload is too large, Safe may fail to execute the transaction. In that case, split your campaigns into multiple smaller batches. Creating up to \~20 campaigns at once typically works fine.
* **Decimal Precision:** Ensure the amounts you distribute match the token's decimals. For example, for an 18-decimal token, `1 token = 1000000000000000000`.
