# Custom Lending and Borrowing Campaigns

Merkl natively supports all lending and borrowing protocols that issue receipt tokens for lenders and debt tokens for borrowers. However, it also extends support to:

* Lending protocols that don’t follow this standard structure.
* Tailored incentive campaigns—for example, rewarding users based on their net lending position (lending minus borrowing).

Some notable custom integrations include Silo, Radiant, Morpho, Ajna, Euler, and Compound V2 (along with its forks).

From a user perspective, interacting with these custom lending protocols feels the same as with standard ones. Merkl’s engine abstracts away protocol-specific differences, ensuring a consistent UX and feature set.

This means that campaign creators can also customize:

* their [campaign hooks](../hooks.md) (e.g. blacklisting, whitelisting systems, forwarding mechanisms, ...)
* their [distribution methods](../distributions.md) (e.g fixed APR, variable APR, ...)

{% hint style="info" %}
If you want your lending and borrowing protocol to be fully integrated and supported by Merkl, or to create tailored protocol-campaigns, please [contact us on the Merkl Discord by opening a BD ticket](https://discord.com/invite/jnYfrGxDbe) to discuss the integration process.
{% endhint %}

Here are below some specificities you may face with some lending protocol integrations on Merkl.

## Morpho

When creating a campaign for a Morpho pool, you'll be asked to input the following parameters:

* **Usage:** From the dropdown menu, select the usage type (_MetaMorpho_, _Supply_, _Borrow_, _Collateral_) you want to incentivize. Choosing _MetaMorpho_ will incentivize a MetaMorpho vault, while selecting _Supply_, _Borrow_, or _Collateral_ will incentivze a Morpho Blue market.
* **Market ID:** Specify the Market ID. For _MetaMorpho_, select the desired MetaMorpho vault to incentivize. For Morpho Blue, choose whether to incentivize the _supply_, _borrow_, or _collateral_ for a specific Morpho Blue market. You also have the option to manually enter the asset address for any type of Market ID.

## Radiant

You'll be asked to set a `cap` parameter in USDC. This is used by Radiant: if you're not the Radiant protocol you should set it to 0.

## Silo

Like on Morpho, there are some custom parameters to choose:

* **Usage:** Select the specific usage for your Silo campaign (Deposit, Protected Deposit, or Debt).
* **Version:** Choose the version of Silo (Silo Legacy, or Silo Llama).
* **Asset:** Select the asset to incentivize from the dropdown menu or enter the asset address manually.

The blacklist/whitelist features also come handy here as you may use it to automatically blacklist silos that should not receive rewards.
