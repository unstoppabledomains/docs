# Resolving Domain Records

Resolving a domain is a process of retrieving a domain's records when the domain name and required record names are given. There are no limits to who can read domain records on the `Registry` side. Anyone with access to a mainnet Ethereum Node can resolve a domain.

This section describes resolving domain records by making calls to Ethereum smart contracts using the Ethereum JSON RPC. For developers who would prefer a more straightforward solution, it might be more convenient to use the [resolution libraries](https://github.com/unstoppabledomains?q=resolution) that Unstoppable Domains maintains.

To resolve a domain, your software must have access to the Ethereum network. For more information, see [Configuring an ethereum network connection](#configuring-an-ethereum-network-connection).

The simplest way to resolve a domain with Ethereum JSON RPC is to make a read-only call to `ProxyReader` smart contract. `ProxyReader` provides an API that allows users to resolve domains making just one call by passing only keys of records and a domain namehash. Without `ProxyReader` it would require executing at least two calls: one to obtain a domain resolver address and another one to get the records themselves. With `ProxyReader` it all happens under the hood.

An example in JavaScript of getting two records (using [ethers library](https://www.npmjs.com/package/ethers)):

```javascript
const proxyReaderAddress = "0x7ea9Ee21077F84339eDa9C80048ec6db678642B1";
// Partial ABI, just for the getMany function.
const proxyReaderAbi = [
  "function getMany(string[] calldata keys, uint256 tokenId) external view returns (string[] memory)",
];
const proxyReaderContract = new ethers.Contract(
  proxyReaderAddress,
  proxyReaderAbi,
  provider
);

const domain = "brad.crypto";
const tokenId = namehash(domain);
const keys = ["crypto.ETH.address", "crypto.BTC.address"];

const values = await proxyReaderContract.getMany(keys, tokenId);
console.log(values);
// [
//   '0x8aaD44321A86b170879d7A244c1e8d360c99DdA8',
//   'bc1q359khn0phg58xgezyqsuuaha28zkwx047c0c3y'
// ]
```

Reference:

- `namehash` - namehashing algorithm implementation. See [Namehashing](namehashing.md).

![](../.gitbook/assets/provide_domain_records_via_proxy_reader_smart_contract.png)

See [Records reference](records-reference.md) for more information about the standardized records.

## Record value validation

`Resolver` doesn't have built-in record value validation when it is updated for two reasons:

- Any validation would require additional gas to be paid
- Solidity is a special-purpose programming language that doesn't have built-in data validation tools like Regular Expressions

Any domain management application should perform record format validation before submitting a transaction. However, there is no guarantee that all management applications will do it correctly. For this reason, records should be validated when the domain is resolved too.

See [Records Reference](records-reference.md) for more information for the validator of each record.

## Configuring an Ethereum network connection

Domain resolution configuration at a low level requires 3 configuration parameters:

- Ethereum JSON RPC provider
- Ethereum CHAIN ID
- Crypto Registry Contract Address

Ethereum JSON RPC provider is an API implementing the Ethereum JSON RPC standard. Usually, it is given in a form of an HTTP API endpoint. However, other forms may exist if the Ethereum node is launched locally. Unstoppable Domains recommends the [Cloudflare Ethereum Gateway](https://developers.cloudflare.com/distributed-web/ethereum-gateway), an Ethereum node service provider. To learn more about providers, see [Nodes and client](https://ethereum.org/en/developers/docs/nodes-and-clients/) and [Nodes as a service](https://ethereum.org/en/developers/docs/nodes-and-clients/nodes-as-a-service/)

Ethereum CHAIN ID is an ID of the Ethereum network a node is connected to. Each RPC provider can only be connected to one network. There is only one production network with CHAIN ID equal to `1` and called `mainnet`. Other networks are only used for testing purposes. See [EIP-155](https://eips.ethereum.org/EIPS/eip-155) for more information. CHAIN ID of an Ethereum node can be determined by calling the [net version method](https://eth.wiki/json-rpc/API#net_version) on JSON RPC which should be used as a default when only JSON RPC provider is given.

Crypto Registry Contract Address is an actual address of a contract deployed. There is only one production registry address on the mainnet: [0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe](https://etherscan.io/address/0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe). This address should be used as a default for production configuration.
