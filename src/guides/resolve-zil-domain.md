---
description: Simplest step-by-step guide on how to resolve .zil domain
---

# .zil Integration

In order to resolve a .zil domain, we will need to satisfy three steps below

* Get a namehash of a domain
* Get resolver contract address from a domain
* Query resolver contract to fetch the records

Let's visualize the resolution process using some of the simplest tools web developer has: knowledge of HTML and js.

### Initialize the project folder

As has been said above all we need is to create a folder and two files index.html and index.js respectively

```text
mkdir unstoppable-zil-resolution
cd unstoppable-zil-resolution
touch index.html index.js
```

Let's create a blank HTML page. We have connected our empty index.js as well as a CDN library of [**js-sha256**](https://www.npmjs.com/package/js-sha256) for future namehashing step.

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>Basic .zil integration</title>
    </head>
    <body>

      <script 
        src="https://cdnjs.cloudflare.com/ajax/libs/js-sha256/0.9.0/sha256.min.js" 
        integrity="sha512-szJ5FSo9hEmXXe7b5AUVtn/WnL8a5VofnFeYC2i2z03uS2LhAch7ewNLbl5flsEmTTimMN0enBZg/3sQ+YOSzQ==" 
        crossorigin="anonymous"></script>

      <script src="index.js"></script>
    </body>
</html>
```

Don't forget to add an input field and a button that will trigger the resolution process

```markup
      <input id="input" />
      <button onclick="resolve()">Resolve</button>
```

#### As for our index.js file

We are going to put some basic code to capture the text from the input field and print it in our console

```javascript
async function resolve() {
  const userInput = document.getElementById("input").value;
  console.log({ domain: userInput });
}
```

### Taking a namehash

Namehashing is an algorithm that converts a domain name in a classical format \(like example.crypto\) to a token id that Zilliqa contract can understand. 

{% hint style="warning" %}
It is important to know the difference between Zilliqa namehashing and [ERC-721](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-137.md#namehash-algorithm) which is a part of [EIP-137](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-137.md).
In ZIL we use **sha256 from SHA-2**, instead of **keccak256** which is used across Ethereum chain
{% endhint %}

To do so we need to split the domain by "." character to get each label and then reduce the label's array with a sha256 hashing of an accumulator and next label starting from the end. 

For the purposes of keeping this tutorial short, instead of going into the details of this process, we are going to use the namehash function with some adaptation to the hashing library

{% hint style="info" %}
Don't forget to add the js-sha256 library to the project in order to use the sha256 function. 
{% endhint %}

```javascript
// <!-------> Namehashing functions <!------->
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

function childhash( parent, label) {
    parent = parent.replace(/^0x/, '');
    return shaWrapper(parent + shaWrapper(label), "hex");
  }

function shaWrapper(msg, inputEnc) {
  if (inputEnc === "hex") {
    var res = [];
    msg = msg.replace(/[^a-z0-9]+/ig, '');
      if (msg.length % 2 !== 0)
        msg = '0' + msg;
    for (var i = 0; i < msg.length; i += 2)
        res.push(parseInt(msg[i] + msg[i + 1], 16));
    return sha256(res);
  }
  return sha256(msg);
}
```

{% hint style="info" %}
namehash takes a string domain, splits it by the '.' and then applies a childhash function to each of the combined with the previous results
{% endhint %}

{% hint style="info" %}
shaWrapper function is needed to convert the string into an array of hex values. This step is required when we concatenate sha256 of accumulated results with the sha256 of the next label. 

Some hashing libraries like`hash.js`has the functionality to take input as a hex value instead of a character string. In this case, the shaWrapper function can be omitted. 
{% endhint %}

Below you can find a table of some examples for namehashing

| "" | 0x0000000000000000000000000000000000000000000000000000000000000000 |
| :--- | :--- |
| zil | 0x9915d0456b878862e822e2361da37232f626a2e47505c8795134a95d36138ed3 |
| brad.zil | 0x5fc604da00f502da70bfbc618088c0ce468ec9d18d05540935ae4118e8f50787 |

### Getting resolver address

Our next step is to fetch two very important addresses attached to every unstoppable domain: **owner address** and **resolver contract address**

In order to do so, we will make a post api call to zilliqa with specific parameters.

```javascript
const ZILLIQA_API = "https://api.zilliqa.com/";

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
    body: JSON.stringify(body)
  }).then(res => res.json());
}
```

The only parameter that is going to be changed is the params field which is nothing else but a simple array with strictly 3 arguments: **contract address**, **contract field name**, **state key**

Let's update our resolve function and use the **fetchZilliqa** function with the following params

```javascript
const UD_REGISTRY_CONTRACT = "9611c53BE6d1b32058b2747bdeCECed7e1216793";

async function resolve() {
  const userInput = document.getElementById("input").value;
  const hash = namehash(userInput);
  
  const contractAddresses = 
    await fetchZilliqa([UD_REGISTRY_CONTRACT, "records", [hash]]);
  const [ownerAddress, resolverAddress] = 
    contractAddresses.result.records[hash].arguments;
  
  ...
}
```

Calling fetchZilliqa with namehash of brad.zil returns us the following:

```javascript
{
id: "1"
jsonrpc: "2.0"
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
Order is very important, as the first address in the arguments array is the owner address and the second one is a resolver contract address
{% endhint %}

{% hint style="warning" %}
Make sure to display your user an appropriate error if an owner address is not set. This means that the domain is not registered under any user and is free to be taken. 
{% endhint %}

### Fetching the domain records

As the last step we are going to make use of fetchZilliqa again, only this time we will change our params to contain the resolver address and for the state key we will pass an empty array

```javascript
const records = await fetchZilliqa([
    resolverAddress.replace("0x", ""),
    "records",
    []
  ]);
  console.log(records.result.records);
```

{% hint style="danger" %}
**It is very important to remove the leading 0x from the contract address**
{% endhint %}

We should get an object printed on our console with all the keys registered under the domain.

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

Congratulation, you have successfully resolved a .zil domain using nothing but some HTML and js. 
