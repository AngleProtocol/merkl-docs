---
description: Helpers to use and integrate Merkl
---

# 🧑‍💻 Smart Contracts Addresses

Merkl relies on two contracts deployed on each chain it supports:

- `Distributor`: where liquidity providers claim their rewards and stakeholders dispute the proposed merkle roots
- `DistributionCreator`: where incentivizors create their campaigns

Both contracts are managed through a `CoreMerkl` contract managed by a multisig which has the power to settle disputes, change dispute parameters, to modify fees and their recipients, and to whitelist new addresses allowed to modify Merkle roots in the `Distributor` contract. It has no ability to alter distributions.

| Chain    | Contracts                                                                                                                                                                                                                                                                                                                                                                                                                |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Ethereum | [Distributor](https://etherscan.io/address/0x3Ef3D8bA38EBe18DB133cEc108f4D14CE00Dd9Ae), [DistributionCreator](https://etherscan.io/address/0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd), [CoreMerkl](https://etherscan.io/address/0x0E632a15EbCBa463151B5367B4fCF91313e389a6), [Management Multisig](https://etherscan.io/address/0x529619a10129396a2F642cae32099C1eA7FA2834)                                             |
| Polygon  | [Distributor](https://polygonscan.com/address/0x3Ef3D8bA38EBe18DB133cEc108f4D14CE00Dd9Ae), [DistributionCreator](https://polygonscan.com/address/0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd), [CoreMerkl](https://polygonscan.com/address/0x9418d0aa02fce40804abf77bb81a1ccbeb91eafc), [Management Multisig](https://polygonscan.com/address/0xc0c07644631543c3af2fA7230D387C5fA418a131)                                 |
| Optimism | [Distributor](https://optimistic.etherscan.io/address/0x3Ef3D8bA38EBe18DB133cEc108f4D14CE00Dd9Ae), [DistributionCreator](https://optimistic.etherscan.io/address/0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd), [CoreMerkl](https://optimistic.etherscan.io/address/0xc2c7a0d9a9e0467090281c3a4f28D40504d08FB4), [Management Multisig](https://optimistic.etherscan.io/address/0x17a7F6a839fea3b716b43f9414ffc93131878BD2) |
| Arbitrum | [Distributor](https://arbiscan.io/address/0x3Ef3D8bA38EBe18DB133cEc108f4D14CE00Dd9Ae), [DistributionCreator](https://arbiscan.io/address/0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd), [CoreMerkl](https://arbiscan.io/address/0xA86CC1ae2D94C6ED2aB3bF68fB128c2825673267), [Management Multisig](https://arbiscan.io/address/0x3350bef226F7BdCA874C5561320aB7EF9DC89E70)                         |
| Base | [Distributor](https://basescan.org/address/0x3Ef3D8bA38EBe18DB133cEc108f4D14CE00Dd9Ae), [DistributionCreator](https://basescan.org/address/0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd), [CoreMerkl](https://basescan.org/address/0xC16B81Af351BA9e64C1a069E3Ab18c244A1E3049), [Management Multisig](https://basescan.org/address/0x19c41F6607b2C0e80E84BaadaF886b17565F278e)                         |
| Gnosis | [Distributor](https://gnosisscan.io/address/0x3Ef3D8bA38EBe18DB133cEc108f4D14CE00Dd9Ae), [DistributionCreator](https://gnosisscan.io/address/0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd), [CoreMerkl](https://gnosisscan.io/address/0xFD0DFC837Fe7ED19B23df589b6F6Da5a775F99E0), [Management Multisig](https://gnosisscan.io/address/0xf4b426A9849933c83041D6554cb39955ddA5c767)                         |
| Polygon ZKEVM | [Distributor](https://zkevm.polygonscan.com/address/0x3Ef3D8bA38EBe18DB133cEc108f4D14CE00Dd9Ae), [DistributionCreator](https://zkevm.polygonscan.com/address/0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd), [CoreMerkl](https://zkevm.polygonscan.com/address/0xC16B81Af351BA9e64C1a069E3Ab18c244A1E3049), [Management Multisig](https://zkevm.polygonscan.com/address/0x9439B96E39dA5AD7EAA75d7a136383D1D9737055)                         |

{% hint style="info" %}
Addresses of the `Distributor` and `DistributionCreator` contracts are the same across all chains.
{% endhint %}
