---
description: Easily interact with the Merkl API using the dedicated NPM package
---

# 📦 Merkl API V4 NPM Package

Merkl API V4 comes with a dedicated NPM package to help you interact with it.

This quickstart guide will help you get started with interacting with the Eden Treaty app object using the Merkl API package.

![](https://github.com/user-attachments/assets/775a2f92-8d7f-4f87-a72a-0a7b6e278edf)

## Installation 🚀

To begin, install the `@merkl/api` package into your project:

```bash
npm install @merkl/api
```

## Usage Example 🛠️

Here's a step-by-step guide to instantiating the Merkl API object and making API calls.

### Import the Merkl API Package 📦

```javascript
import { MerklApi } from '@merkl/api'
```

### Instantiate the Merkl API Object 🌐

Initialize the Merkl API object by providing the base URL. Developers can leverage autocomplete features in their IDEs since all methods are strongly typed, ensuring a seamless development experience:

```javascript
const merkl = MerklApi('https://api.merkl.xyz').v4
```

### Making API Calls 📡

Below are examples of common API calls you can make. When making API calls, you can pass `query` parameters as an object under the `query` property and `path` parameters directly in the method arguments, as shown in the examples below:

#### Get Opportunities 💼

Retrieve a list of opportunities filtered by a specific `chainId`:

```javascript
const opportunities = await merkl.opportunities.index.get({
  query: { chainId: '1' },
})

console.log(opportunities.data)
```

#### Get Rewards for a Specific Address 🏆

Retrieve rewards for a specific user address. Note that all methods are strongly typed for better developer experience: this ensures reduced debugging time, as incorrect parameter usage is flagged early, and improved code completion in supported IDEs for faster development.

```javascript
const rewards = await merkl
  .users({
    address: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045',
  })
  .rewards.get({ query: { chainId: 1 } })

console.log(rewards.data)
```

#### Get Campaigns Rewarding USDC

Retrieve campaigns filtered by token symbol, such as `USDC`:

```javascript
const campaignsUSDC = await merkl.campaigns.index.get({
  query: { tokenSymbol: 'USDC' },
})

console.log(campaignsUSDC.data)
```

## Notes 📝

- All API responses are wrapped within a `data` attribute. This consistency across all API calls ensures that developers can reliably access the actual response payload. Access this attribute to get the actual response payload.
- The Merkl API object methods are strongly typed, making it easier to catch errors during development.

For additional documentation and advanced usage, refer to the [Merkl API swagger](https://api.merkl.xyz/v4/docs).

---

You're now ready to interact with the Merkl API! 🎉
