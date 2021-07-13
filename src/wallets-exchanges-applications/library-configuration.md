# Library configuration

Resolution libraries require a connection to the Ethereum network to resolve .crypto and .eth domains. To initialize the library, you need to specify an Ethereum node service provider. Once the instance is created you can begin resolving domains. Below are examples of how to initialize the library with different providers.

Each of the resolution libraries supports an Ethereum provider url for configuration. You can obtain a provider url from a service like [Infura](https://infura.io) where obtaining an API key is free and only requires creating an account.

To choose an alternative Ethereum provider see [Nodes as a Service guide.](https://ethereum.org/en/developers/docs/nodes-and-clients/nodes-as-a-service/)

{% hint style="info" %}
Unstoppable libraries use Infura provider by default without restrictions and rate limits for CNS \(.crypto\) resolution. Default configuration can be considered as production-ready.

For ENS \(.eth\) resolution it's recommended update Ethereum provider to use in production.
{% endhint %}

* [JavaScript Resolution library](library-configuration.md#javascript-resolution-library) 
* [Java Resolution library](library-configuration.md#java-resolution-library)
* [Swift Resolution library](library-configuration.md#swift-resolution-library)

## JavaScript Resolution library

Configuration for the [Javascript resolution library](https://github.com/unstoppabledomains/resolution)

### Provider URL

```javascript
import Resolution from "@unstoppabledomains/resolution";

const infuraApiKey = INFURA_PROJECT_ID;

const resolution = new Resolution({
  cns: {
    url: `https://mainnet.infura.io/v3/${infuraApiKey}`,
    network: "mainnet",
  },
});

// or

const resolution = Resolution.infura(infuraApiKey, "mainnet");
```

### Web3 provider

Connect a web3 provider. You may already have one available in your application from wallets like Metamask and WalletConnect.

```javascript
import Resolution from "@unstoppabledomains/resolution";

// if web3rovider is attached to window
const web3Provider = window.ethereum;

// if web3Provider.version - 0.x
const resolution = Resolution.fromWeb3Version0Provider(web3Provider);
// or
// if web3Provider.version - 1.x
const resolution = Resolution.fromWeb3Version1Provider(web3Provider);
```

### Ethers provider

Connect a provider from [ethers.js](https://www.npmjs.com/package/ethers)

```javascript
import Resolution from "@unstoppabledomains/resolution";

const resolution = Resolution.fromEthersProvider(ethersProvider);
```

## Java Resolution library

Configuration for the [Java resolution library](https://github.com/unstoppabledomains/resolution-java)

### Provider URL

```java
import com.unstoppabledomains.resolution.Resolution

String infuraApiKey = INFURA_PROJECT_ID;
String providerURL = "https://mainnet.infura.io/v3/" + infuraApiKey

DomainResolution resolution = Resolution.builder()
                .providerUrl(NamingServiceType.CNS, providerURL)
                .build();
```

## Swift Resolution library

Configuration for the [Swift resolution library](https://github.com/unstoppabledomains/resolution-swift)

### Provider URL

```swift
import UnstoppableDomainsResolution

let infuraApiKey = INFURA_PROJECT_ID

guard let resolution = try? Resolution(providerUrl: "https://mainnet.infura.io/v3/" + infuraApiKey, network: "mainnet") else {
  print ("Init of Resolution instance with custom parameters failed...")
  return
}
```

