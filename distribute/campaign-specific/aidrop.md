---
description: Guide for people using Merkl to aidrop rewards
---

# Airdrop tokens with Merkl

Everything you need to know to airdrop tokens with Merkl!

## Campaign configuration

With airdrops, you are responsible for allocating the rewards to the users and Merkl is responsible for handling the distribution of the tokens through its claim system.

To allocate the rewards you will need to upload a JSON file to IPFS with the following format:

### Creating the JSON file

#### JSON format

The json file should have the following format

```typescript
type AirdropJSON = {
    // Token being distributed
    rewardToken: string;
    // User rewards
    rewards: {
        // recipient: user address
        [recipient: string]: {
            // Reason: how the user got the rewards
            [reason: string]: string; // value: amount with all decimals as a string
        };
    };
};
```

#### JSON example

```json
{
  "rewardToken": "0x1a7e4e63778B4f12a199C062f3eFdD288afCBce8",
  "rewards": {
    "0x529619a10129396a2F642cae32099C1eA7FA2834": {
      "reason1": "18270342438556"
    },
    "0x0C2553e4B9dFA9f83b1A6D3EAB96c4bAaB42d430": {
      "reason1": "41411959210326626",
      "reason2": "10352989802581656"
    }
  }
}
```

One your JSON file is ready, you should ping us on Telegram so we can help you craft the campaign transaction (frontend is coming soon)
