---
description: >-
  Get your campaign on the Merkl app while keeping control of computational
  load.
---

# Encompassing Campaigns

Merkl's All Encompassing Campaigns enable protocols to distribute rewards without requiring Merkl's Engine to calculate reward allocation. In this model, partners provide some API endpoints containing opportunity data and rewards, which are distributed based on the provided output.

The Merkl Engine aggregates the computed reward data provided by the partner into a Merkle root and pushes it onchain, enabling users to claim their rewards.

Once [a reward update](../technical-overview.md#reward-updates) has been processed, it becomes immutable and cannot be reverted. Nevertheless, campaign data can be dynamically updated throughout the campaign duration by modifying the reward endpoint output, with the final update required before the campaign's end date.

## 🛠️ Campaign configuration

Campaigns creators must provide the following information upon creation:

* 📅 Start Date – The date on which the campaign starts.
* 📅 End Date – The date on which the campaign ends.
* 🎁 Reward url - A public endpoint returning a reward JSON file.
* 📊 Data url - A public endpoint returning a data JSON file.

### 1. 🎁 Reward JSON

The expected format for the JSON reward is the following

```
type EncompassingJSON = {
    // Token being distributed
    rewardToken: string;
    // User rewards
    rewards: {
        // recipient: user address
        [recipient: string]: {
            // Reason: how the user got the rewards
            [reason: string]: {
              // amount: amount with all decimals as a string
              amount: string;
              // timestamp: unix timestamp in seconds, date at which the reward is sent to the recipient (must not exceed the end date of the campaign)
              timestamp: string; 
            }
        };
    };
};
```

Example:

```
{
  "rewardToken": "0xE0688A2FE90d0f93F17f273235031062a210d691",
  "rewards": {
    "0x9f76a95AA7535bb0893cf88A146396e00ed21A12": {
      "epoch-1": {
        "amount": "40000000000000000000",
        "timestamp": "1732294694"
      }
    },
    "0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185": {
      "epoch-2": {
        "amount": "100000000000000000000",
        "timestamp": "1741370722"
      }
    }
  }
}
```

* **rewardToken:** The token being distributed.
* **rewards:** User rewards with recipient addresses, reasons, amount and timestamp for the rewards.

Good to know:

* `timestamp` is the unix timestamp in seconds, date at which the reward is sent to the recipient. This timestamp must not exceed the end date of the campaign (it will not be processed if it's after the end date), and if it's before the start date of the campaign, the reward will be sent to the recipient at the start date.
* Once the engine has processed some rewards, even if the reward endpoint is updated, the rewards already distributed can't be reverted. If a campaign creator needs to send additional rewards to the same user, he can do it by adding another reason for the same recipient.

Example:

```
{
  "rewardToken": "0xE0688A2FE90d0f93F17f273235031062a210d691",
  "rewards": {
    "0x9f76a95AA7535bb0893cf88A146396e00ed21A12": {
      "epoch-1": {
        "amount": "40000000000000000000",
        "timestamp": "1732294694" // at this timestamp, `40000000000000000000` tokens will be claimable by the recipient
      },
      "epoch-2": { // new reason for the same recipient
        "amount": "100000000000000000000",
        "timestamp": "1741370722" // at this timestamp, `100000000000000000000` additional tokens will be claimable by the recipient
      }
    },
    "0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185": {
      "epoch-2": {
        "amount": "100000000000000000000",
        "timestamp": "1741370722"
      }
    }
  }
}
```

{% hint style="warning" %}
**Keep recent history:** Include at least 3/4 days of past entries in your Reward JSON so that, if there are delays, the Merkl engine can still fetch and process recent rewards without requiring you to re-upload old data.
{% endhint %}


### 2. 📊 Data JSON

The expected format for the JSON data is the following

```
type DataJSON = {
  tvl: string;
  apr: string; // in decimal
  opportunityName: string; // optional
}
```

Example:

```
{
  "tvl": "100000",
  "apr": "0.5",  // 0.5 in decimal = 50% APR
  "opportunityName": "Testing distribution aglaMerkl"
}
```

This data will be used to display the campaign in the Merkl frontend.

### ⚠️ Important Note

Merkl applies a 0.5% fee to this type of campaigns. This fee is added on top of the total airdropped amount, ensuring recipients receive the full intended distribution.

* 💡 If you want exactly 100,000 tokens to be distributed to users, you need to provide 100,502.51 tokens (calculated as 100,000 / (1 - 0.5%)).
* 💡 If you prefer to send exactly 100,000 tokens from your wallet, then the total sum of allocations in your JSON file should be 99,500 tokens (calculated as 100,000 \* (1 - 0.5%)).

Our frontend automatically calculates the correct amount for you.

### ⏳ Distribution lag

Tokens become claimable at the next [reward update](../technical-overview.md#reward-updates) on the target chain, which typically occurs within 8 hours. If you plan to announce the distribution, we recommend waiting until the rewards are claimable to notify your users.
