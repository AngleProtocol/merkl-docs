---
description: Everything you need to know to create multiple campaigns at once on Merkl
---

# Create multiple campaigns

## 🔀 Step-by-step process for multiple campaigns creation

This guide provides a step-by-step process for creating a payload to launch multiple campaigns simultaneously using existing campaign models.

A multi-campaign payload allows you to create multiple campaigns in a single transaction, saving time and gas costs. You can use existing campaigns as templates and customize their parameters to suit your needs. This guide covers how to retrieve campaign models, generate a payload, and execute it using a Safe Wallet.

## 1. Retrieve the campaign templates you want to use

To create a multi-campaign payload, you first need to identify the campaign models you want to use as templates.

- Find the `campaignId` (0x...) of the campaign model you want to use. To find it, go in our app [here](https://app.merkl.xyz/), select the opportunity and go in the "Advanced" tab.
- Input the `campaignId` in this route [GET /v4/campaigns API endpoint](https://api.merkl.xyz/docs#tag/campaigns/get/v4/campaigns/), and note down the `id` (not the `campaignId`) which appears at the first line in the response (e.g 3011317640800818752). This is the `id` you'll use in the next step for each campaign of the batch.

## 2. Prepare the multi-campaigns payload

To generate a multi-campaigns payload from the selected campaign models, use this [endpoint](https://api.merkl.xyz/docs#tag/campaigns/post/v4/campaigns/generate-payload).

We strongly encourage you to retrieve an `id` of a campaign that is as close as possible to the one you wish to create, so that you minimize the number of parameters to edit and reduce the risk of errors.

The list of parameters you can override when generating the payload are the ones in the `params` field of the response. Depending on the campaign type selected, you will be able to edit the relevant parameters. Also, for each campaign, you will always be able to edit the `amount` of tokens to distribute.

For each campaign in the batch, you will have to provide the `id` from Step 1 (e.g 3011317640800818752) in the `campaignsParams` parameter.

**The following parameters will apply to the whole campaign batch. That means if you want to create a campaign with one different parameter, you'll have to create another payload.**

- `creatorAddress`: the address creating the campaigns
- `rewardToken`: the token used for rewards across all campaigns
- `distributionChainId`: the chainId where campaigns will run
- `startTimestamp`: the start date of the campaigns (in unix time stamp format)
- `endTimestamp`: the end date of the campaigns (in unix time stamp format)

You can also override the following parameters:
- `computeChainId` (make sure it's a chain we already support)
- `hooks`
- `APR`: you can edit the the APR type (variable, fixed, capped) of the template campaign by modyfing the following section: 

```
"distributionMethod": "FIX_APR",
"distributionSettings": {
"apr": "0.16",
}
```


Here is the list of the relevant parameters depending on the campaign type used as a template:

**ERC20 campaigns:**
- `targetToken`: the 0x address of the token you want to incentivize holding (for token holding campaigns) or the 0x address of the LP token for the underlying incentivized v2 pool

**UniV3 campaigns:**
- `poolAddress`: the 0x address of the incentivized pool
- `weightFees`: the weight for the rewards split associated to the total fees generated
- `weightToken0`: the weight for the rewards split of token 0 in the pool
- `weightToken1`: the weight for the rewards split of token 1 in the pool
- `isOutOfRangeIncentivized`: if you want to reward out-of-range positions or not

**UniV4 campaigns:**
- `poolId`: the 0x address of the incentivized pool
- `weightFees`: the weight for the rewards split associated to the total fees generated
- `weightToken0`: the weight for the rewards split of token 0 in the pool
- `weightToken1`: the weight for the rewards split of token 1 in the pool
- `isOutOfRangeIncentivized`: if you want to reward out-of-range positions or not

**Morpho single token campaigns:**
- `targetToken`: the 0x address of the token supplied on any Morpho Market

**Euler supply campaigns:**
- `targetToken`: the 0x address of the incentivized vault
- `addressAsset`: the 0x address of the supplied token

**Euler borrow campaigns:**
- `targetToken`: the 0x address of the incentivized vault
- `addressAsset`: the 0x address of the borrowed token 

Once you have completed all the parameters, you can generate the payload.

Here is an example of a UniV3 campaign creation (`id`: 9427880006586247706) and a lending campaign on Aave (`id`: 711211603263558496). When using the API route, the body will look like this:

```json
{
"creatorAddress": "0xF057afeEc22E220f47AD4220871364e9E828b2e9",
"rewardToken": "0x58D97B57BB95320F9a05dC918Aef65434969c2B2",
"distributionChainId": 1,
"startTimestamp": 1756512000,
"endTimestamp": 1756598400,
"campaignsParams": {
"9427880006586247706": [
{
"amount": "496250000000000000000000",
"poolAddress": "0x2A2C512beAA8eB15495726C235472D82EFFB7A6B",
"weightToken0": 3000,
"weightToken1": 2000,
"weightFees": 5000,
"blacklist": [],
"forwarders": [],
"hooks": []
},
{
"amount": "496250000000000000000000",
"poolAddress": "0xaCc2874ed22e811afdc47979c7b7985cCEd53b29",
"computeChainId": 8453,
"weightToken0": 3000,
"weightToken1": 2000,
"weightFees": 5000,
"blacklist": [],
"forwarders": [],
"hooks": []
}
],
"711211603263558496": [
      {
        "amount": "496250000000000000000000",
        "distributionMethodParameters": {
"distributionMethod": "FIX_APR",
"distributionSettings": {
"apr": "0.15",
"targetToken": "0xE3190143Eb552456F88464662f0c0C4aC67A77eB",
"symbolTargetToken": "aHorRwaRLUSD",
"rewardTokenPricing": true,
"targetTokenPricing": true,
"decimalsTargetToken": 18
}
}
      }
    ]
  }
}
```

## 3. Generate the multi-campaigns payload

The response contains two objects:

- `campaignPayloads`: A Safe payload for creating the campaigns on-chain
- `simulatedCampaigns`: A preview simulation of the campaigns that will be created from this payload

Once the payload is generated, you can check the `simulatedCampaigns` section to confirm the campaigns match your expectations (e.g., tokens, amounts, timestamps, etc), and preview what will be displayed in our app (title, description, etc)

Then, copy the `campaignPayloads` value from the response, and import it using the Gnosis Safe Transaction Builder to execute the onchain transactions that will create the campaigns.

You're done! The campaigns will appear in our app shortly!

## ⏭️ Create batch campaigns or multiple campaigns at once (2nd method)

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
   * Use the correct number of decimals (e.g. `5000000000000000000` for 5 tokens if the token has 18 decimals).
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
