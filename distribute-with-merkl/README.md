---
description: Everything you need to know to create campaigns on Merkl
---

# 😽 Create a Merkl Campaign

## Getting started

### 📌 Before You Create a Campaign

Before setting up your campaign on Merkl, make sure you have read and understood:

- [How Merkl works](../mechanisms/technical-overview.md): A high-level overview of Merkl’s mechanisms and processes.
- [Configuring Your Incentive Mechanism](../mechanisms/incentive-mechanisms.md): A detailed guide on campaign types, distribution methods, and customization options.

Key Considerations:

- Ensure you understand how your selected campaign and distribution types work.
- Familiarize yourself with the hooks you plan to use for customization.
- Check for any additional features that may be relevant to your campaign.

{% hint style="info" %}
If you're looking for some incentive mechanism for which you're not sure about whether it's supported or looking to add incentive features or hooks that are not displayed on the platform, please contact us!
{% endhint %}

Some Merkl features (e.g., new distribution types, campaign types, hooks) may not be directly available in the campaign creation flow. If a required configuration option is not available in the frontend, we can:

- Help you generate the correct campaign payload.
- Provide dedicated API endpoints so you can structure your campaign as needed.

{% hint style="info" %}
Want to create multiple campaigns at once as part of your program? Skip to the [Batch Campaigns Creation](#create-batch-campaigns-or-multiple-campaigns-at-once)
{% endhint %}

### Token whitelisting

Before starting your campaign, you need to ensure that the token you want to use as reward with Merkl has been whitelisted. We usually process token whitelisting requests once a day, and it's a way for us to ensure that we will be able to properly compute APRs and that the token will be safe to use for our users.

You can find all whitelisted tokens by chain on this [page](https://app.merkl.xyz/integrations). If your token is not part of the list, please fill out the following [form](https://anglemoney.notion.site/1aecfed0d48c808a8194fe2befd50bdb?pvs=105).

### Merkl Terms

You must also make sure that you have read and understood [Merkl's Terms & Conditions](https://app.merkl.xyz/terms). You will be required to agree to these terms during the campaign setup process.

## Step-by-step process for single campaign creation

1. **Access the Campaign Creation Page**
   Go to the Merkl App and go to the campaign creation section by selecting _Create Campaign_ from the dashboard.

<figure><img src="../.gitbook/assets/create-campaign-screenshot.png" alt=""><figcaption></figcaption></figure>

3. **Connect your Wallet**

Connect your wallet and select the chain on which you want to distribute the rewards. **The chain connected to your wallet during the campaign creation will determine where the rewards are distributed, but not necessarily where you'll be incentivizing activity.** To see all the chains that Merkl supports, check this [page](https://app.merkl.xyz/integrations).

4. **Select your campaign type**

Once your token is whitelisted, you can choose your specific campaign type by clicking on the appropriate button (see screenshot below).

{% hint style="info" %}
While you may see various campaign types associated with some lending or borrowing protocols, if a lending protocol or a constant product AMM doesn't have its own card in the campaign creation page of the Merkl frontend, you may still incentivize it directly on Merkl using the **ERC20 Campaign** type.
{% endhint %}

<figure><img src="../.gitbook/assets/CLAMM-campaign-create-screenshot.png" alt=""><figcaption></figcaption></figure>

5. **Fill Out Campaign Details**

<figure><img src="../.gitbook/assets/CLAMM-fill-out-campaign-detail.png" alt=""><figcaption></figcaption></figure>

You will then be redirected to the campaign configuration page. The details required will depend on the campaign type you selected. Refer to [the documentation](../mechanisms/campaigns/README.md) for your specific campaign type or category. These pages provide detailed explanations of each required parameter.

In most campaigns, you will be required to input a rewards amount as well as start and end dates for the campaign. The total rewards amount is the total amount of rewards to be distributed over the whole campaign duration (a maintenance fee may be applied on top).

For each whitelisted token, Merkl sets a minimum amount of token that can be distributed per hour. If your token amount is too low (generally parameters are calibrated so that you cannot distribute less than \$1 per hour), then you will not be able to create your campaign.

While the Merkl frontend usually detects the protocols incentivized and will automatically link to the right frontend associated with the opportunity, campaign creators may provide onchain the deposit URL where users can participate in the campaign.

6. **Customize your campaign with hooks**

Campaigns on Merkl are fully customizable, and you may typically have to decide whether to modify the default behavior for the campaign.
Some of the hooks supported on Merkl are detailed [here](../mechanisms/hooks/README.md).

7. **Preview Transaction and Submit**

Double-check all the information entered for accuracy. Once you have finished configuring your campaign, proceed by pressing the _Preview Transaction_ button.

<figure><img src="../.gitbook/assets/CLAMM-preview-transaction.png" alt=""><figcaption></figcaption></figure>

8. **Sign and Submit**

You can sign and submit using either an EOA account or a multisig wallet. The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder. Regardless of the method you choose, you will need to follow these steps: accept the T\&C, approve the tokens for transfer, and deposit them.

- **Using an EOA Account:**
  - Double-check your campaign configuration.
  - Read and accept Merkl's T\&Cs by clicking on the _Accept_ button and signing using your wallet.
  - Approve the tokens for transfer and deposit the amount you want to incentivize, plus the maintenance fee of 3%.

<figure><img src="../.gitbook/assets/CLAMM-accept-approve-deposit.png" alt=""><figcaption></figcaption></figure>

After these steps, congratulations! You have created your Merkl Campaign!

{% hint style="info" %}
Please note that once created, your campaign may take up to one hour to become visible on the front-end.
{% endhint %}

- **Using a multisig wallet (Safe Wallet):**

The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder.

To learn how to deploy your campaign from a multisig or Gnosis Safe Transaction Builder, check this [page](./deploy-your-campaign-from-a-multisig-or-gnosis-safe.md) where everything is explained in more detail.

## Create batch campaigns or multiple campaigns at once
If you're planning to launch several campaigns simultaneously — for example, as part of a protocol-level program or a chain-wide initiative — please reach out to us on Telegram so we can better support you and fast-track the operational setup and onboarding. If we’re not already in contact, you can open a BD ticket on our [Discord](https://discord.gg/kZVG3T6Z)

### Set up

To get started, you’ll need to provide:
- The assets you'd like to incentivize
- Any optional custom hooks you'd like to apply (you can read more about supported hooks [here](https://docs.merkl.xyz/merkl-mechanisms/hooks))


Once provided, we’ll save this configuration on our end and generate the corresponding **keys** needed to launch your campaigns in bulk. We will share with you these keys via a GitHub Gist.

Once your configuration is set, you’ll be able to create multiple campaigns at once, all sharing the same following base parameters:
- `program`: Provided by us – the internal ID of your incentive program
- `creator`: The Safe address that will execute the campaign payload
- `rewardToken`: In checksum format
- `distributionChainId`: The chain where the rewards will be distributed
- `startTimestamp`: Campaign start time (Unix)
- `endTimestamp`: Campaign end time (Unix)

This setup is particularly useful for protocols or chains running recurring or large-scale programs. For example, a chain running a coordinated incentive program may want to incentivize its DEXes, lending protocols, vaults, and more — all with aligned campaign durations and launch timing.

### Payload Generation
You can generate your campaign payloads using this endpoint: [https://api.merkl.xyz/docs#tag/programpayload/POST/v4/program-payload/program/withAmounts](https://api.merkl.xyz/docs#tag/programpayload/POST/v4/program-payload/program/withAmounts).

To use it:

1. Input the base parameters listed above.
2. In the request body, paste the JSON file with the **keys and placeholder amounts** we provided via GitHub Gist. You can find an example below in the example section.
3. For each key:
   - Replace the placeholder amount with the number of tokens you want to allocate (in raw units).
   - Use the correct number of decimals (e.g. `5000000000000000000` for 5 tokens if the token has 18 decimals).
   - **If you do not plan to incentivize a specific key, do not set the amount to `0`. Instead, remove the key entirely from the JSON.**
4. Click Send. If the payload is successfully generated, you’ll be able to download it. If not, there may be an error, feel free to reach out to us — we’ll help troubleshoot.

5. Download the generated payload and drag it into the Safe Transaction Builder to execute it.

### Example
Let’s say you’re a chain and want to incentivize:
- 5 Uniswap pools  
- 2 Euler vaults  

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
The values(`100000000000000000000`) are placeholder values. When creating your campaigns, you’ll need to replace them with the actual amounts you want to allocate. *All amounts must be entered in raw format using the correct token decimals.*

Then proceed with the steps outlined in the Payload Generation section above.

### Considerations Before Generating a Payload

- **Minimum Rewards Threshold:** Each campaign must meet the minimum hourly token distribution (typically ≥ ~$1/hour). If a campaign in the payload falls below this threshold, the payload will not be generated and will return an error message.
- **Duplicate Campaigns:** If you're reusing the same keys to increase rewards for an existing campaign, you’ll need to modify the `startTimestamp` or `endTimestamp` slightly (e.g., by 1 second) to avoid a duplicate campaign error.
- **Payload Size Limitations:** If your payload is too large, Safe may fail to execute the transaction. In that case, split your campaigns into multiple smaller batches. Creating up to ~20 campaigns at once typically works fine.
- **Decimal Precision:**  Ensure the amounts you distribute match the token's decimals. For example, for an 18-decimal token, `1 token = 1000000000000000000`.
