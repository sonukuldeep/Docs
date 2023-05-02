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

# Ethers

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
