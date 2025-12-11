# Airdrop Campaigns

Merkl supports airdrop campaigns, simplifying the process for protocols looking to airdrop tokens to their community by allowing them to get it done in a few minutes with just a few clicks while minimizing gas fees.

Merkl supports two different types of airdrops:

1. **Custom Allocation Airdrops**: Campaign creators define the reward distribution, specifying which users receive what amount. Providers upload a JSON file with allocation details, and Merkl handles the distribution via its claim system.
2. **Snapshot-Based Airdrops**: Merkl distributes rewards based on a snapshot of a given token’s balance at a chosen point in time.

{% hint style="info" %}
Airdrops do not appear “live” like continuously computed campaigns because the Merkl Engine performs no ongoing calculation.\
\
To review your airdrop in the main opportunities page after creation, use this filtered view:\
[https://app.merkl.xyz/?status=LIVE%2CSOON%2CPAST\&sort=lastCampaignCreatedAt-desc](https://app.merkl.xyz/?status=LIVE%2CSOON%2CPAST\&sort=lastCampaignCreatedAt-desc)\
{% endhint %}

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

{% hint style="warning" %}
**Important**: All addresses in your JSON file MUST be in [checksummed format](https://eips.ethereum.org/EIPS/eip-55). Non-checksummed addresses will cause the campaign to fail. Use tools like [Etherscan](https://etherscan.io/) or ethers.js `getAddress()` to convert addresses to the correct format.
{% endhint %}

### Fees for airdrops

Merkl applies a 0.5% fee to airdrop campaigns. This fee is added on top of the total airdropped amount, ensuring recipients receive the full intended distribution.

* If you want exactly 100,000 tokens to be distributed to users, you need to provide 100,502.51 tokens (calculated as 100,000 / (1 - 0.5%)).
* If you prefer to send exactly 100,000 tokens from your wallet, then the total sum of allocations in your JSON file should be 99,500 tokens (calculated as 100,000 \* (1 - 0.5%)).

Our frontend automatically calculates the correct amount for you.

### Airdrop timing

Once an airdrop campaign is created and launched on Merkl, it usually takes up to **12 hours** before the **tokens become claimable** by users (as the airdrop becomes available at the next [reward update](../technical-overview.md#reward-updates) on the target chain).

{% hint style="warning" %}
We recommend until the rewards are claimable **to notify your community** that the airdrop is live.
{% endhint %}

As mentioned above, airdrops always appear as `PAST` on the Merkl app, with  **campaign end date** in the past. Note that the end date on the Merkl app is **different from the airdrop claim end date**: it is the date when new rewards stop distributing. Even after a campaign is marked as “ended” on the Merkl App, users can still claim their tokens via their Merkl dashboard.

{% hint style="warning" %}
The **campaign end date** on the Merkl App **cannot be modified**.
{% endhint %}

### Controlling Claim Dates with Token Wrappers

As explained [in this page of our docs](../features.md#-infinite-customizability-with-token-wrappers), it is possible to distribute wrappers of standard ERC20 tokens using Merkl.

In the context of an airdrop, we offer a wrapper type that allows you to retain the tokens you airdrop in your wallet and maintain full control over when they become claimable by users.

Once your wrapper is deployed and you've minted wrapper tokens, the process works as follows:

1. Create your airdrop campaign on Merkl, distributing your wrapper token instead of the underlying token you want to airdrop
2. Wait for a reward update for the wrapper tokens to become claimable on Merkl
3. The wrapper tokens are typically marked as test tokens, making them invisible on the frontend
4. Users can only claim the actual tokens once you:
   * Approve the wrapper token contract to spend the underlying token
   * Hold the underlying tokens in your address

This approach is particularly useful for TGE (Token Generation Event) processes where tokens must only become claimable after a specific date chosen by the campaign creator.

From a user perspective, users only see the underlying token arriving in their wallet, not the wrapper token.

## Token balance airdrops

Merkl allows you to distribute tokens to holders based on their snapshotted balance at a chosen moment in time.

For this type of campaign, in addition to the reward amount, campaign creators must specify:

* Snapshot Date – The balance snapshot is taken at the last block before this date.
* Distribution Date – The date on which rewards should be distributed.
* Snapshot Name – A label for your snapshot.
