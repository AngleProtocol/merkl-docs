# Fee Model

Merkl's fees are deducted directly from the incentives distributed by campaign creators.

## Pay-as-You-Go

**Standard campaign fee**: 3% of distributed incentives

**Airdrop discount**: JSON-based airdrops are charged at a reduced rate of 0.5%

**Degressive fee structure**: The more you distribute (excluding airdrops), the lower your effective fee rate. This makes Merkl increasingly cost-efficient at scale. Fees are calculated based on the total amount distributed across all campaigns and addresses associated with your entity:

- **3%** on the first $1M distributed
- **2.25%** on amounts between $1M and $2.5M
- **1.75%** on amounts between $2.5M and $5M
- **1.5%** on amounts above $5M

{% hint style="info" %}
**Example**: If you distribute $3M in total, you'll pay 3% on the first $1M ($30k), 2.25% on the next $1.5M ($33.75k), and 1.75% on the final $500k ($8.75k), for a total fee of $72.5k (2.42% effective rate).
{% endhint %}

## Commitment Model (For High-Volume Campaigns)

For projects planning to distribute significant incentives, Merkl offers a 25% fee discount when you commit upfront to campaigns distributing more than $500k in total incentives.

**How it works**: Pay your fees in advance for campaigns totaling at least $500k in distributions, and receive an automatic 25% reduction on those fees.

{% hint style="success" %}
Contact the Merkl team to discuss commitment model options and unlock volume discounts.
{% endhint %}

## Unclaimed Rewards Policy

Campaign creators on Merkl can [freely reallocate](../merkl-mechanisms/features.md) all unclaimed rewards once their campaign ends.

One year after the campaign's end, if the campaign creators have not previously reallocated rewards and if there remain unclaimed rewards, Merkl reserves the right to reclaim them.