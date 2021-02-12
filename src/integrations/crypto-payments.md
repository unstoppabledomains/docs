# Crypto Payments

## Table of contents

- [Overview](crypto-payments.md#overview)
- [Integrate resolution using libraries](crypto-payments.md#integrate-resolution-using-libraries)
  - [Resolve crypto records](crypto-payments.md#resolve-crypto-records)
  - [Errors handling](crypto-payments.md#errors-handling)
  - [Best practices](crypto-payments.md#best-practices)
- [Links](crypto-payments.md#links)    

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
Unstoppable. The libraries communicate with Ethereum and Zilliqa blockchain directly.

### Resolve crypto records

#### Resolve `ryan.crypto` into Ethereum address

In order to support crypto payment to domain - a domain name should be resolved intro blockchain address. To achieve
this we need to resolve domain into cryptocurrency address.

{% tabs %}

{% tab title="resolution" %} Resolution library is suitable for `javascript/typescript/react native` applications.

```javascript
const {default: Resolution} = require('@unstoppabledomains/resolution');
const resolution = new Resolution();
resolution
    .addr('ryan.crypto', 'ETH')
    .then((receiverETHAddress) => {
        // receiverETHAddress consists receiver ethereum address
        // use this address as recipient of the payment
    })
    .catch(console.error);
```

{% endtab %}

{% tab title="resolution-java" %}

```java
import com.unstoppabledomains.resolution.Resolution
...
DomainResolution resolution = new Resolution();
String receiverETHAddress = resolution.getAddress("ryan.crypto", "ETH");
// receiverETHAddress consists receiver ethereum address
// use this address as recipient of the payment
```

{% endtab %}

{% tab title="resolution-swift" %}

```swift
import UnstoppableDomainsResolution

guard let resolution = try? Resolution() else {
  print ("Init of Resolution instance with default parameters failed...")
  return
}

resolution.addr(domain: "ryan.crypto", ticker: "ETH") { result in
  switch result {
  case .success(let returnValue):
    let receiverETHAddress = returnValue
    // receiverETHAddress consists receiver ethereum address
    // use this address as recipient of the payment
  case .failure(let error):
    print("Expected eth Address, but got \(error)")
  }
}
```

{% endtab %}

{% endtabs %}

{% hint style="danger" %} To communicate with Ethereum libraries use Linkpool Ethereum public provider by default with
low rate limit threshold. For production usage it's required to switch on Ethereum provider with high rate-limits
threshold. To configure library properly use [library configuration guide.](../integrations/library-configuration.md)
{% endhint %}

##### Records involved

`addr() / getAddr()` methods convert provided 3-letters ticker intro `crypto.<TICKER>.address` and reads this key value
for provided domain. In case of `ryan.crypto` resolution `ETH` ticker becomes `crypto.ETH.address` when the library
makes query to the blockchain.

#### Resolve `udtestdev-usdt.crypto` into USDT-ERC20 address

`USDT` currency exists in multiple blockchains it requires a different key format to store. Libraries provide dedicated
method to query `USDT` address for different blockchains.

{% tabs %}

{% tab title="resolution" %} Resolution library is suitable for `javascript/typescript/react native` applications.

```javascript
const {default: Resolution} = require('@unstoppabledomains/resolution');
const resolution = new Resolution();
resolution
    .usdt('udtestdev-usdt.crypto', 'ERC20')
    .then((receiverUSDTAddress) => {
        // receiverUSDTAddress consists address for receiving USDT on Ethereum (ERC20 version)
        // use this address as recipient of the payment
    })
    .catch(console.error);
```

{% endtab %}

{% tab title="resolution-java" %}

```java
import com.unstoppabledomains.resolution.Resolution
import com.unstoppabledomains.resolution.TickerVersion
...
DomainResolution resolution = new Resolution();
String receiverUSDTAddress = resolution.getUsdt("udtestdev-usdt.crypto", TickerVersion.ERC20);
// receiverUSDTAddress consists address for receiving USDT on Ethereum (ERC20 version)
// use this address as recipient of the payment
```

{% endtab %}

{% tab title="resolution-swift" %}

```swift
import UnstoppableDomainsResolution

guard let resolution = try? Resolution() else {
  print ("Init of Resolution instance with default parameters failed...")
  return
}

resolution.usdt(domain: "udtestdev-usdt.crypto", version: .ERC20) { (result) in
  switch result {
  case .success(let returnValue):
     receiverUSDTAddress = returnValue;
     // receiverUSDTAddress consists address for receiving USDT on Ethereum (ERC20 version)
     // use this address as recipient of the payment
  case .failure(let error):
     print("Expected USDT-ETC20 Address, but got \(error)")
  }
}
```

{% endtab %}

{% endtabs %}

##### Records involved

`usdt() / getUsdt()` methods create a key from provided USDT version. The key format
is `crypto.USDT.version.<VERSION>.address`. When getting USDT-ERC20 version the key would
be `crypto.USDT.version.ERC20.address` when the library makes query to the blockchain.

{% hint style="info" %} To get detailed information about supported crypto payment tickers and USDT versions
read [Crypto Payment Records section in Managing domain records article](../managing-domains/managing-domain-records.md#crypto-payment-records)  
{% endhint %}

### Errors handling

![errors example](../.gitbook/assets/integrations/crypto-payments/errors-example.gif)  

There are 4 main error cases need to be handled by the client. 
The libraries return errors with additional information to distinguish error types.  

- Domain is not registered  
- Crypto record is not found (or empty)  
- Domain is not configured (empty resolver)  
- Domain is not supported  

{% tabs %}

{% tab title="resolution" %} 
```javascript
const {default: Resolution} = require('@unstoppabledomains/resolution');
const resolution = new Resolution();
resolution
    .addr('domain-with-error.crypto', 'ETH')
    .then((ethAddress) => {
    })
    .catch((error) => {
        if (error.code === 'UnregisteredDomain') {
            console.log('Domain is not registered')
        }
        if (error.code === 'RecordNotFound') {
            console.log('Crypto record is not found (or empty)')
        }
        if (error.code === 'UnspecifiedResolver') {
            console.log('Domain is not configured (empty resolver)')
        }
        if (error.code === 'UnsupportedDomain') {
            console.log('Domain is not supported')
        }
    });
```   

To see all supported error codes please check 
[resolution library api docs](https://unstoppabledomains.github.io/resolution/v1.17.0/enums/resolutionerrorcode.html)      
{% endtab %}

{% tab title="resolution-java" %}
```java
import com.unstoppabledomains.resolution.Resolution
import com.unstoppabledomains.exceptions.ns.NamingServiceException
import com.unstoppabledomains.exceptions.ns.NSExceptionCode

...
DomainResolution resolution = new Resolution();
try {
    String receiverETHAddress = resolution.getAddress("domain-with-error.crypto", "ETH");
} catch (NamingServiceException exception) {
   if (exception.getCode() == NSExceptionCode.UnregisteredDomain) {
        // Domain is not registered
   }
   if (exception.getCode() == NSExceptionCode.RecordNotFound) {
        // Crypto record is not found (or empty)
   }
   if (exception.getCode() == NSExceptionCode.UnspecifiedResolver) {
        // Domain is not configured (empty resolver)
   }
   if (exception.getCode() == NSExceptionCode.UnsupportedDomain) {
        // Domain is not supported
   }
}
```  


To see all supported error codes please check
[resolution-java readme](https://github.com/unstoppabledomains/resolution-java#errors)
{% endtab %}

{% tab title="resolution-swift" %}
```swift
import UnstoppableDomainsResolution

guard let resolution = try? Resolution() else {
  print ("Init of Resolution instance with default parameters failed...")
  return
}

resolution.addr(domain: "domain-with-error.crypto", ticker: "ETH") { result in
  switch result {
      case .success(let returnValue):
        // Success flow
      case .failure(let error):
            switch error {
                case ResolutionError.unregisteredDomain:
                    // Domain is not registered
                    break;
                
                case ResolutionError.recordNotFound:
                    // Crypto record is not found (or empty)
                    break;
                
                case ResolutionError.unspecifiedResolver:
                    // Domain is not configured (empty resolver)
                    break;
                
                case ResolutionError.unsupportedDomain:
                    // Domain is not supported
                    break;
            }
  }
}
```

To see all supported error codes please check
[resolution-swift readme](https://github.com/unstoppabledomains/resolution-swift#possible-errors)
{% endtab %}

{% endtabs %}

{% hint style="danger" %}
Alert: always check address validity after receiving result from the library. 
The user has a full control over the domain - any value could be set as crypto record.  
{% endhint %}

## Integration best practices

- Always show resolved address near the domain name.
- Don’t overwrite the input field with the cryptocurrency address.
- Always try to resolve domain with provided currency code.
- Always handle resolution errors according to error type.
- Support .crypto and .zil domains.

## Links
- [Get test domain](./get-test-domain.md)
- [Discord community](https://discord.com/invite/b6ZVxSZ9Hn)
- [Resolution libraries](./libraries-list.md)

