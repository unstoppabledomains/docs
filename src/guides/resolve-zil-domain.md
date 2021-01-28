---
description: Simplest step-by-step guide on how to resolve .zil domain
---

# How to integrate .zil resolution from scratch

![Demo](../.gitbook/assets/demozil.gif)

## Table of contents

* [Initialize the project folder](resolve-zil-domain.md#initialize-the-project-folder)
* [Get a namehash of a domain](resolve-zil-domain.md#taking-a-namehash)
* [Get resolver contract address from a domain](resolve-zil-domain.md#getting-resolver-address)
* [Query resolver contract to fetch the records](resolve-zil-domain.md#fetching-the-domain-records)
* [Displaying results](resolve-zil-domain.md#displaying-result)
* [Error handling](resolve-zil-domain.md#error-handling)

## Initialize the project folder

#### Install dependencies

First, install the project's two dependencies:

1. [parcel.js](https://parceljs.org/) — A simple bundler for fast development
2. [hash.js](https://www.npmjs.com/package/hash.js/v/1.1.7) — For taking sha256 hashes of a domain name

```shell
npm install -g add parcel-bundler

mkdir unstoppable-zil-resolution
cd unstoppable-zil-resolution

npm init
npm install hash.js

touch index.ts index.html
```

Your folder structure should look like this.

```
.
├── index.html
├── index.ts
├── node_modules
├── package.json

```

#### Create `package.json`

Next, open `package.json` to configure your build scripts.

As mentioned above, this project uses `parcel.js`. One of the features of which is a built in development server. We can activate this server by calling `parcel <entry file>`

Parcel also requires us to add a `browserlist` due to its [babel technology](https://flaviocopes.com/parcel-regeneratorruntime-not-defined/).

The resulting `package.json` file should look like this.

```javascript
// package.json
{
  "name": "unstoppable-resolution",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "hash.js": "^1.1.7"
  },
  "scripts": {
    "dev": "parcel index.html",
    "build": "parcel build index.html"
  },
  "browserslist": [
    "since 2017-06"
  ]
}
```

#### Create `index.html`

As our next step, let's open our HTML page and add some boilerplate code.

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="utf-8">
  <title>Basic .zil integration</title>
</head>

<body>
  <div id="main" style="
        display: flex;
        flex-direction: column;
        height: 100vh;
      ">
    <input id="input" />
    <button id="button">Resolve</button>

    <div id="records" style="display: flex; flex-direction: column;">

    </div>
  </div>
  <script src="./index.ts"></script>
</body>

</html>
```

<sup>This is a simple html document with a single div element in the body. It contains input field for our user, a button to trigger the resolution, and another div where we'll display the results.</sup>

### Add javascript to handle our button press and resolution

In this section, we'll create a new file, `index.js`, import our hash library for future encoding, and define two constants. 

| constant | description |
| :--- | :--- |
| **ZILLIQA_API** | an official Zilliqa endpoint which serves us an entry point to the Zilliqa blockchain |
| **UD_REGISTRY_CONTRACT_ADDRESS** | Registry address of UD without _0x_ prefix |

We'll discuss the registry contract address later in this guide.

#### Create `index.js`

```javascript
// index.js
import hash from 'hash.js';

const ZILLIQA_API = "https://api.zilliqa.com/";
const UD_REGISTRY_CONTRACT_ADDRESS = "9611c53BE6d1b32058b2747bdeCECed7e1216793";
```

Next, we need to define and attach a function `resolve` to our HTML button under `id="button"`.

We start writing this function by simply taking our input from the text field and preparing to handle an incorrect domain. We will revisit [error handling](resolve-zil-domain.md#error-handling) later in this guide.

{% hint style="warning" %}
Any domain that does not end with `.zil` is out of scope for this guide.
{% endhint }

#### Add .zil resolution

```javascript
// index.js
import hash from 'hash.js';

const ZILLIQA_API = "https://api.zilliqa.com/";
const UD_REGISTRY_CONTRACT_ADDRESS = "9611c53BE6d1b32058b2747bdeCECed7e1216793";

async function resolve() {
  const userInput = (document.getElementById("input")).value;
  if (!userInput.endsWith(".zil") {
    // placeholder for future error handling
    return;
  }
}

document.getElementById("button").addEventListener('click', () => resolve());
```

## Taking a namehash

Namehashing is an algorithm that tokenizes your domain name in a way that a Zilliqa smart contract can understand.

{% hint style="warning" %}
It is essential to know the difference between Zilliqa namehashing and [EIP-137](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-137.md#namehash-algorithm). Zilliqa uses **sha256 from SHA-2**, instead of **keccak256** which is more commonly used in Ethereum.
{% endhint %}

To tokenize our domain we need to split it by the "." character into separate labels, reverse the array, and reduce it to a single hash using a childhash function. Childhash generates a hash of the current label, concatenates it with the parent, and hashes the result as a hex value. For our first parent, we'll take a string of **64 zeros**.

#### Adding a namehash

```javascript
// index.js
function namehash(domain) {
  const parent =
    '0000000000000000000000000000000000000000000000000000000000000000';
  return '0x' + [parent]
    .concat(
      domain
      .split('.')
      .reverse()
    )
    .reduce((parent, label) =>
      childhash(parent, label),
    );
}

function childhash(parentHash, label) {
  parentHash = parentHash.replace(/^0x/, '');
  const labelHash = sha256(label)
  return sha256(parentHash + labelHash, "hex");
}

function sha256(message, inputEnc) {
  return hash.sha256()
    .update(message, inputEnc)
    .digest('hex');
}
```

Sample namehash outputs

| domain | namehash |
| :--- | :--- |
| "" | 0x1c9ecec90e28d2461650418635878a5c91e49f47586ecf75f2b0cbb94e897112 |
| zil | 0x9915d0456b878862e822e2361da37232f626a2e47505c8795134a95d36138ed3 |
| brad.zil | 0x5fc604da00f502da70bfbc618088c0ce468ec9d18d05540935ae4118e8f50787 |

## Getting the resolver address

Our next step is to fetch two very important addresses attached to every Unstoppable Domain: the **owner address** and the **resolver contract address**. We can get them by querying the Unstoppable Domains [registry contract](https://viewblock.io/zilliqa/address/zil1jcgu2wlx6xejqk9jw3aaankw6lsjzeunx2j0jz)

The owner's address, as expected, is the address that owns a domain. The resolver contract address requires a little more explanation. All unstoppable domains are stored using two main smart contracts: a **Registry** contract and a **Resolver** contract.

| contract | explanation |
| :--- | :--- |
| Registry contract | Stores the owner's address and a resolver contract address |
| Resolver contract | Stores all records attached to the domain, such as BTC address or an IPFS website |

So, in order to get the BTC address from a domain we will need two queries. One to check the registry for the appropriate resolver address. And one to check the resolver for the records.

Let's write a function to make a JSON-RPC POST API request to the Zilliqa blockchain using their gateway. This function will take an array of parameters that we want to send and make a POST call to the Zilliqa API.

#### Fetch the contract addresses

```javascript
// index.js
async function fetchZilliqa(params) {
  const body = {
    method: "GetSmartContractSubState",
    id: "1",
    jsonrpc: "2.0",
    params
  };

  return await fetch(ZILLIQA_API, {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(body),
  }).then(res => res.json());
}
```

The parameters we need to send are 

* The **contract address** we want to query,
* The **contract field name** we will use string `records`,
* The **contract state keys** array of strings, in our case namehash of the domain is the only value in the array, 

Let's update our resolve function and use the **fetchZilliqa** function

#### Adding address resolution

```javascript
// index.js
async function resolve() {
  const userInput = document.getElementById("input").value;
  if (!userInput.endsWith(".zil") {
    // placeholder for future error handling
    return;
  }

  const hash = namehash(userInput);
  const registryState =
    await fetchZilliqa([UD_REGISTRY_CONTRACT_ADDRESS, "records", [hash]]);

  if (contractAddress.result === null) {
    // placeholder for future error handling
    return;
  }
  const [ownerAddress, resolverAddress] =
  registryState.result.records[hash].arguments; console.log({
    ownerAddress,
    resolverAddress
  });

}
```
**fetchZilliqa("brad.zil")** returns following:

```javascript
{
  id: "1",
  jsonrpc: "2.0",
  result: {
    records: {
      0x5fc604da00f502da70bfbc618088c0ce468ec9d18d05540935ae4118e8f50787: {
        argtypes: [],
        arguments: [
          "0x2d418942dce1afa02d0733a2000c71b371a6ac07",
          "0xdac22230adfe4601f00631eae92df6d77f054891"
        ],
        constructor: "Record"
      }
    }
  }
}
```

{% hint style="info" %}
Order is very important, as the first address in the arguments array is the owner address and the second one is a resolver contract address.
{% endhint %}

{% hint style="warning" %}
Make sure to display an appropriate error to your users if an owner address is not set. This means that the domain is not registered under any user and is available for purchase.
{% endhint %}

## Fetching domain records

After we verify that a domain has an owner address and we can query its resolver contract address for its records.

Use our `fetchZilliqa` function again, only this time change the parameters to contain the **resolver address**. For the state keys we can pass an empty array.

```typescript
// index.ts resolve function
const records = await fetchZilliqa([
    resolverAddress.replace("0x", ""),
    "records",
    []
  ]);
  console.log(records.result.records);
```

{% hint style="danger" %}
**It is very important to remove the leading 0x from the contract address, this is a requirement of the Zilliqa Blockchain API**
{% endhint %}

We should get an object printed to our console with all the keys registered under that domain. Let's test it out with domain `brad.zil`. As the result, you should get something similar to the following.

```javascript
{
  "crypto.BCH.address": "qrq4sk49ayvepqz7j7ep8x4km2qp8lauvcnzhveyu6",
  "crypto.BTC.address": "1EVt92qQnaLDcmVFtHivRJaunG2mf2C3mB",
  "crypto.DASH.address": "XnixreEBqFuSLnDSLNbfqMH1GsZk7cgW4j",
  "crypto.ETH.address": "0x45b31e01AA6f42F0549aD482BE81635ED3149abb",
  "crypto.LTC.address": "LetmswTW3b7dgJ46mXuiXMUY17XbK29UmL",
  "crypto.XMR.address": "447d7TVFkoQ57k3jm3wGKoEAkfEym59mK96Xw5yWamDNFGaLKW5wL2qK5RMTDKGSvYfQYVN7dLSrLdkwtKH3hwbSCQCu26d",
  "crypto.ZEC.address": "t1h7ttmQvWCSH1wfrcmvT4mZJfGw2DgCSqV",
  "crypto.ZIL.address": "zil1yu5u4hegy9v3xgluweg4en54zm8f8auwxu0xxj",
  "ipfs.html.value": "QmVaAtQbi3EtsfpKoLzALm6vXphdi2KjMgxEDKeGg6wHuK",
  "ipfs.redirect_domain.value": "www.unstoppabledomains.com",
}
```

## Displaying the result

Since this is a simple example we won't get too fancy. We'll just create a span element for each record containing its key and value, its owner address, and its resolver address. 

#### Displaying the domain records
```javascript
// index.js
function cleanDOM(parent) {
  while (parent.firstChild) {
    parent.removeChild(parent.firstChild);
  }
}

function displayResolution(resolution) {
  const {
    ownerAddress,
    resolverAddress,
    records
  } = resolution;
  const mainContainer = document.getElementById('records');
  cleanDOM(mainContainer);

  const ownerRecord = document.createElement('span');
  ownerRecord.innerHTML = `ownerAddress: ${ownerAddress}`;

  const resolverRecord = document.createElement('span');
  resolverRecord.innerHTML = `resolverAddress: ${resolverAddress}`;

  mainContainer.appendChild(ownerRecord);
  mainContainer.appendChild(resolverRecord);

  Object.entries(records).map(([key, value]) => {
    const recordSpan = document.createElement('span');
    recordSpan.innerHTML = `${key} : ${value}`;
    mainContainer.appendChild(recordSpan);
  });
}
```

We should see something like following on successful resolution.

![Screenshot](../.gitbook/assets/screen-shot-2021-01-26-at-10.33.11-pm.png)

### Error handling

Now that we have made a successful call let's deal with all possible errors that could happen during the resolution. For this purpose, we can create a function to place an error in our records div.

#### Handle errors
```javascript
// index.js
function displayError(message, cleanDom) {
  const mainContainer = document.getElementById('records');
  if (cleanDom) {
    cleanDOM(mainContainer);
  }
  const error = document.createElement('p');
  error.style.color = "red";
  error.innerHTML = message;
  mainContainer.appendChild(error);
  return ;
}
```

Error troubleshooting

| Error | Thrown when |
| :--- | :--- |
| **Domain is not registered** | We couldn't find an owner address |
| **Domain is not supported** | We are trying to resolve a domain that doesn't ends with .zil |
| **Domain is not configured** | It is possible that owner address exists while the resolver address is set to `0x00000000000000000000000000000000` |
| **Record is not found** | This is returned when you query a domain for a records like `crypto.BTC.address` which the domain owner has not set |

Although any string can be stored as a key under the domain, Unstoppable domains has [standardized some of the keys](../domain-registry-essentials/records-reference.md) across many applications.   

For the **Record is not found** error message we can check if the domain has a BTC address and if not we will show the error without cleaning the entire DOM.

![Example without BTC address](../.gitbook/assets/screen-shot-2021-01-26-at-10.53.37-pm.png)

#### Resolve function

```javascript
// index.js
async function resolve() {
  const userInput = (document.getElementById("input")).value;
  if (!userInput.endsWith(".zil")) {
    displayError('domain is not supported');
    return;
  }

  const hash = namehash(userInput);
  const contractAddresses = await fetchZilliqa([UD_REGISTRY_CONTRACT_ADDRESS, "records", [hash]]);
  console.log(contractAddresses);
  if (contractAddresses.result == null) {
    displayError('domain is not registered', true);
    return;
  }
  console.log(contractAddresses);
  const [ownerAddress, resolverAddress] = await contractAddresses.result.records[hash].arguments;

  if (resolverAddress === "0x0000000000000000000000000000000000000000") {
    displayError('domain is not configured', true);
    return;
  }

  const records = await fetchZilliqa([
    resolverAddress.replace("0x", ""),
    "records",
    []
  ]).then(data => (data.result.records));

  displayResolution({
    resolverAddress,
    ownerAddress,
    records
  });
}
```
#### Display resolution function

```javascript
// index.js
function displayResolution(resolution) {
  const {
    ownerAddress,
    resolverAddress,
    records
  } = resolution;
  const mainContainer = document.getElementById('records');
  cleanDOM(mainContainer);

  const ownerRecord = document.createElement('span');
  ownerRecord.innerHTML = `ownerAddress: ${ownerAddress}`;

  const resolverRecord = document.createElement('span');
  resolverRecord.innerHTML = `resolverAddress: ${resolverAddress}`;

  mainContainer.appendChild(ownerRecord);
  mainContainer.appendChild(resolverRecord);

  Object.entries(records).map(([key, value]) => {
    const recordSpan = document.createElement('span');
    recordSpan.innerHTML = `${key} : ${value}`;
    mainContainer.appendChild(recordSpan);
  });

  if (!records['crypto.BTC.address']) {
    displayError('crypto.BTC.address: Record is not found', false);
  }
}
```

{% hint style="info" %}
If domain doesn't have a resolver address it will be set as **"0x0000000000000000000000000000000000000000"**

_You can check it with the domain: **paulalcock.zil**_
{% endhint %}

At this point you can now resolve any .zil domain and show an appropriate error message for your users. 

Use `yarn dev` to kickstart the dev server and play with the app or build your app for future release with `yarn build`.

The full source code for this guide can be found on [github](https://github.com/unstoppable-domains-integrations/zil-Integration).

[![Get help on Discord](https://img.shields.io/badge/Get%20help%20on-Discord-blueviolet)](https://discord.gg/b6ZVxSZ9Hn)

If you have questions, visit our Unstoppable Domains Developer Community on Discord.
