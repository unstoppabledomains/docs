# Ethereum Providers

Resolution libraries require a connection to the Ethereum network to resolve .crypto and .eth domains. To initialize the library, you need to specify an Ethereum node service provider. Once the instance is created you can begin resolving domains. Below are examples of how to initialize the library with different providers.

### Import library

Javascript:

```javascript
import Resolution from "@unstoppabledomains/resolution";
```

### Infura

[Infura]("https://infura.io") is a popular Ethereum node service. Obtaining an API key is free and only requires creating an account.

Javascript:

```javascript
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

### Web3Provider

Connect a web3Provider. You may already have one available in your application from wallets like Metamask and WalletConnect.

Javascript:

```javascript
const web3Provider = window.ethereum;

// web3 0.x version provider
const resolution = Resolution.fromWeb3Version0Provider(web3Provider);
// or
// web3 1.x version provider
const resolution = Resolution.fromWeb3Version1Provider(web3Provider);
```

### EthersProvider

Connect a provider from [ethers.js]('https://www.npmjs.com/package/ethers')

Javascript:

```javascript
const resolution = Resolution.fromEthersProvider(ethersProvider);
```
