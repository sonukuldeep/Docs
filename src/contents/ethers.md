---
title: Ethers
author: Internet
datetime: 2023-05-02
slug: "ethers"
featured: false
draft: false
tags:
  - ethers
  - react
  - nodejs
ogImage: ""
description: Axios is a promise-based HTTP Client for node.js and the browser.
---

# Ethers v5.7

The ethers.js library aims to be a complete and compact library for interacting with the Ethereum Blockchain and its ecosystem. It was originally designed for use with ethers.io and has since expanded into a more general-purpose library.

![main image](https://i.morioh.com/2022/03/19/4ee0fd50.webp)

## Installation

```js
npm i ethers
```

## Imports

```js
const { ethers } = require("ethers"); //node js
import { ethers } from "ethers"; //react
```

<table class="table minimal">
<tbody>
<tr>
<td><b>Provider</b></td>
<td>A Provider (in ethers) is a class which provides an abstraction for a connection to the Ethereum Network. It provides read-only access to the Blockchain and its status.</td>
</tr>
<tr>
<td><b>Signer</b></td>
<td>A Signer is a class which (usually) in some way directly or indirectly has access to a private key, which can sign messages and transactions to authorize the network to charge your account ether to perform operations.</td>
</tr>
<tr><td><b>Contract</b></td>
<td>A Contract is an abstraction which represents a connection to a specific contract on the Ethereum Network, so that applications can use it like a normal JavaScript object.</td>
</tr>
</tbody></table>

## Providers

- JsonRpcProvider [Doc](https://docs.ethers.org/v5/api/providers/jsonrpc-provider/#JsonRpcProvider)

* Other Providers
  - Web3Provider [Doc](https://docs.ethers.org/v5/api/providers/other/#Web3Provider)
  - WebSocketProvider [Doc](https://docs.ethers.org/v5/api/providers/other/#WebSocketProvider)
  - FallbackProvider [Doc](https://docs.ethers.org/v5/api/providers/other/#FallbackProvider)
  - IpcProvider [Doc](https://docs.ethers.org/v5/api/providers/other/#IpcProvider)
  - JsonRpcBatchProvider [Doc](https://docs.ethers.org/v5/api/providers/other/#JsonRpcBatchProvider)
  - UrlJsonRpcProvider [Doc](https://docs.ethers.org/v5/api/providers/other/#UrlJsonRpcProvider)
* API providers [Doc](https://docs.ethers.org/v5/api/providers/api-providers/#api-providers)
  - EtherscanProvider
  - InfuraProvider
  - AlchemyProvider
  - CloudflareProvider
  - PocketProvider
  - AnkrProvider

## Default Provider [Doc](https://docs.ethers.org/v5/api/providers/#providers-getDefaultProvider)

[more](https://docs.ethers.org/v5/api-keys/#api-keys--getDefaultProvider)

It is recommended you use a Default Provider.

The default provider is the safest, easiest way to begin developing on Ethereum, and it is also robust enough for use in production.

It creates a FallbackProvider connected to as many backend services as possible. When a request is made, it is sent to multiple backends simultaneously. As responses from each backend are returned, they are checked that they agree. Once a quorum has been reached (i.e. enough of the backends agree), the response is provided to your application.

This ensures that if a backend has become out-of-sync, or if it has been compromised that its responses are dropped in favor of responses that match the majority.

```js
// Use the mainnet
const network = "homestead";

// Specify your own API keys
// Each is optional, and if you omit it the default
// API key for that service will be used.
const provider = ethers.getDefaultProvider(network, {
    etherscan: YOUR_ETHERSCAN_API_KEY,
    infura: YOUR_INFURA_PROJECT_ID,
    // Or if using a project secret:
    // infura: {
    //   projectId: YOUR_INFURA_PROJECT_ID,
    //   projectSecret: YOUR_INFURA_PROJECT_SECRET,
    // },
    alchemy: YOUR_ALCHEMY_API_KEY,
    pocket: YOUR_POCKET_APPLICATION_KEY
    // Or if using an application secret key:
    // pocket: {
    //   applicationId: ,
    //   applicationSecretKey:
    // },
    ankr: YOUR_ANKR_API_KEY
});
```

## Signers

A Signer in ethers is an abstraction of an Ethereum Account, which can be used to sign messages and transactions and send signed transactions to the Ethereum Network to execute state changing operations.

The available operations depend largely on the sub-class used.

For example, a Signer from MetaMask can send transactions and sign messages but cannot sign a transaction (without broadcasting it).

The most common Signers you will encounter are:

- Wallet, which is a class which knows its private key and can execute any operations with it
- JsonRpcSigner, which is connected to a JsonRpcProvider (or sub-class) and is acquired using getSigner

## Contract Interaction

A Contract object is an abstraction of a contract (EVM bytecode) deployed on the Ethereum network. It allows for a simple way to serialize calls and transactions to an on-chain contract and deserialize their results and emitted logs.

## Getting Provider, signer & contractInstance

React App

```js
// A Web3Provider wraps a standard Web3 provider, which is
// what MetaMask injects as window.ethereum into each page
const provider = new ethers.providers.Web3Provider(window.ethereum);
// MetaMask requires requesting permission to connect users accounts
await provider.send("eth_requestAccounts", []);
// The MetaMask plugin also allows signing transactions to
// send ether and pay to change state within the blockchain.
// For this, you need the account signer...
const signer = provider.getSigner();

// contract instance
const contractInstance = new ethers.Contract(ADDRESS, ABI, signer);

// solidity function call
await contractInstance.functionName();
```

Nodejs

```js
const provider = new ethers.providers.JsonRpcProvider(
  `https://sepolia.infura.io/v3/${process.env.WEB3APIKEY}`
);
const contractInstance = new ethers.Contract(ADDRESS, ABI, provider);
const name = await contractInstance.functionName();
```
