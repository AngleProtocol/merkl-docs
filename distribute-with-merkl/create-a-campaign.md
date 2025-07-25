---
description: Everything you need to know to create campaigns on Merkl
---

# Create a campaign

## 🪜 Step-by-step process for single campaign creation

{% hint style="info" %}
Want to create multiple campaigns at once as part of your program? Skip to the [Batch Campaigns Creation section](create-a-campaign.md#create-batch-campaigns-or-multiple-campaigns-at-once)
{% endhint %}



1. **Go to Merkl Studio**\
   [Merkl Studio](https://studio.merkl.xyz/) is your command center to launch, manage and optimize your incentive campaigns.

<figure><img src="../.gitbook/assets/Capture d’écran 2025-06-10 à 17.41.52 1.png" alt=""><figcaption><p>Merkl Studio homepage</p></figcaption></figure>



2. **Connect your wallet**

Connect the wallet holding the rewards to distribute. Make sure you’re connected on the chain on which you want to distribute the rewards. To see all chains that Merkl supports, check the [Status page](https://app.merkl.xyz/status) in the Merkl App.

<figure><img src="../.gitbook/assets/Group 8.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Capture d’écran 2025-06-10 à 17.42.03 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
You can create cross-chain campaigns with Merkl — incentivize an asset on one chain while distributing the rewards on another (e.g. incentivize an WETH-USDC pool on Ethereum by distributing WBTC on Base).
{% endhint %}



3. **Click on&#x20;**_**Create Campaign**_

Click on the _Create Campaign_ button to create a single campaign.

<figure><img src="../.gitbook/assets/Group 7.png" alt=""><figcaption></figcaption></figure>



4. **Select your incentive category**

Choose your incentive category by clicking on the appropriate card.

<figure><img src="../.gitbook/assets/Capture d’écran 2025-06-10 à 17.42.21 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
To incentivize Aave lending positions, Uniswap V2, or similar liquidity pools, please use the _Token Holding_ campaign category.
{% endhint %}



5. **Configure the campaign**

Fill out campaign details such as the asset to incentivize, the reward amount to distribute, a start date and an end date, etc. The details required will depend on the campaign type you selected. Please check the [campaign types page](../merkl-mechanisms/campaign-types/) to get detailed explanations of each required parameter.

<figure><img src="../.gitbook/assets/Capture d’écran 2025-06-10 à 17.42.59 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The total rewards amount is the total amount of rewards to be distributed over the whole campaign duration (a maintenance fee may be applied on top).
{% endhint %}

{% hint style="info" %}
For each token to distribute as reward, Merkl sets a minimum amount of token that can be distributed per hour. If your token amount is too low (generally parameters are calibrated so that you cannot distribute less than $1 per hour), then you will not be able to create your campaign.
{% endhint %}



6. **Personalize your campaign with customization options**

Campaigns on Merkl can be fully tailored to your needs. You can choose whether or not to apply customization options such as boosts, forwarders and more. New options are constantly being added to Merkl! You can check out the non-exhaustive list of Merkl customization options [here](../merkl-mechanisms/customization-options.md).

<figure><img src="../.gitbook/assets/Capture d’écran 2025-06-10 à 17.43.49 1.png" alt=""><figcaption></figcaption></figure>

You may also be asked to provide a deposit URL — this is the link to the asset you want to incentivize (e.g. a lending market, liquidity pool, etc.). Since Merkl is non-custodial, users must interact directly with the protocols to be eligible for rewards. Merkl does not hold any user funds.



7. **Review campaign and accept Terms**

Double-check all the information. Once you’re done, hit the _Accept Terms_ button, and confirm in your wallet.

<figure><img src="../.gitbook/assets/Group 9.png" alt=""><figcaption></figcaption></figure>



8. **Approve and launch**

Approve rewards transfer from your wallet to Merkl, then create the campaign by signing the transaction. You can sign and submit using either an EOA account or a multisig wallet.

Regardless of the method you choose, you will need to follow these 3 steps:

* accept the T\&C
* approve the tokens for transfer
* deposit the tokens

{% hint style="info" %}
**Using an EOA Account:**

* Double-check your campaign configuration.
* Read and accept Merkl's T\&Cs by clicking on the _Accept_ button and signing using your wallet.
* Approve the tokens for transfer and deposit the amount you want to incentivize, plus the maintenance fee of 3%.
{% endhint %}

{% hint style="info" %}
**Using a multisig wallet (Safe Wallet):**

The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder. To learn how to deploy your campaign from a multisig or Gnosis Safe Transaction Builder, check this [page](deploy-your-campaign-from-a-multisig-or-gnosis-safe.md) where everything is explained in more detail.
{% endhint %}

Congratulations! You have launched your incentive campaign! 🎉

{% hint style="success" %}
**Please note that once created, your campaign may take up to one hour to become visible on the front-end.**
{% endhint %}

## ⏭️ Create batch campaigns or multiple campaigns at once

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
