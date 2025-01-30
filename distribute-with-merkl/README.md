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

### Token whitelisting

Before starting your campaign, you need to ensure that the token you want to use as reward with Merkl has been whitelisted. We usually process token whitelisting requests once a day, and it's a way for us to ensure that we will be able to properly compute APRs and that the token will be safe to use for our users.

You can find all whitelisted tokens by chain on this [page](https://app.merkl.xyz/integrations). If your token is not part of the list, please fill out the following [form](https://tally.so/r/3y2bqx).

### Merkl Terms

You must also make sure that you have read and understood [Merkl's Terms & Conditions](https://app.merkl.xyz/merklTerms.pdf). You will be required to agree to these terms during the campaign setup process.

## Step-by-step process

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
