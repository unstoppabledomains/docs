# CNS smart contracts

## User-facing contracts

### Registry

Registry is the central smart contract, which stores all CNS domains. Implementing ERC-721 non-fungible token standard, it defines ownership rules. It stores owner and Resolver addresses. For more details, see [Architecture overview - Registry](./architecture-overview.md#registry).

**Source code:** [contracts/Registry.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Registry.sol)

| Network | Contract address |
| Mainnet | [0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe](https://etherscan.io/address/0xd1e5b0ff1287aa9f9a268759062e4ab08b9dacbe) |

### Resolver

Resolver is the smart contract that stores domain records and provides methods for domain resolution. For more details, see [Architecture overview - Resolver](./architecture-overview.md#resolver). 

**Source code:** [contracts/Resolver.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Resolver.sol)

| Network | Contract address |
| Mainnet | [0xb66DcE2DA6afAAa98F2013446dBCB0f4B0ab2842](https://etherscan.io/address/0xb66DcE2DA6afAAa98F2013446dBCB0f4B0ab2842) |

**Legacy Mainnet Resolvers:**
- [0xa1cac442be6673c49f8e74ffc7c4fd746f3cbd0d](https://etherscan.io/address/0xa1cac442be6673c49f8e74ffc7c4fd746f3cbd0d)
- [0x878bc2f3f717766ab69c0a5f9a6144931e61aed3](https://etherscan.io/address/0x878bc2f3f717766ab69c0a5f9a6144931e61aed3)

### ProxyReader

ProxyReader provides an interface, that allows users to fetch information about domains from both Registry and Resolver smart contracts in one call. For more details, see [Architecture overview - RroxyReader](./architecture-overview.md#proxyreader).

**Source code:** [contracts/ProxyReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/ProxyReader.sol)

| Network | Contract address |
| Mainnet | [0x7ea9Ee21077F84339eDa9C80048ec6db678642B1](https://etherscan.io/address/0x7ea9Ee21077F84339eDa9C80048ec6db678642B1) |

### SignatureController

SignatureController allows any account to submit management transactions on behalf of a token owner, if an owner provides a signature for such a call.

**Source code:** [contracts/controllers/SignatureController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/SignatureController.sol)

| Network | Contract address |
| Mainnet | [0x82EF94294C95aD0930055f31e53A34509227c5f7](https://etherscan.io/address/0x82EF94294C95aD0930055f31e53A34509227c5f7) |

### DomainZoneController

DomainZoneController provides the functionality that allows owners of a domain zone to mint subdomains, that can be managed only by domain zone owners. For more details, see [Architecture overview - Alternative ownership models](./architecture-overview.md#alternative-ownership-models).

**Source code:** [contracts/controllers/DomainZoneController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/DomainZoneController.sol)

| Network | Contract address |
| Mainnet | [0xeA70777e28E00E81f58b8921fC47F78B8a72eFE7](https://etherscan.io/address/0xeA70777e28E00E81f58b8921fC47F78B8a72eFE7) |

### FreeRegistrar

FreeRegistrar is a contract that can be used for allowing any user to freely mint a domain belonging to a subdomain zone.

**Source code:** [contracts/controllers/FreeRegistrar.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/FreeRegistrar.sol)

### WhitelistedMinter

WhitelistedMinter defines an interface for minting second-level domains. This smart contract is primarily used by Unstoppable Domains team, but its intefrace also supports delegating minting process to other parties via [Meta transactions](../managing-domains/meta-transactions.md). All calls to WhitelistedMinter get proxied to the Registry via [MintingController](./cns-smart-contracts.md#mintingcontroller) smart contract.

**Source code:** [contracts/util/WhitelistedMinter.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/WhitelistedMinter.sol)

| Network | Contract address |
| Mainnet | [0xd3fF3377b0ceade1303dAF9Db04068ef8a650757](https://etherscan.io/address/0xd3fF3377b0ceade1303dAF9Db04068ef8a650757) |

## Registry controllers

Unstoppable Domains team reserves the right to mint second-level domains and edit some Registry settings, such as token URI prefix. To avoid giving anyone absolute admin rights, CNS Registry utilizes controllers, that implement a limited set of admin actions.

### MintingController

MintingController is a smart contract that is allowed to mint second-level domains. The deployed version of the Registry smart contract allows only MintingController to mint domains. This smart contract is used by [WhitelistedMinter](./cns-smart-contracts.md#whitelistedminter) as a proxy.

**Source code:** [contracts/controllers/MintingController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/MintingController.sol)

| Network | Contract address |
| Mainnet | [0xb0EE56339C3253361730F50c08d3d7817ecD60Ca](https://etherscan.io/address/0xb0EE56339C3253361730F50c08d3d7817ecD60Ca) |

### URIPrefixController

URIPrefixController enables Unstoppable Domains team to edit the token URI prefix.

**Source code:** [contracts/controllers/URIPrefixController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/URIPrefixController.sol)

| Network | Contract address |
| Mainnet | [0x09B091492759737C03da9dB7eDF1CD6BCC3A9d91](https://etherscan.io/address/0x09B091492759737C03da9dB7eDF1CD6BCC3A9d91) |

## Interfaces

The following interfaces can be used as a guidance to the minimal implementation of custom smart contracts versions. Also, Solidity developers can rely on them for making calls to the official CNS smart contracts.

### IRegistry

IRegistry interface declares all the Registry events and methods (both read and write ones).

**Source code:** [contracts/IRegistry.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IRegistry.sol)<br/>
**Implemented by:**
- [Registry](./cns-smart-contracts.md#registry)

### IRegistryReader

IRegistryReader interface declares only read-only Registry methods.

**Source code:** [contracts/IRegistryReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IRegistryReader.sol)<br/>
**Implemented by:**
- [Registry](./cns-smart-contracts.md#registry)
- [ProxyReader](./cns-smart-contracts.md#proxyreader)

### IResolver

IResolver interface declares the minimal set of Resolver methods that are used for configuring domain records.

**Source code:** [contracts/IResolver.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IResolver.sol)<br/>
**Implemented by:**
- [Resolver](./cns-smart-contracts.md#resolver)

### IResolverReader

IResolverReader interface declares the set of Resolver methods that can are used for reading Resolver records.

**Source code:** [contracts/IResolverReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IResolverReader.sol)<br/>
**Implemented by:**
- [Resolver](./cns-smart-contracts.md#resolver)
- [ProxyReader](./cns-smart-contracts.md#proxyreader)

### IDataReader

IDataReader interface declares the methods that are unique to the ProxyReader smart contract, which return combined data from the Registry and Resolver contracts.

**Source code:** [contracts/IDataReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IDataReader.sol)<br/>
**Implemented by:**
- [ProxyReader](./cns-smart-contracts.md#proxyreader)

## Other

### LinkTokenMock ?

### BulkWhitelistedRole +

### ControllerRole +

### ERC677Receiver ?

### MultiSend ?

### RelayTest ?

### SignatureUtil +

### Simple ?

### Migrations ?
