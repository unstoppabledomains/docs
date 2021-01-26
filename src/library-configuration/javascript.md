## Javascript / Typescript

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

Connect a provider from [ethers.js]('https://www.npmjs.com/package/ethers')

```javascript
import Resolution from "@unstoppabledomains/resolution";

const resolution = Resolution.fromEthersProvider(ethersProvider);
```
