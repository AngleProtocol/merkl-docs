# Airdrop Campaigns

Merkl supports airdrop campaigns, simplifying the process for protocols looking to airdrop tokens to their community by allowing them to get it done in a few minutes with just a few clicks while minimizing gas fees.

Merkl supports two different types of airdrops:

1. **Custom Allocation Airdrops**: Campaign creators define the reward distribution, specifying which users receive what amount. Providers upload a JSON file with allocation details, and Merkl handles the distribution via its claim system.
2. **Snapshot-Based Airdrops**: Merkl distributes rewards based on a snapshot of a given token’s balance at a chosen point in time.

## JSON airdrops

For Custom Allocation Airdrops, providers only need to upload a JSON file specifying user addresses and reward amounts. To create your JSON Airdrop campaign on Merkl, visit this [page](https://studio.merkl.xyz/create-campaign/airdrop).

The expected format for the JSON is the following

```
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

Example:

```
{
  "rewardToken": "0x1a7e4e63778B4f12a199C062f3eFdD288afCBce8",
  "rewards": {
    "0x529619a10129396a2F642cae32099C1eA7FA2834": {
      "reason1": "1827034243855622331"
    },
    "0x0C2553e4B9dFA9f83b1A6D3EAB96c4bAaB42d430": {
      "reason1": "1827034243855622331",
      "reason2": "1035298980258165611"
    }
  }
}
```

* **rewardToken:** The token being distributed.
* **rewards:** User rewards with recipient addresses and reasons for the rewards.

### Important Note

Merkl applies a 0.5% fee to airdrop campaigns. This fee is added on top of the total airdropped amount, ensuring recipients receive the full intended distribution.

* If you want exactly 100,000 tokens to be distributed to users, you need to provide 100,502.51 tokens (calculated as 100,000 / (1 - 0.5%)).
* If you prefer to send exactly 100,000 tokens from your wallet, then the total sum of allocations in your JSON file should be 99,500 tokens (calculated as 100,000 \* (1 - 0.5%)).

Our frontend automatically calculates the correct amount for you.

### Distribution lag

Airdropped tokens become claimable at the next [reward update](../../glossary.md#reward-update-aka-merkl-root-update) on the target chain, which typically occurs within 8 hours. If you plan to announce the airdrop, we recommend waiting until the rewards are claimable to notify your users.

## Token Balance Airdrops

Merkl allows you to distribute tokens to holders based on their snapshotted balance at a chosen moment in time.

For this type of campaign, in addition to the reward amount, campaign creators must specify:

* Snapshot Date – The balance snapshot is taken at the last block before this date.
* Distribution Date – The date on which rewards should be distributed.
* Snapshot Name – A label for your snapshot.
