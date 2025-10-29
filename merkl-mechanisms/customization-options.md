---
description: Personalize reward distribution to make your campaign unique
---

# Customization Options

Merkl enables campaign creators to **customize their incentive programs with optional features** — from participant eligibility to reward boosts and referral mechanisms — providing greater flexibility beyond standard campaign settings.

The full list of customization options is available in [Merkl Studio](https://studio.merkl.xyz/create-campaign/erc20) under the **Personalize** step when creating a campaign (you can simulate the campaign creation to explore all options — no need to actually launch a campaign).

<figure><img src="../.gitbook/assets/Group 15 (1).png" alt=""><figcaption></figcaption></figure>

Below are some of the most common customization options.

## Access Control

### 🚫 OFAC Compliance

Merkl allows you to **blacklist addresses flagged by the U.S. Office of Foreign Assets Control (OFAC)** to ensure compliance with U.S. economic and trade sanctions imposed on certain countries, organizations, and individuals.

To do this, you need to provide the address of the contract that contains the registry of OFAC-flagged addresses, such as the [Chainalysis Sanctions Oracle on Ethereum](https://etherscan.io/address/0x40c57923924b5c5c5455c48d93317139addac8fb).

### 🌍 Worldchain ID

Merkl enables filtering of campaign participants using Worldchain’s identity system, [World ID](https://docs.world.org/world-id), ensuring that only real, unique humans — not bots — are rewarded.

## Eligibility

### 🔒 Token Holding

Merkl allows campaign creators to add a customization option that **limits reward eligibility to users holding a minimum amount of a specific token in their wallet over a given period** — and this token can differ from the reward token.

Example of a campaign requiring 500 stUSD held for 30 days to earn rewards:

- A user holding 600 stUSD for 44 days is eligible
- A user holding 400 stUSD for 60 days is not eligible

Addresses that fail to meet either the token amount or duration threshold are excluded from the campaign’s rewards, ensuring incentives are focused on long-term participants rather than short-term holders.

### 📸 Token Snapshot

You can also customize campaigns to reward only users who hold a minimum amount of a specific token in their wallet at a given snapshot, instead of over a duration.

## Boosts

### 🚀 Boost

Merkl allows campaign creators to boost rewards for users holding a specific token or NFT.

- Similar to Curve’s vote-escrowed boost formula but with more flexibility.
- No 2.5x limit – You can customize boost multipliers as needed.

#### Boost Formula Computation:

$$
B = b \times \frac{R \times v}{V \times r} + 1
$$

Where:

- **B**: Boost multiplier
- **b**: Custom boost factor chosen by campaign creator
- **R**: Total rewards per epoch
- **r**: User’s reward per epoch
- **V**: Total supply of the boost token/NFT
- **v**: User’s holdings of the boost token/NFT

{% hint style="warning" %}
**Example:** To achieve a boost of 10%, you'll need to indicate a Boost Multiplicator of 1.1
{% endhint %}

#### NFT-Based Boosting

Boosts can be based on NFT holdings. Contact us for help setting up NFT-based reward boosts.

### 📡 API Boost

Merkl provides several methods to manage dynamic boosting through an API. Here are the different methods available:

#### Multiply

Multiply the current amount by the provided input:

$$
\text{amount} = \text{amount} \times \text{boost}
$$

#### Multiply with Offset

Apply a 1 + boost computation:

$$
\text{amount} = \text{amount} \times (1 + \text{boost})
$$

#### Add

Add the boost number to the current amount:

$$
\text{amount} = \text{amount} + \text{boost}
$$

#### Replace

Replace the current amount with the boosted amount:

$$
\text{amount} = \text{boost}
$$

#### Implementation

The customization option has the following parameters:

- **url**: The endpoint to which the API call will be made.
- **boostingFunction**: The function used to calculate the boost. Options include `REPLACE`, `ADD`, `MULTIPLY`, and `MULTIPLY_WITH_OFFSET`.
- **sendScores**: A boolean indicating whether to send ongoing reward computation scores along with the addresses.
- **defaultBoost**: The default boost value to use if no specific boost is provided. Options include `ZERO_ADDRESS` and `ERROR`.

  - `ZERO_ADDRESS`: If an eligible address as per our reward computation is not within your API response, we will use the `ZERO_ADDRESS` boost as a default boost value for this address. Since we always exclude the zero address in our computation, it is indeed safe to use the null address as a default. If you use this option, you must therefore absolutely input a boost value for the zero address.
  - `ERROR`: If there is an address eligible to rewards that we do not find in your API response, the campaign will not proceed.

Depending on whether `sendScores` is true or false, we will POST the following body along with the API call:`sendScores=True`

```jsx
let body: { address: string, score: string }[]
```

`sendScores=False`

```jsx
let body: { addresses: string[] }
```

We will expect the following response:

```jsx
const data : {
  address!: string;
  boost!: bigint;
}[]
```

Any other response will be dropped, and if the object cannot be parsed, the cutomization option will fail.


{% hint style="warning" %}
**Important:** **The boost values are to be given in base 9**. This means that for a boost of 1, the value given for boost should be: "1000000000".
{% endhint %}

#### Dynamic whitelist (example)
If you expect to **add whitelisted addresses over time**, use this method so you don't have to cancel and recreate the campaign with the updated list of whitelisted addresses. To achieve this:
- Use the `MULTIPLY` boosting function 
- Set a boost of "1000000000" (i.e 1× in base 9) for whitelisted addresses, "0" for the `ZERO_ADDRESS` (that way all non-whitelisted addresses do not receive rewards)

```json
{
  [
  {
    "address": "0x1234567890abcdef1234567890abcdef12345678",
    "boost": "1000000000"
  },
  {
    "address": "0xabcdef1234567890abcdef1234567890abcdef1234",
    "boost": "1000000000"
  },
  {
    "address": "0x0000000000000000000000000000000000000000",
    "boost": "0"
  }
]
}
```

#### Dynamic blacklist (example)
Similarly, **you can keep a mutable blacklist without having to cancel and then recreate a campaign**. To achieve this:
- Use the `MULTIPLY` boosting function 
- Set a boost of "0" for blacklisted addresses, and "1000000000" (i.e 1× in base 9) for the `ZERO_ADDRESS` (that way all non-blacklisted addresses receive rewards)

```json
{
  [
  {
    "address": "0x1234567890abcdef1234567890abcdef12345678",
    "boost": "0"
  },
  {
    "address": "0xabcdef1234567890abcdef1234567890abcdef1234",
    "boost": "0"
  },
  {
    "address": "0x0000000000000000000000000000000000000000",
    "boost": "1000000000"
  }
]
}
```
## Advanced Logic

### 🌉 Jumper Bridge

Merkl has partnered with Jumper to enable campaign creators to reward users who bridged liquidity from another chain before participating in a campaign.

This feature ensures that only cross-chain liquidity is incentivized, not movements within the same chain.

### 🤝 Referral Program

The onchain Referral Program allows campaign creators to reward users who refer others to a campaign they’ve participated in, as well as the invitees themselves. This helps acquire new users and accelerate liquidity participation.

The feature offers a wide range of customization options — from user rewards to whitelist gates — all secured by blockchain technology.

#### Key Features:

- **Create Unlimited Referral Programs**: Launch as many referral programs as you want.
- **Referral Code Generation**: Users can generate unique referral codes, share them with their friends, and earn rewards.
- **Whitelabel Integration**: Easily integrate the program into your front-end with whitelabel options (Contact Merkl for details).
- **Cross-Protocol Support**: Referral programs are compatible across the entire Merkl ecosystem, allowing creators to incentivize on any protocol/behaviour integrated with Merkl.
- **Customizable Rewards**: Tailor rewards to users, referrers, invited users, or even non-participating users.
- **Conditions to participate**: Add a whitelist restriction to the program, and optionally charge a fee to create a referral code. Or let anyone participate.
- **Blockchain Security**: Users need to sign a transaction to confirm their referral action, ensuring secure and verified participation.

{% hint style="info" %}
**Contact Merkl for details on how to implement referral programs**
{% endhint %}

### 🎟️ Raffle

Merkl allows you to set up raffles that randomly select lucky winners for your campaigns. You can customize these raffles in various ways to match your campaign needs.

#### Customization

Merkl provides several options for you to tailor your raffles:

- **Multiple raffles**: Choose how often you want raffles to run. Every day? Every week? The choice is yours
- **Number of winners**: Decide how many winners you want to select in each raffle. You can have one grand prize winner, or ay number of lucky winners, depending on your preference.
- **Selection method**: You can choose how winners are selected.
  - **Everyone is equal**: All participants have an equal chance of winning.
  - **Whales first**: Users with higher campaign scores have a better chance of winning (this can help reward top participants).
- **Multiple selection**: You can set up multiple raffles that run at the same time, each with its own rules on how rewards are distributed.

#### Reproducibility

**Seed**\
The seed is the **block hash** of the very next block after the timestamp set by the campaign creator.

**Generating Winning Numbers**\
The random number generation is done using the **XORShift128Plus** pseudo-random number generator (PRNG). This PRNG is seeded with the previously generated seed.\
The **step** is the minimum score of a user divided by 1000. (In the case where the score is not used to pick users, the step is, of course, 1.)

```typescript
function getSelectedNumbers(
  seed: number,
  numberOfWinners: number,
  totalScore: number,
  step: number,
): number[] {
  const random = new XORShift128Plus(seed)
  const selectedNumbers = []
  for (let i = 0; i < numberOfWinners; i++) {
    selectedNumbers.push(random.randBelow(totalScore / step) * step)
  }
  return selectedNumbers
}
```

**Winners**\
The selected numbers correspond to different **"ranges"** in the list of participants (based on their amount or score). The system picks winners by matching these random numbers with ranges of amounts that participants have.\
Winners are picked by sorting all the participants by their address.

In the case of a **snapshot** or **airdrop**, obtaining the list of addresses is straightforward because the participants are known ahead of time. This list typically includes all the addresses that are eligible for the snapshot or airdrop, and they are usually collected from a specific event or condition.

For **more complex campaigns**, where winners are determined through specific criteria or weighted selections, one approach is to run a campaign with the **exact same parameters** but **without the raffle option**.

The key idea is:

- You run this simplified version of the campaign **with a smaller amount** (i.e., a lower prize or allocation) and **without the raffle option**.
- This will **generate the list of users** that Merkl identifies as eligible or **potential winners**.
- You can use this list to generate the list of users.

In essence, the result of this campaign (without the raffle option) gives you the list of users who met the conditions set by the campaign. These users are then the ones Merkl considers for potential winning when the raffle customization option is applied.

#### Example with 5 users, 5 weights, and one number picked

We have **5 users** in the raffle, each with an associated **weight** (which represents their chance of winning). The system will pick **one winner** based on a randomly selected number.

Let’s assume the users and their weights are as follows:

| User | Weight (Amount) |
| ---- | --------------- |
| A    | 10              |
| B    | 20              |
| C    | 30              |
| D    | 40              |
| E    | 50              |

We want to pick one winner randomly based on these weights.

**1/ Total Weight Calculation:**

First, we calculate the **total weight** by summing the weights of all the users. This total represents the "pool" from which the random number will be drawn.

Total Weight = 10 + 20 + 30 + 40 + 50 = 150

**2/ Random Number Generation**:

The system will then pick a random number. This random number will be between **0 and the total weight (150)**.

Let’s assume the randomly picked number is **107**.

**3/ Weighted Selection**

The system uses this random number to determine which user will win. To do this, it calculates cumulative weights, which define "ranges" for each user.

- For **User A**: The range is from **0 to 10** (because A has a weight of 10).
- For **User B**: The range is from **10 to 30** (since A’s range is 0–10, and B has a weight of 20).
- For **User C**: The range is from **30 to 60**.
- For **User D**: The range is from **60 to 100**.
- For **User E**: The range is from **100 to 150**.

So, the cumulative ranges are as follows:

| User | Cumulative Weight Range | Range |
| ---- | ----------------------- | ----- |
| A    | 0–10                    | 10    |
| B    | 10–30                   | 20    |
| C    | 30–60                   | 30    |
| D    | 60–100                  | 40    |
| E    | 100–150                 | 50    |

**4/ Determine the Winner:**

Now, the system checks where the random number falls in the cumulative weight ranges:

- **Random number**: 107\
  The number **107** falls in **User E's** range (100–150), so **User E** is the winner.
