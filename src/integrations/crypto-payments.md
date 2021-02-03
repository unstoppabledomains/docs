# Crypto Payments

## Table of contents

TBD

## Overview

The most common way to integrate Unstoppable Domains in the application is translate `.crypto` or `.zil` into the
cryptocurrency addresses like `BTC, ETH` and more when the user is sending cryptocurrency or token. Instead of putting a
long hash (cryptocurrency address) in `Receiver / Send to:` field the user just typing receiver `.crypto` or `.zil`
name.

![success payment example](../.gitbook/assets/integrations/crypto-payments/success-payment-example.gif)

On a high-level an application reads domain records from smart contracts deployed at `Ethereum` and
`Zilliqa` blockchains (for `.crypto` and `.zil` accordingly). Records are stored in smart contracts storage which
basically is a key-value storage. In the example we need to send ether to specified domain. To achieve this we need to
read record attached to the domain by the reserved key: `crypto.ETH.address`. The value will be the Ethereum address
attached to the receiver's domain name.

{% hint style="info" %}  
A domain can store various types of records and key formats. To learn about all supported records
see [records reference guide.](../domain-registry-essentials/records-reference.md)
{% endhint %}

![success payment flow](../.gitbook/assets/integrations/crypto-payments/success-payment-flow.svg)

## Integrate resolution using libraries

The easiest way to integrate domain resolution for crypto payments is using resolution libraries maintained by
Unstoppable.

// todo Merge import and resolution
### Import library

{% tabs %}

{% tab title="resolution" %} Resolution library is suitable for `javascript/typescript/react native` applications.

```javascript
const {default: Resolution} = require('@unstoppabledomains/resolution');
const resolution = new Resolution();
```

{% endtab %}

{% tab title="resolution-java" %}

```java
import com.unstoppabledomains.resolution.Resolution

class Resolve {
    public static void main(String[] args) {
        DomainResolution resolution = new Resolution();
    }
}
```

{% endtab %}

{% tab title="resolution-swift" %}

```swift
import UnstoppableDomainsResolution

guard let resolution = try? Resolution() else {
  print ("Init of Resolution instance with default parameters failed...")
  return
}
```

{% endtab %}

{% endtabs %}

{% hint style="danger" %} To communicate with Ethereum libraries use Linkpool Ethereum public provider by default with
low rate limit threshold. For production usage it's required to switch for suitable Ethereum provider with high
rate-limits threshold. To configure library properly
use [library configuration guide.](../integrations/library-configuration.md)
{% endhint %}

### Resolve crypto records

In order to support crypto payment to domain - a domain name should be resolved intro blockchain address. To achieve
this we need to resolve domain into cryptocurrency address.

#### Resolve `ryan.crypto` into ETH address

{% tabs %}

{% tab title="resolution" %}

```javascript
```

{% endtab %}

{% tab title="resolution-java" %}

```java
```

{% endtab %}

{% tab title="resolution-swift" %}

```swift
```

{% endtab %}

{% endtabs %}

##### Records involved

#### Resolve `example.crypto` into USDT-ERC20 address

// Resolve USDT

##### Records involved

// Records involved //// USDT include // Crypto tickers reference

#### Multi chain currencies

### Errors handling

// gif

// flow

## Links

