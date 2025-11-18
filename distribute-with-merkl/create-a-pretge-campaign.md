---
description: Everything you need to know to create a campaign distributing a pre-TGE token with Merkl
---

# Create a Campaign pre-TGE

Merkl enables you to distribute tokens before they become transferable at TGE. This approach combines the best of both worlds: users can claim actual tokens (not just points), while you maintain control over transferability until your TGE.

**How this differs from standard campaigns**: The distributed token is initially non-transferable but becomes fully transferable at TGE. Users can claim their tokens through the Merkl UI and see them in their wallets—they just can't transfer them yet.

**How this differs from points campaigns**: There's no need for a separate airdrop later. Tokens earned by users are real tokens that automatically convert 1:1 to transferable tokens at TGE. Users already hold the tokens; they simply gain transferability at launch.

## How it works

1. **Whitelist Merkl contracts**: Grant permission to Merkl's `Creator` and `Distributor` contracts so they can distribute your non-transferable tokens.
2. **Create your campaigns**: Use Merkl's standard campaign creation flow to define reward rates, multipliers, and which onchain actions to track. Campaigns can be [created programmatically](./create-a-campaign.md) with simple scripts.
3. **Users claim tokens**: Participants interact onchain as usual and claim their non-transferable tokens directly through their Merkl dashboard. These tokens appear in their wallets immediately.

## Benefits

With this setup, you benefit from **all of Merkl's built-in features**:

- **Public visibility**: Your campaigns appear in [the Merkl app](https://app.merkl.xyz/?tokenType=PRETGE&sort=tvl-desc) under the Pre-TGE section
- **Leaderboards**: Users can track their rankings and performance
- **APR displays**: Merkl can assign an estimated value to your tokens directly in the UI, showing users estimated APRs when farming. This estimate is typically based on your latest fundraising valuation or the agreed price with a CEX for an upcoming listing.
- **Full customization**: Access to all of Merkl's [customization options](../merkl-mechanisms/customization-options.md), including boosts, referral programs, and eligibility rules

<figure><img src="../.gitbook/assets/Group 26.png" alt=""><figcaption><p>Pre-TGE token campaigns appear in the Merkl app under the Pre-TGE section</p></figcaption></figure>

{% hint style="success" %}
This is the most straightforward way to leverage Merkl's full feature set while maintaining control over token transferability until your TGE. No separate airdrop needed—users already hold the tokens.
{% endhint %}

## Pricing

The pricing for pre-TGE campaigns is the [same as campaigns distributing transferable tokens with Merkl](fee-model.md).