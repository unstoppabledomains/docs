# CNS smart contracts

CNS is built on Ethereum and comprises a bundle of Solidity smart contracts. Their source code is hosted in the [dot-crypto repository](https://github.com/unstoppabledomains/dot-crypto) and maintained by the Unstoppable Domains team. This article lists all the smart contracts from the repository and gives a brief description of each of them, along with the links to the source code and deployment addresses. If you are interested in high-level details about how CNS works, see [Architecture overview](./architecture-overview.md).

This page is divided into sections, grouping contracts by the following categories:
* [User-facing contracts](./cns-smart-contracts.md#user-facing-contracts)
* [Registry controllers](./cns-smart-contracts.md#registry-controllers)
* [Interfaces](./cns-smart-contracts.md#interfaces)
* [Utility contracts](./cns-smart-contracts.md#utility-contracts)
* [Test smart contracts](./cns-smart-contracts.md#test-smart-contracts)

## User-facing contracts

This section lists all the smart contracts that users can directly interact with.

### Registry

Registry is the central smart contract, which stores all CNS domains. Implementing ERC-721 non-fungible token standard, it defines ownership rules. It stores owner and Resolver addresses. For more details, see [Architecture overview - Registry](./architecture-overview.md#registry).

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe](https://etherscan.io/address/0xd1e5b0ff1287aa9f9a268759062e4ab08b9dacbe) |

**Source code:** [contracts/Registry.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Registry.sol)

### Resolver

Resolver is the smart contract that stores domain records and provides methods for domain resolution. For more details, see [Architecture overview - Resolver](./architecture-overview.md#resolver).

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xb66DcE2DA6afAAa98F2013446dBCB0f4B0ab2842](https://etherscan.io/address/0xb66DcE2DA6afAAa98F2013446dBCB0f4B0ab2842) |

**Legacy Mainnet Resolvers:**
- [0xa1cac442be6673c49f8e74ffc7c4fd746f3cbd0d](https://etherscan.io/address/0xa1cac442be6673c49f8e74ffc7c4fd746f3cbd0d)
- [0x878bc2f3f717766ab69c0a5f9a6144931e61aed3](https://etherscan.io/address/0x878bc2f3f717766ab69c0a5f9a6144931e61aed3)

**Source code:** [contracts/Resolver.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Resolver.sol)

### ProxyReader

ProxyReader provides an interface, that allows users to fetch information about domains from both Registry and Resolver smart contracts in one call. For more details, see [Architecture overview - RroxyReader](./architecture-overview.md#proxyreader).

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0x7ea9Ee21077F84339eDa9C80048ec6db678642B1](https://etherscan.io/address/0x7ea9Ee21077F84339eDa9C80048ec6db678642B1) |

**Source code:** [contracts/ProxyReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/ProxyReader.sol)

### SignatureController

SignatureController allows any account to submit management transactions on behalf of a token owner if an owner provides a signature for such a call.

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0x82EF94294C95aD0930055f31e53A34509227c5f7](https://etherscan.io/address/0x82EF94294C95aD0930055f31e53A34509227c5f7) |

**Source code:** [contracts/controllers/SignatureController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/SignatureController.sol)

### DomainZoneController

DomainZoneController provides the functionality that allows owners of a domain zone to mint subdomains, that can be managed only by domain zone owners. For more details, see [Architecture overview - Alternative ownership models](./architecture-overview.md#alternative-ownership-models).

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xeA70777e28E00E81f58b8921fC47F78B8a72eFE7](https://etherscan.io/address/0xeA70777e28E00E81f58b8921fC47F78B8a72eFE7) |

**Source code:** [contracts/controllers/DomainZoneController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/DomainZoneController.sol)

### FreeRegistrar

FreeRegistrar is a contract that can be used for allowing any user to freely mint a domain belonging to a subdomain zone.

**Source code:** [contracts/controllers/FreeRegistrar.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/FreeRegistrar.sol)

### WhitelistedMinter

WhitelistedMinter defines an interface for minting second-level domains. This smart contract is primarily used by the Unstoppable Domains team, but its interface also supports delegating minting process to other parties via [Meta transactions](../managing-domains/meta-transactions.md). All calls to WhitelistedMinter get proxied to the Registry via the [MintingController](./cns-smart-contracts.md#mintingcontroller) smart contract.

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xd3fF3377b0ceade1303dAF9Db04068ef8a650757](https://etherscan.io/address/0xd3fF3377b0ceade1303dAF9Db04068ef8a650757) |

**Source code:** [contracts/util/WhitelistedMinter.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/WhitelistedMinter.sol)

### TwitterValidationOperator

TwitterValidationOperator contract is used for initiating Chainlink requests, that validate Twitter usernames.

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xbb486C6E9cF1faA86a6E3eAAFE2e5665C0507855](https://etherscan.io/address/0xbb486C6E9cF1faA86a6E3eAAFE2e5665C0507855) |

**Source code:** [contracts/operators/TwitterValidationOperator.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/operators/TwitterValidationOperator.sol)

## Registry controllers

The Unstoppable Domains team reserves the right to mint second-level domains and edit some Registry settings, such as token URI prefix. To avoid giving anyone absolute admin rights, CNS Registry utilizes controllers, that implement a limited set of admin actions.

### MintingController

The deployed version of the Registry smart contract allows only MintingController to mint second-level domains. This smart contract is used by [WhitelistedMinter](./cns-smart-contracts.md#whitelistedminter) as a proxy.

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xb0EE56339C3253361730F50c08d3d7817ecD60Ca](https://etherscan.io/address/0xb0EE56339C3253361730F50c08d3d7817ecD60Ca) |

**Source code:** [contracts/controllers/MintingController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/MintingController.sol)

### URIPrefixController

URIPrefixController enables the Unstoppable Domains team to edit the token URI prefix.

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0x09B091492759737C03da9dB7eDF1CD6BCC3A9d91](https://etherscan.io/address/0x09B091492759737C03da9dB7eDF1CD6BCC3A9d91) |

**Source code:** [contracts/controllers/URIPrefixController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/URIPrefixController.sol)

## Interfaces

The following interfaces can be used as a guidance to the minimal implementation of custom smart contracts versions. Also, Solidity developers can rely on them for making calls to the official CNS smart contracts.

### IRegistry

IRegistry interface declares all the Registry events and methods (both read and write ones).

**Source code:** [contracts/IRegistry.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IRegistry.sol)

**Implemented by:**
* [Registry](./cns-smart-contracts.md#registry)

### IRegistryReader

IRegistryReader interface declares only read-only Registry methods.

**Source code:** [contracts/IRegistryReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IRegistryReader.sol)

**Implemented by:**
* [Registry](./cns-smart-contracts.md#registry)
* [ProxyReader](./cns-smart-contracts.md#proxyreader)

### IResolver

IResolver interface declares the minimal set of Resolver methods that are used for configuring domain records.

**Source code:** [contracts/IResolver.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IResolver.sol)

**Implemented by:**
* [Resolver](./cns-smart-contracts.md#resolver)

### IResolverReader

IResolverReader interface declares the set of Resolver methods that can are used for reading Resolver records.

**Source code:** [contracts/IResolverReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IResolverReader.sol)

**Implemented by:**
* [Resolver](./cns-smart-contracts.md#resolver)
* [ProxyReader](./cns-smart-contracts.md#proxyreader)

### IDataReader

IDataReader interface declares the methods that are unique to the ProxyReader smart contract, which returns combined data from the Registry and Resolver contracts.

**Source code:** [contracts/IDataReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IDataReader.sol)

**Implemented by:**
* [ProxyReader](./cns-smart-contracts.md#proxyreader)

### IMintingController

IMintingController interface declares a set of methods for minting, that both MintingController and WhitelistedMinter implement.

**Source code:** [contracts/controllers/IMintingController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/IMintingController.sol)

**Implemented by:**
* [MintingController](./cns-smart-contracts.md#mintingcontroller)
* [WhitelistedMinter](./cns-smart-contracts.md#whitelistedminter)

### ISignatureController

ISignatureController interface declares the functions that are implemented by SignatureController to enable [Meta transactions](../managing-domains/meta-transactions.md) for the Registry smart contract.

**Source code:** [contracts/controllers/ISignatureController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/ISignatureController.sol)

**Implemented by:**
* [SignatureController](./cns-smart-contracts.md#signaturecontroller)

### IURIPrefixController

IURIPrefixController interface declares the functions that are implemented by URIPrefixController.

**Source code:** [contracts/controllers/IURIPrefixController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/IURIPrefixController.sol)

**Implemented by:**
* [URIPrefixController](./cns-smart-contracts.md#uriprefixcontroller)

### ERC677Receiver

ERC677Receiver interface declares an ERC-677 method for receiving smart contracts.

**Source code:** [contracts/util/ERC677Receiver.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/ERC677Receiver.sol)

**Implemented by:**
* [TwitterValidationOperator.sol](./cns-smart-contracts.md#twittervalidationoperator)

## Utility contracts

The utility contracts are generally used for sharing common functionality between other smart contracts. The list also includes some contracts that are used internally by the Unstoppable Domains team.

### BulkWhitelistedRole

BulkWhitelistedRole is an extension of Open Zeppelin's WhitelistedRole, that adds bulk operations for adding and removing whitelisted accounts.

**Source code:** [contracts/util/BulkWhitelistedRole.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/BulkWhitelistedRole.sol)

**Used by:**
* [WhitelistedMinter](./cns-smart-contracts.md#whitelistedminter)

### ControllerRole

The ControllerRole smart contract defines an Open Zeppelin's [Role](https://docs.openzeppelin.com/contracts/2.x/access-control#using-roles), which is used by the Registry to designate controllers.

**Source code:** [contracts/util/ControllerRole.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/ControllerRole.sol)

**Used by:**
* [Registry](./cns-smart-contracts.md#registry)

### MultiSend

The MultiSend smart contract is used internally by the Unstoppable Domains team to fund worker accounts.

**Source code:** [contracts/util/MultiSend.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/MultiSend.sol)

### SignatureUtil

SignatureUtil is a helper smart contract. Its implementation is used to extend smart contracts that require [Meta transactions](../managing-domains/meta-transactions.md) functionality.

**Source code:** [contracts/util/SignatureUtil.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/SignatureUtil.sol)

**Used by:**
* [Resolver](./cns-smart-contracts.md#resolver)
* [SignatureController](./cns-smart-contracts.md#signaturecontroller)

### Migrations

The [Truffle migrations](https://www.trufflesuite.com/docs/truffle/getting-started/running-migrations) smart contract.

**Source code:** [contracts/Migrations.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Migrations.sol)

## Test smart contracts

There are several smart contracts that are used for testing purposes, without being deployed to public networks or imported by other smart contracts.

### LinkTokenMock

The LinkTokenMock smart contract is used for testing [TwitterValidationOperator](./cns-smart-contracts.md#twittervalidationoperator).

**Source code:** [contracts/test-helpers/LinkTokenMock.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/test-helpers/LinkTokenMock.sol)

### RelayTest

RelayTest is used for testing relaying functionality for [Meta transactions](../managing-domains/meta-transactions.md).

**Source code:** [contracts/test-helpers/RelayTest.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/test-helpers/RelayTest.sol)

### Simple

The Simple smart contract is used for testing the validations that check smart contracts for being valid ERC-721 receivers.

**Source code:** [contracts/test-helpers/Simple.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/test-helpers/Simple.sol)
