# Resolving Domain Records

Resolving a domain is a process of retrieving a domain records when the domain name and required record names are given. There is no limitation on who can read domain records on Registry side. Anyone having access to Ethereum Node on the mainnet can resolve a domain.

This section describes resolving domain records with making calls to Ethereum smart contracts, using Ethereum JSON RPC. For developers, who are interested in more high level solution, it might be more convenient to use resolution libraries instead. Unstoppable team supports the following libraries for domain resolution:

*  [resolution.js](https://github.com/unstoppabledomains/resolution)
*  [resolution-java](https://github.com/unstoppabledomains/resolution-java)
*  [resolution-swift](https://github.com/unstoppabledomains/resolution-swift) _\(work in progress\)_

Resolving a domain requires a software to have access to Ethereum network. For more information, see [Configuring Ethereum Network connection](resolving-domain-records.md#configuring-ethereum-network-connection).

The simplest way to resolve a domain with Ethereum JSON RPC is to make a readonly call to ProxyReader smart contract. ProxyReader provides an API that allows users to resolve domains making just one call, passing only keys of records and a domain namehash. Without ProxyReader it would require executing at least two calls: one to obtain a domain resolver address and another one to get the records themselves. With ProxyReader it all happens under the hood.

An example in JavaScript of getting two records \(using [ethers library](https://www.npmjs.com/package/ethers)\):

```javascript
const proxyReaderAddress = '0x7ea9Ee21077F84339eDa9C80048ec6db678642B1';
// Partial ABI, just for the getMany function.
const proxyReaderAbi = [
  'function getMany(string[] calldata keys, uint256 tokenId) external view returns (string[] memory)'
];
const proxyReaderContract = new ethers.Contract(
  proxyReaderAddress,
  proxyReaderAbi,
  provider,
);

const domain = 'brad.crypto';
const tokenId = namehash(domain);
const keys = ['crypto.ETH.address', 'crypto.BTC.address'];

const values = await proxyReaderContract.getMany(keys, tokenId);
console.log(values);
// [
//   '0x8aaD44321A86b170879d7A244c1e8d360c99DdA8',
//   'bc1q359khn0phg58xgezyqsuuaha28zkwx047c0c3y'
// ]
```

Reference:

* `namehash` - namehashing algorithm implementation. See [Namehashing](namehashing.md).

![](../.gitbook/assets/provide_domain_records_via_proxy_reader_smart_contract.png)

See [Records Reference](reference.md) for more information about the standardized records.

## Record Value Validation

Crypto resolver doesn't have any built-in record value validation when it is updated for two reasons:

* Any validation would require additional gas to be paid
* Solidity is special purpose programming language that doesn't have any built-in data validation tools like Regular Expressions

Any domain management application must perform record format validation before submitting a transaction. However, there is no guarantee that all management application will do it correctly. That is why records must be validated when domain is resolved too.

See [Records Reference](reference.md) for more information for the validator of each record.

## Configuring Ethereum Network connection

Domain Resolution Configuration at low level requires 3 configuration parameters:

1. Ethereum JSON RPC provider
2. Ethereum CHAIN ID
3. Crypto Registry Contract Address

Ethereum JSON RPC provider is an API implementing Ethereum JSON RPC standard. Usually, it is given in a form of HTTP API end point. However, other forms may exist in case when ethereum node is launched locally. Unstoppable domains recommends [Cloudflare Ethereum Gateway](https://developers.cloudflare.com/distributed-web/ethereum-gateway) an ethereum node service provider.

Ethereum CHAIN ID is an ID of ethereum network a node is connected to. Each RPC provider can only be connected to one network. There is only one production network with CHAIN ID equal to `1` and called `mainnet`. Other networks are only used for testing purposes of different kinds. See [EIP-155](https://eips.ethereum.org/EIPS/eip-155) for more information. CHAIN ID of an ethereum node can be determined by calling [net version method](https://eth.wiki/json-rpc/API#net_version) on JSON RPC which should be used as a default when only JSON RPC provider is given.

Crypto Registry Contract Address is an actual address of a contract deployed. There is only one production registry address on the mainnet: [0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe](https://etherscan.io/address/0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe). This address should be used as a default for production configuration.

