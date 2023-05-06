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
