---
description: Guide to deploy a Merkl Campaign from a multisig or Gnosis Safe
---

# Deploy your campaign from a multisig or Gnosis Safe

The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder.

To deploy successfully your campaign, you will need to follow these steps: sign the T\&C conditions, approve the tokens for transfer, and deposit them.

## Signing the Terms & Conditions

Merkl requires anyone depositing incentives into the system to accept the Terms & Conditions. This is done by calling the `acceptConditions` function of the `DistributionCreator`. For those creating multiple campaigns in a single transaction batch, accepting the Terms & Conditions once is sufficient.

{% file src="../.gitbook/assets/Accept T&Cs JSON.json" %}

## Building the payload from the Merkl Frontend

Follow these steps to build the payload and finalize your campaign creation:

1. **Configure your campaign:**

* Go the the [Merkl campaign creation page](https://app.merkl.xyz/create) and fill in all the necessary data to configure your campaign.

2. **Build the Payload:**

* Once your configuration is complete, build the payload by clickcing on the Using <img src="https://raw.githubusercontent.com/AngleProtocol/angle-token-list/main/src/assets/tokens/SAFE.svg" alt="token" data-size="line"> Safe ? Build a payload. This option can be found at the bottom of the campaign creation page (see screenshot below).

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

3. Download or Copy the Safe Template

* Download the Safe Template (recommended), or copy the payload to your clipboard if you intend to customize it further. If you need help with customization, [reach out to us on Discord by opening a BD ticket. ](https://discord.com/channels/1209830388726243369/1210212731047776357)

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

4. &#x20;**Upload the JSON File:**

* Using the Gnosis Safe Transaction Builder, simply drag and drop the JSON file into the appropriate area, or click on "Choose a file" to upload it manually.

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

If you are using something other than the Transaction Builder, proceed as you are accustomed to.

5. **Execute the Transactions:**

* The app might tell you that the transaction batch is from another chain; you can disregard this message (the Merkl smart contract addresses are the same on all chains).

You will see the 3 following transactions:

* `approve`: Approval for the MerklDistributor contract to spend the reward tokens
* `acceptConditions`: Transaction to accept Merkl T\&Cs. It is mandatory to have this transaction the first time you create a campaign; you can remove it later if you wish.
* `createCampaign`: Transaction to create the campaign

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

* Click on "Create Batch" and then "Send Batch" to execute the transactions and deploy your Merkl Campaign.

Congratulations! You have successfully created your Merkl campaign!

## Create Multiple Campaigns in a single transaction batch

If you do not want to create multiple campaigns in a single transaction batch, you can skip this section. However, if you do wish to create multiple campaigns in one transaction batch, follow these steps:

1. **Retain Initial Transactions:**

* Keep the first three transactions from the first campaign safe template. These include `acceptConditions`, `approve`, and `createCampaign`.

2. **Download the Safe Template for Each other Campaign:**

* For subsequent campaigns, download the safe template (the process is the same as for the first campaign Safe Template). Each template will consist of the same three elements: `acceptConditions`, `approve`, and `createCampaign`.

3. **Compile the Transaction List:**

* Copy and paste the `acceptConditions`, `approve`, and `createCampaign` elements from each additional template into the list in the JSON of the first Safe Template. This will compile the transactions for all campaigns you intend to deploy.

4. **Optimize for Gas fees:**

* Note that you do not need to include `acceptConditions` for every campaign. Including it only in the first element of the list can help reduce gas fees, which is particularly beneficial for chains with high transaction costs.

5. **Save for Future Use:**

* Save the transaction batch in your library. This will streamline the process and allow you to create new campaigns faster in the future

By following these steps, you can efficiently deploy multiple campaigns in a single transaction batch. Congratulations on optimizing your campaign deployment process!

**Please note that once created, your campaign may take up to one hour to become visible on the front-end.**
