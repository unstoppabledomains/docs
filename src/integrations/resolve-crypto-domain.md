---
description: Simplest step-by-step guide on how to resolve .crypto domain
---

# .crypto Integration

## Plan

In order to resolve a .crypto domain, we will need to

* Get a namehash of a domain
* Configure ethers.js library to call Unstoppable contract
* Make a call and fetch the data

Let's visualize the resolution process using some of the simplest tools web developer has: knowledge of HTML and js.

### Initialize the project folder

As has been said above all we need is to create a folder and three files index.html, index.js, and ethers.js respectively

```text
mkdir unstoppable-zil-resolution
cd unstoppable-zil-resolution
touch index.html index.js ethers.js
```

Let's use some blank HTML page. We have connected our empty index.js as well as some dependencies

We will need the keccak\_256 hash function from [js-sha3](https://www.npmjs.com/package/js-sha3?utm_source=cdnjs&utm_medium=cdnjs_link&utm_campaign=cdnjs_library) in order to find a namehash for the .crypto domain, We also need the [ethers.js](https://docs.ethers.io/v5/getting-started/) library to easily connect with the Unstoppable Proxy Reader contract.

```markup
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>Basic .zil integration</title>
    </head>
    <body>

      <!-- This exposes keccak_256 hash function -->
      <script
        src="https://cdnjs.cloudflare.com/ajax/libs/js-sha3/0.8.0/sha3.min.js"
        integrity="sha512-PmGDkK2UHGzTUfkFGcJ8YSrD/swUXekcca+1wWlrwALIZho9JX+3ddaaI9wmmf8PmgDIpMtx6TU8YBJAZS0mPQ=="
        crossorigin="anonymous">
      </script>
      <!-- This exposes the library as a global variable: ethers -->
      <script src="https://cdn.ethers.io/lib/ethers-5.0.umd.min.js"
        type="application/javascript"></script>
      <!-- This is our custom files -->
      <script src="ethers.js"></script>
      <script src="index.js"></script>
    </body>
</html>
```

{% hint style="danger" %}
Also do not forget to include our two files ethers.js and index.js
{% endhint %}

Don't forget to add an input field and a button that will trigger the resolution process

```markup
      <input id="input" />
      <button onclick="resolve()">Resolve</button>
```

### As for our index.js file

We are going to put some basic code to capture the text from the input field and print it in our console

```javascript
// <!-------> Resolving domain <!------->
async function resolve() {
  const userInput = document.getElementById("input").value;
  console.log({ domain: userInput });
}
```

## Taking a namehash

Namehashing is an algorithm that converts a domain name in a classical format \(like "example.crypto"\) to ERC-721 token id. This process is described in more details over our [namehash article](https://docs.unstoppabledomains.com/domain-registry-essentials/namehashing) 

For the purposes of keeping this tutorial short, instead of going into the details of this process, we are going to use the namehash function with some adaptation to the hashing library

{% hint style="info" %}
Don't forget to add the [js-sha3](https://cdnjs.cloudflare.com/ajax/libs/js-sha3/0.8.0/sha3.min.js) library to the project in order to use the keccak\_256 function.
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

function childhash(parent, label) {
    parent = parent.replace(/^0x/, '');
    const childHash = keccak_256_wrapper(label);
    // eslint-disable-next-line no-undef
    return keccak_256_wrapper(parent + childHash, "hex");
}

function keccak_256_wrapper(msg, inputEnc) {
  if (inputEnc === "hex") {
    var res = [];
    msg = msg.replace(/[^a-z0-9]+/ig, '');
      if (msg.length % 2 !== 0)
        msg = '0' + msg;
    for (var i = 0; i < msg.length; i += 2)
        res.push(parseInt(msg[i] + msg[i + 1], 16));
    return keccak_256(res);
  }
  return keccak_256(msg);
}
```

{% hint style="info" %}
keccak\__256\__wrapper function is needed to convert the hex string into an array of hex values. This step is required when we concatenate the hash of accumulated results with the hash of the next label.
{% endhint %}

Below you can find a table of some examples for namehashing

| "" | 0x88d4843af302c2093286898cd34cba7a471c3cdce4c78514fc971c3c6a53891e |
| :--- | :--- |
| crypto | 0x0f4a10a4f46c288cea365fcf45cccf0e9d901b945b9829ccdb54c10dc3cb7a6f |
| brad.crypto | 0x756e4e998dbffd803c21d23b06cd855cdc7a4b57706c95964a37e24b47c10fc9 |

## Configuring Ethers library

In order to talk with any blockchain contract using ethers.js we need to know the following 

* Contract address
* Contract interface a.k.a. abi
* Provider

Let's add the following information to our **ethers.js** file

```javascript
var address = '0xa6E7cEf2EDDEA66352Fd68E5915b60BDbb7309f5';
var abi = [
  {
    constant: true,
    inputs: [
      {
        internalType: 'string[]',
        name: 'keys',
        type: 'string[]',
      },
      {
        internalType: 'uint256',
        name: 'tokenId',
        type: 'uint256',
      },
    ],
    name: 'getData',
    outputs: [
      {
        internalType: 'address',
        name: 'resolver',
        type: 'address',
      },
      {
        internalType: 'address',
        name: 'owner',
        type: 'address',
      },
      {
        internalType: 'string[]',
        name: 'values',
        type: 'string[]',
      },
    ],
    payable: false,
    stateMutability: 'view',
    type: 'function',
  }
];
var provider = ethers.providers.getDefaultProvider('mainnet');
```

{% hint style="info" %}
For the scope of this project, we are going to need only _getData_ function from our [proxy reader contract](https://etherscan.io/address/0xa6E7cEf2EDDEA66352Fd68E5915b60BDbb7309f5#code).
{% endhint %}

Next, we need to create a contract instance and export the function to make the query to our contract

```javascript
var contract = new ethers.Contract(address, abi, provider);

async function fetchContractData(keys, tokenId) {
  return contract.getData(keys, tokenId);
}
```

By inspecting the getData function interface we can see that it requires from us an **array of keys** and **tokenId**. We can get **tokenId** by calling **namehash** function from above. 

As for the keys, although anything can be stored under the domain we have a [set of standardized records](https://docs.unstoppabledomains.com/domain-registry-essentials/records-reference) that are widely used across multiple dapp applications.

In this tutorial, we are going to be looking up for the following records:

| Key | Description |
| :--- | :--- |
| crypto.BTC.address | BTC address attached to the domain |
| crypto.ETH.address | ETH address attached to the domain |
| gundb.username.value | Gundb username attached to the domain |
| gundb.public\_key.value | Gundb public key attached to the domain |

## Making the call to the contract

Let's update our resolve function to use namehash, and look up the desired record keys from the input domain name

```javascript
async function resolve() {
  const userInput = document.getElementById("input").value;
  const tokenId = namehash(userInput);
  
  const interestedKeys = [
    "crypto.BTC.address",
    "crypto.ETH.address",
    "gundb.username.value",
    "gundb.public_key.value",
  ];
  
  fetchContractData(interestedKeys, tokenId).then(data => {
    console.log({
      ownerAddress: data.owner,
      resolverAddress: data.resolver,
      records: data[2]
    });
  });
}
```

Trying to resolve brad.crypto with the above keys returns us the following in the console

```javascript
{
  "ownerAddress":"0x8aaD44321A86b170879d7A244c1e8d360c99DdA8",
  "resolverAddress":"0xb66DcE2DA6afAAa98F2013446dBCB0f4B0ab2842",
  "records":[
    "bc1q359khn0phg58xgezyqsuuaha28zkwx047c0c3y",
    "0x8aaD44321A86b170879d7A244c1e8d360c99DdA8",
    "0x8912623832e174f2eb1f59cc3b587444d619376ad5bf10070e937e0dc22b9ffb2e3ae059e6ebf729f87746b2f71e5d88ec99c1fb3c7c49b8617e2520d474c48e1c",
    "pqeBHabDQdCHhbdivgNEc74QO-x8CPGXq4PKWgfIzhY.7WJR5cZFuSyh1bFwx0GWzjmrim0T5Y6Bp0SSK0im3nI"
    ]
}
```

{% hint style="info" %}
data\[2\] is an array containing all resolved records in the same order as has been sent. In this case, the first argument is BTC address and the last one is a gundb public key attached as an example.
{% endhint %}

Congratulations, you have successfully resolved _.crypto domain_. 