---
description: >-
  This tutorial will teach you how to make a JSON RPC call to our Zilliqa smart
  contract to get various records as well as owner address of a domain owner.
---

# Resolving .zil domain

## Steps to resolve unstoppable domain

In order to fetch any data from the Zilliqa blockchain you will need to do the following:

1. Get a namehash of a domain
2. Get resolver and owner addresses
3. Get the contract sub-state related to your domain

### Namehashing

Namehashing is an algorithm that converts a domain name in a classical format \(like www.example.crypto\) to ERC-721 token id. This process is described  in more details over  [here](https://docs.unstoppabledomains.com/domain-registry-essentials/namehashing)

```typescript
import hash from 'hash.js';

function namehash(domain: string): string {
    const parent =
      '0000000000000000000000000000000000000000000000000000000000000000';
    return '0x' + [parent]
      .concat(
        domain
          .split('.')
          .reverse()
          .filter(label => label),
      )
      .reduce((parent, label) =>
        childhash(parent, label),
      );
  }

function childhash( parent: string, label: string): string {
    parent = parent.replace(/^0x/, '');
    return sha256(parent + sha256(label), 'hex',);
  }

function sha256(message: string, inputEnc?: 'hex') {
    return hash.sha256()
      .update(message, inputEnc)
      .digest('hex');
  }
```

In more simplistic human words we are hashing all of the labels of the domain with the accumulated hash of its parents in order to get a unique ERC-721 token id. 

### Getting resolver and owner addresses

In order to get the desirable addresses, we would need a Zilliqa registry contract address 

| Zilliqa registry contract address | Zilliqa API url |
| :--- | :--- |
| 0x9611c53BE6D1b32058b2747bdeCECed7e1216793 | https://api.zilliqa.com/ |

Next, we are going to make a post API call to Zilliqa API with the parameters of our request

{% api-method method="post" host="https://api.zilliqa.com" path="/" %}
{% api-method-summary %}
​Fetch owner and resolver addresses
{% endapi-method-summary %}

{% api-method-description %}
For params, you will need to pass an array containing three values  
  
\* contractAddress  -&gt;  **"0x9611c53BE6d1b32058b2747bdeCECed7e1216793"**  
\* contractField -&gt; **"records"**  
\* Array of keys -&gt; **\[ namehash\(domain\) \]**
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Content-Type" type="string" required=true %}
application/json
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="params" type="array" required=true %}
Array of params, order is importand!  
contractAddress, contractField, \[keys\]
{% endapi-method-parameter %}

{% api-method-parameter name="method" type="string" required=true %}
"GetSmartContractSubState"
{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}
"1"
{% endapi-method-parameter %}

{% api-method-parameter name="jsonrpc" type="string" required=true %}
"2.0"
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
You can find owner address in result.records.nodehash.arguments as a first argument and resolver address as a second argument
{% endapi-method-response-example-description %}

```javascript
{
"id":"1",
"jsonrpc":"2.0",
"result":{
    "records":{
        "0x08ab2ffa92966738c881a37d0d97f168d2e076d24639921762d0985ebaa62e31":{
            "argtypes":[],
            "arguments":[
                "0xcea21f5a6afc11b3a4ef82e986d63b8b050b6910",
                "0x34bbdee3404138430c76c2d1b2d4a2d223a896df"
            ],
            "constructor":"Record"
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Get the resolver contract substate

Resolver contract is the one who contains all possible records that has been stored under the domain, so all what we need to do is to repeat the api call with slightly different **params** and a different **contractAddress**.

{% hint style="info" %}
Every field should stay the same, only params are allowed to be changed for a successfull call
{% endhint %}

{% api-method method="post" host="https://api.zilliqa.com" path="/" %}
{% api-method-summary %}
Fetch domain records
{% endapi-method-summary %}

{% api-method-description %}
For params you will need to pass an array containing three values:  
  
\* contractAddress -&gt; **resolverContract address from the previous step  
\*** contractField -&gt; **records  
\*** array of keys -&gt; **\[ \] empty array**
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Content-type" type="string" required=true %}
application/json
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="params" type="string" required=true %}
Array of params, order is importand!  
contractAddress, contractField, \[keys\]
{% endapi-method-parameter %}

{% api-method-parameter name="method" type="string" required=true %}
"GetSmartContractSubState"
{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}
"1"
{% endapi-method-parameter %}

{% api-method-parameter name="jsonrpc" type="string" required=true %}
"2.0"
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```typescript
{
"id":"1",
"jsonrpc":"2.0",
"result":{
    "records":{
        "crypto.ETH.address":"0xe7474D07fD2FA286e7e0aa23cd107F8379085037",
        "ipfs.html.value":"QmQ38zzQHVfqMoLWq2VeiMLHHYki9XktzXxLYTWXt8cydu",
        "whois.email.value":"jeyhunt@gmail.com"
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}