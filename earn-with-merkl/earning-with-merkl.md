---
description: Everything you need to know to earn rewards with Merkl
---

# Earn with Merkl

{% hint style="warning" %}
**Before your start**

Make sure you understand:

* The difference between an [opportunity and a campaign](../merkl-mechanisms/technical-overview.md#opportunity-vs.-campaign)
* Key terms: [daily rewards, APR, and TVL](../merkl-mechanisms/technical-overview.md#main-metrics)
* How the [Merkl App](../merkl-mechanisms/technical-overview.md#merkl-app) is structured
{% endhint %}

All yield opportunities from protocols and chains distributing rewards through Merkl are available on the [Merkl App](https://app.merkl.xyz/).

<figure><img src="../.gitbook/assets/Capture d’écran 2025-09-17 à 10.51.55 2.png" alt=""><figcaption></figcaption></figure>



## How to get started

#### 1. Explore opportunities

Visit the [Merkl App](https://app.merkl.xyz/) and browse the available opportunities to find the one that suits you best. You can filter by chain, protocol, category, status, and more for an easy overview.

<figure><img src="../.gitbook/assets/Capture d’écran 2025-11-18 à 17.55.17 1.png" alt=""><figcaption></figcaption></figure>

#### 2. Check campaign details

Select an opportunity and review campaign details such as APR, TVL, daily rewards, end date, and distribution strategy.

<figure><img src="../.gitbook/assets/Capture d’écran 2025-11-18 à 17.56.35 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Make sure to check the **eligibility rules** (e.g., blacklist, health factor) to confirm you qualify for rewards.
{% endhint %}

{% hint style="info" %}
Do not get lured by high APRs. Rewards may be limited to whitelisted addresses or mostly go to large positions.
{% endhint %}

#### 3. Interact with the protocol

Use the CTA button on the opportunity page to interact directly with the incentivized protocol (e.g., supply, lend, borrow).

{% hint style="danger" %}
Always do your own research (DYOR) before investing and ensure you trust the protocol you’re interacting with.

**Merkl is non-custodial**. No funds are held in Merkl smart contracts. You interact directly with the protocols, while Merkl tracks your onchain activity and distributes rewards accordingly.

The main risks when using Merkl come from the underlying protocols, which may have smart contract vulnerabilities or operational issues that could put your funds at risk.

Merkl is not responsible for any issues arising from incentivized protocols running campaigns on the platform.
{% endhint %}

#### 4. Start earning rewards

Once you’ve interacted with the protocol, you’ll start earning rewards automatically. No staking or additional onchain actions are required.

#### 5. Claim your rewards

Visit your [Merkl dashboard](https://app.merkl.xyz/users/) to track and claim your rewards. Ensure you are connected with the same wallet used to interact with the protocol. Make sure to claim all your rewards within a year of receiving them!

<figure><img src="../.gitbook/assets/Capture d’écran 2025-11-12 à 15.14.56 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Rewards usually show up in your dashboard around 8 hours after you interact with the protocol, giving Merkl time to calculate and distribute them. Check the [Status page](https://app.merkl.xyz/status) for details on reward distribution per chain.
{% endhint %}

{% hint style="info" %}
You can claim all your rewards per chain at once to optimize gas costs!
{% endhint %}

## Advanced features

Rewards on Merkl do not increase block by block, but can be claimed at a frequency which depends on the chain. You can check the claim frequency on the [Status page](https://app.merkl.xyz/status).

### Operator System: Delegating Claim Rights

By default, rewards can only be claimed by the address that earned them. However, Merkl provides a flexible operator system that allows you to delegate the right to claim rewards to another address, while still receiving the rewards yourself.

**How regular operators work:**

You can approve an operator to claim on your behalf by calling the function `toggleOperator` on the [distributor smart contract](https://app.merkl.xyz/status). When an operator claims on your behalf, the rewards are still sent to your original address—the operator only facilitates the transaction.

**Example:**

Assuming Alice earned the rewards:

- **Default behavior**: Only Alice can claim, and rewards are sent to Alice.
- **With operator**: By calling `toggleOperator`, Alice can allow Bob to claim on her behalf. Bob can then claim for Alice by submitting Alice's proof to the contract, and rewards are still sent to Alice's address.

If you can't call `toggleOperator` and are stuck, please [open a tech ticket in our Discord ](https://discord.com/channels/1209830388726243369/1210212731047776357), the team may be able to call it on your behalf.

### Claiming from a multisig

When claiming rewards from a multisig address, we recommend delegating the claim process to an operator. This is because Merkl reward proofs are only valid for on average four hours (varies depending on the chain). If you fail to gather the necessary signatures and execute the claim transaction before a Merkle root is updated, your claim transaction will fail.

### Claiming as an operator

Currently, when you're connected to the Merkl frontend with an operator address, you only see rewards earned by that operator address—not by the addresses that have appointed it.

To claim rewards on behalf of another address using an operator, you'll need to bypass the frontend and use the Merkl API directly. For example, you can retrieve the Merkle proof for a specific user and chain by calling: `https://api.merkl.xyz/v4/users/0xA9DdD91249DFdd450E81E1c56Ab60E1A62651701/rewards?chainId=1`

This response will include the Merkle proof data needed to submit a claim. You can then use this information to call the claim function on the [Merkl Distributor contract](../integrate-merkl/smart-contract-addresses.md) via a block explorer or directly onchain.

To structure the claim:

* `users`: Provide the address(es) you're claiming on behalf of. If claiming for multiple tokens, repeat the user address for each token.
* `tokens`: List the token addresses you're claiming.
* `amounts`: Use the amount field from the Merkl API response for each token.
* `proofs`: Include the Merkle proof array for each token, also retrieved from the API.

<figure><img src=".gitbook/assets/DistributorClaim.png" alt=""><figcaption><p>Claiming Merkl rewards using a block explorer</p></figcaption></figure>

## Claim Recipient System

The claim recipient system allows you to redirect where claimed rewards are sent, instead of always sending to the user who earned them.

### Function: `setClaimRecipient()`

**Signature:**
```solidity
function setClaimRecipient(address recipient, address token) external
```

**Parameters:**

- `recipient`: Address that will receive the claimed tokens. Use `address(0)` to remove the custom recipient (revert to default)
- `token`: Token for which to set the recipient. Use `address(0)` to set a **global recipient** that applies to all tokens without a specific recipient

**Access:** Callable by any user (sets recipient for `msg.sender`)

**How it works:**

- Sets a custom recipient for your own claims
- Setting `recipient = address(0)` removes the custom recipient for that token
- When `token = address(0)`, it sets `claimRecipient[user][address(0)]` as a global fallback

**Priority order when claiming:**

When claiming rewards, the system follows this priority order:

1. **Token-specific recipient**: `claimRecipient[user][token]` (if set and not `address(0)`)
2. **Global recipient**: `claimRecipient[user][address(0)]` (if set)
3. **Default**: User's own address

Token-specific recipients take precedence over the global recipient.

**Examples:**

```solidity
// Define a global recipient for all tokens
setClaimRecipient(vaultAddress, address(0));
// ✅ claimRecipient[user][address(0)] = vaultAddress
// ✅ All tokens without a specific recipient will go to vaultAddress

// Define a specific recipient for aglaMerkl (takes priority over global)
setClaimRecipient(aglaMerklVaultAddress, aglaMerkl);
// ✅ claimRecipient[user][aglaMerkl] = aglaMerklVaultAddress
// ✅ aglaMerkl goes to aglaMerklVaultAddress (not the global vault)

// Remove the specific recipient for aglaMerkl
setClaimRecipient(address(0), aglaMerkl);
// ✅ claimRecipient[user][aglaMerkl] = address(0)
// ✅ aglaMerkl will now use the global recipient (vaultAddress)
```

**Example flow:**

```solidity
// Setup
setClaimRecipient(vaultAglaMerkl, aglaMerkl);           // Specific for aglaMerkl
setClaimRecipient(defaultVault, address(0));  // Global fallback

// When claiming:
// - aglaMerkl → goes to vaultAglaMerkl (token-specific)
// - WETH → goes to defaultVault (global fallback)
// - DAI → goes to defaultVault (global fallback)
```

**Use cases:**

- Routing rewards to smart contracts that cannot directly claim
- Redirecting rewards to user-controlled addresses
- Protocol-level reward forwarding mechanisms

---

## Claim Callback System (Automatic Actions)

The `claimWithRecipient()` function supports passing arbitrary data (`bytes`) to recipient contracts, enabling automatic actions like swaps via aggregators when rewards are claimed.

### How it works

When you call `claimWithRecipient()` with non-empty `datas`, the system:

1. Transfers tokens to the recipient address
2. Automatically calls `onClaim()` on the recipient contract (if it implements `IClaimRecipient`)
3. Passes the custom data to the callback, allowing the recipient to perform actions like swaps, deposits, or conversions

### Function: `claimWithRecipient()`

**Signature:**
```solidity
function claimWithRecipient(
    address[] calldata users,
    address[] calldata tokens,
    uint256[] calldata amounts,
    bytes32[][] calldata proofs,
    address[] calldata recipients,
    bytes[] memory datas  // Custom data for callback
) external
```

**Parameters:**

- `datas`: Array of arbitrary `bytes` data, one per claim. This data is passed to the recipient's `onClaim()` callback.

### IClaimRecipient Interface

Recipient contracts must implement this interface:

```solidity
interface IClaimRecipient {
    function onClaim(address user, address token, uint256 amount, bytes memory data) 
        external returns (bytes32);
}
```

The callback must return `CALLBACK_SUCCESS` (keccak256("IClaimRecipient.onClaim")) or the claim will revert.

**Important notes:**

- If `data.length == 0`, no callback is made (standard transfer only)
- If the callback reverts, the token transfer still occurred (uses `try/catch`)
- The `datas` array must have the same length as other arrays
- Data encoding is flexible - you can encode any parameters your recipient contract needs

### Use cases

- **Automatic swaps**: Swap rewards immediately via DEX aggregators (1inch, 0x, etc.)
- **Auto-deposits**: Deposit rewards into liquidity pools or yield farms
- **Token conversions**: Convert rewards to a different token automatically
- **Complex routing**: Perform multiple actions based on encoded data

### Example: Automatic Swap

```solidity
// 1. Set a swap vault as recipient
setClaimRecipient(swapVaultAddress, aglaMerkl);

// 2. Claim with swap parameters encoded in data
bytes memory swapData = abi.encode(
    targetToken,      // Token to receive after swap
    minAmountOut,     // Minimum amount expected
    swapRouter        // Aggregator router address
);

claimWithRecipient(
    [userAddress],
    [aglaMerkl],
    [amount],
    [proof],
    [swapVaultAddress],
    [swapData]  // Data for automatic swap
);

// The swap vault receives aglaMerkl, automatically swaps it, and sends the result to the user
```

---

## Support for `address(0)` as Wildcard

The `Distributor` contract uses `address(0)` as a wildcard parameter in several functions, representing **"all tokens"** or **"default"**.

### Where `address(0)` is used:

1. **`setClaimRecipient(recipient, address(0))`**
   - Sets a **global recipient** that applies to all tokens without specific recipients

2. **`setClaimRecipientWithGov(user, recipient, address(0))`**
   - Sets a **global recipient** for a user across all tokens

### How it works:

- When `address(0)` is used as the token parameter, it acts as a fallback/default
- This simplifies operations that need to work across multiple tokens
- Token-specific recipients take precedence over the global recipient (see priority order above)

### Address Remapping

If a smart contract you use can’t claim rewards, ask the Merkl team to remap those rewards to a claimable wallet by reaching out on [Discord](https://discord.com/channels/1209830388726243369/1210212731047776357) with the campaign ID, source address, and destination address. The full walkthrough lives in the [Reward Forwarding guide](../merkl-mechanisms/reward-forwarding.md#address-remapping).
