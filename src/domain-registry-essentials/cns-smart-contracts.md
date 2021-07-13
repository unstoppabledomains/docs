# CNS smart contracts

Crypto Name Service \(CNS\) is built on Ethereum and is effectively a bundle of Solidity smart contracts. Their source code is hosted in the [dot-crypto repository](https://github.com/unstoppabledomains/dot-crypto) and maintained by the Unstoppable Domains team. This page lists the smart contracts from that repository and gives a brief description of each along with links to the source code and deployment addresses. For high-level details about how CNS works, see [Architecture overview](architecture-overview.md).

This page is divided into sections, grouping contracts by the following categories:

* [User-facing contracts](cns-smart-contracts.md#user-facing-contracts)
* [Registry controllers](cns-smart-contracts.md#registry-controllers)
* [Interfaces](cns-smart-contracts.md#interfaces)
* [Utility contracts](cns-smart-contracts.md#utility-contracts)
* [Test smart contracts](cns-smart-contracts.md#test-smart-contracts)

## User-facing contracts

This section lists all the smart contracts that users can directly interact with.

### Registry

`Registry` is the central smart contract, which stores all CNS domains. Implementing the ERC-721 non-fungible token standard, `Registry` defines domain ownership rules. It stores owner and `Resolver` addresses. For more details, see [Architecture overview - Registry](architecture-overview.md#registry).

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe](https://etherscan.io/address/0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe) |
| Rinkeby | [0xAad76bea7CFEc82927239415BB18D2e93518ecBB](https://rinkeby.etherscan.io/address/0xAad76bea7CFEc82927239415BB18D2e93518ecBB) |

**Source code:** [contracts/Registry.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Registry.sol)

### Resolver

`Resolver` is the smart contract that stores domain records and provides methods for domain resolution. For more details, see [Architecture overview - Resolver](architecture-overview.md#resolver).

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xb66DcE2DA6afAAa98F2013446dBCB0f4B0ab2842](https://etherscan.io/address/0xb66DcE2DA6afAAa98F2013446dBCB0f4B0ab2842) |
| Rinkeby | [0x95AE1515367aa64C462c71e87157771165B1287A](https://rinkeby.etherscan.io/address/0x95AE1515367aa64C462c71e87157771165B1287A) |

| Network | Legacy addresses |
| :--- | :--- |
| Mainnet | [0xa1cac442be6673c49f8e74ffc7c4fd746f3cbd0d](https://etherscan.io/address/0xa1cac442be6673c49f8e74ffc7c4fd746f3cbd0d) [0x878bc2f3f717766ab69c0a5f9a6144931e61aed3](https://etherscan.io/address/0x878bc2f3f717766ab69c0a5f9a6144931e61aed3) |

**Source code:** [contracts/Resolver.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Resolver.sol)

### ProxyReader

`ProxyReader` provides an interface that allows users to fetch information about domains from both `Registry` and `Resolver` smart contracts in one call. For more details, see [Architecture overview - ProxyReader](architecture-overview.md#proxyreader).

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xa6E7cEf2EDDEA66352Fd68E5915b60BDbb7309f5](https://etherscan.io/address/0xa6E7cEf2EDDEA66352Fd68E5915b60BDbb7309f5) |
| Rinkeby | [0x3A2e74CF832cbA3d77E72708d55370119E4323a6](https://rinkeby.etherscan.io/address/0x3A2e74CF832cbA3d77E72708d55370119E4323a6) |

| Network | Legacy addresses |
| :--- | :--- |
| Mainnet | [0x7ea9Ee21077F84339eDa9C80048ec6db678642B1](https://etherscan.io/address/0x7ea9Ee21077F84339eDa9C80048ec6db678642B1) |

**Source code:** [contracts/ProxyReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/ProxyReader.sol)

### SignatureController

`SignatureController` allows any account to submit management transactions on behalf of a token owner if an owner provides a signature for that call.

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0x82EF94294C95aD0930055f31e53A34509227c5f7](https://etherscan.io/address/0x82EF94294C95aD0930055f31e53A34509227c5f7) |
| Rinkeby | [0x66a5e3e2C27B4ce4F46BBd975270BE154748D164](https://rinkeby.etherscan.io/address/0x66a5e3e2C27B4ce4F46BBd975270BE154748D164) |

**Source code:** [contracts/controllers/SignatureController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/SignatureController.sol)

### DomainZoneController

`DomainZoneController` allows owners of a domain zone to mint subdomains. These subdomains can be managed only by the domain zone owners. For more details, see [Architecture Overview - Alternative Ownership Models](architecture-overview.md#alternative-ownership-models).

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xeA70777e28E00E81f58b8921fC47F78B8a72eFE7](https://etherscan.io/address/0xeA70777e28E00E81f58b8921fC47F78B8a72eFE7) |
| Rinkeby | [0x6f8F96A566663C1d4fEe70edD37E9b62Fe39dE5D](https://rinkeby.etherscan.io/address/0x6f8F96A566663C1d4fEe70edD37E9b62Fe39dE5D) |

**Source code:** [contracts/controllers/DomainZoneController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/DomainZoneController.sol)

### FreeMinter

`FreeMinter` is a contract that can be used for allowing any user to freely mint a test domain with `udtestdev-` prefix.

**Source code:** [contracts/controllers/FreeMinter.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/FreeMinter.sol)

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0x1fC985cAc641ED5846b631f96F35d9b48Bc3b834](https://etherscan.io/address/0x1fC985cAc641ED5846b631f96F35d9b48Bc3b834) |
| Rinkeby | [0x84214215904cDEbA9044ECf95F3eBF009185AAf4](https://rinkeby.etherscan.io/address/0x84214215904cDEbA9044ECf95F3eBF009185AAf4) |

### WhitelistedMinter

`WhitelistedMinter` defines an interface for minting second-level domains. This smart contract is primarily used by the Unstoppable Domains team, but its interface also supports delegating minting process to other parties via [Meta Transactions](../managing-domains/meta-transactions.md). All calls to `WhitelistedMinter` are proxied to the `Registry` via the [MintingController](cns-smart-contracts.md#mintingcontroller) smart contract.

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xd3fF3377b0ceade1303dAF9Db04068ef8a650757](https://etherscan.io/address/0xd3fF3377b0ceade1303dAF9Db04068ef8a650757) |
| Rinkeby | [0xbcB32f13f90978a9e059E8Cb40FaA9e6619d98e7](https://rinkeby.etherscan.io/address/0xbcB32f13f90978a9e059E8Cb40FaA9e6619d98e7) |

**Source code:** [contracts/util/WhitelistedMinter.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/WhitelistedMinter.sol)

### TwitterValidationOperator

`TwitterValidationOperator` is used when initiating Chainlink verification requests to link domain records with Twitter usernames.

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xbb486C6E9cF1faA86a6E3eAAFE2e5665C0507855](https://etherscan.io/address/0xbb486C6E9cF1faA86a6E3eAAFE2e5665C0507855) |
| Rinkeby | [0x1CB337b3b208dc29a6AcE8d11Bb591b66c5Dd83d](https://rinkeby.etherscan.io/address/0x1CB337b3b208dc29a6AcE8d11Bb591b66c5Dd83d) |

**Source code:** [contracts/operators/TwitterValidationOperator.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/operators/TwitterValidationOperator.sol)

## Registry controllers

The Unstoppable Domains team reserves the right to mint second-level domains and edit some `Registry` settings, such as token URI prefix. To avoid giving anyone absolute admin rights, `Registry` utilizes controllers that implement a limited set of admin actions.

### MintingController

The deployed version of the `Registry` smart contract only allows `MintingController` to mint second-level domains. This smart contract is used by [WhitelistedMinter](cns-smart-contracts.md#whitelistedminter) as a proxy.

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0xb0EE56339C3253361730F50c08d3d7817ecD60Ca](https://etherscan.io/address/0xb0EE56339C3253361730F50c08d3d7817ecD60Ca) |
| Rinkeby | [0x51765307AeB3Df2E647014a2C501d5324212467c](https://rinkeby.etherscan.io/address/0x51765307AeB3Df2E647014a2C501d5324212467c) |

**Source code:** [contracts/controllers/MintingController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/MintingController.sol)

### URIPrefixController

`URIPrefixController` enables the Unstoppable Domains team to edit the token URI prefix.

| Network | Contract address |
| :--- | :--- |
| Mainnet | [0x09B091492759737C03da9dB7eDF1CD6BCC3A9d91](https://etherscan.io/address/0x09B091492759737C03da9dB7eDF1CD6BCC3A9d91) |
| Rinkeby | [0xe1d2e4B9f0518CA5c803073C3dFa886470627237](https://rinkeby.etherscan.io/address/0xe1d2e4B9f0518CA5c803073C3dFa886470627237) |

**Source code:** [contracts/controllers/URIPrefixController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/URIPrefixController.sol)

## Interfaces

The following interfaces can be used as guidelines for the minimal implementation of custom smart contract versions. Also, Solidity developers can rely on them for making calls to the official CNS smart contracts.

### IRegistry

The `IRegistry` interface declares all the `Registry` events and methods \(both read and write\).

**Source code:** [contracts/IRegistry.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IRegistry.sol)

**Implemented by:**

* [Registry](cns-smart-contracts.md#registry)

### IRegistryReader

The `IRegistryReader` interface declares only read-only `Registry` methods.

**Source code:** [contracts/IRegistryReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IRegistryReader.sol)

**Implemented by:**

* [Registry](cns-smart-contracts.md#registry)
* [ProxyReader](cns-smart-contracts.md#proxyreader)

### IResolver

The `IResolver` interface declares the minimal set of `Resolver` methods for configuring domain records.

**Source code:** [contracts/IResolver.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IResolver.sol)

**Implemented by:**

* [Resolver](cns-smart-contracts.md#resolver)

### IResolverReader

The `IResolverReader` interface declares the set of methods for reading `Resolver` records.

**Source code:** [contracts/IResolverReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IResolverReader.sol)

**Implemented by:**

* [Resolver](cns-smart-contracts.md#resolver)
* [ProxyReader](cns-smart-contracts.md#proxyreader)

### IDataReader

The `IDataReader` interface declares the methods that are unique to the `ProxyReader` smart contract, which returns combined data from the `Registry` and `Resolver` contracts.

**Source code:** [contracts/IDataReader.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IDataReader.sol)

**Implemented by:**

* [ProxyReader](cns-smart-contracts.md#proxyreader)

### IMintingController

The `IMintingController` interface declares a set of methods for minting, which both `MintingController` and `WhitelistedMinter` implement.

**Source code:** [contracts/controllers/IMintingController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/IMintingController.sol)

**Implemented by:**

* [MintingController](cns-smart-contracts.md#mintingcontroller)
* [WhitelistedMinter](cns-smart-contracts.md#whitelistedminter)

### ISignatureController

The `ISignatureController` interface declares the functions that are implemented by `SignatureController` to enable [Meta transactions](../managing-domains/meta-transactions.md) for the `Registry` smart contract.

**Source code:** [contracts/controllers/ISignatureController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/ISignatureController.sol)

**Implemented by:**

* [SignatureController](cns-smart-contracts.md#signaturecontroller)

### IURIPrefixController

The `IURIPrefixController` interface declares the functions that are implemented by `URIPrefixController`.

**Source code:** [contracts/controllers/IURIPrefixController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/controllers/IURIPrefixController.sol)

**Implemented by:**

* [URIPrefixController](cns-smart-contracts.md#uriprefixcontroller)

### ERC677Receiver

The `ERC677Receiver` interface declares an ERC-677 method for receiving smart contracts.

**Source code:** [contracts/util/ERC677Receiver.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/ERC677Receiver.sol)

**Implemented by:**

* [TwitterValidationOperator.sol](cns-smart-contracts.md#twittervalidationoperator)

## Utility contracts

Utility contracts are generally used to share common functionality between other smart contracts. This list also includes some contracts that are used internally by the Unstoppable Domains team.

### BulkWhitelistedRole

`BulkWhitelistedRole` is an extension of Open Zeppelin's `WhitelistedRole` that adds bulk operations for adding and removing whitelisted accounts.

**Source code:** [contracts/util/BulkWhitelistedRole.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/BulkWhitelistedRole.sol)

**Used by:**

* [WhitelistedMinter](cns-smart-contracts.md#whitelistedminter)

### ControllerRole

The `ControllerRole` smart contract defines an Open Zeppelin [Role](https://docs.openzeppelin.com/contracts/2.x/access-control#using-roles), which is used by `Registry` to designate controllers.

**Source code:** [contracts/util/ControllerRole.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/ControllerRole.sol)

**Used by:**

* [Registry](cns-smart-contracts.md#registry)

### MultiSend

The `MultiSend` smart contract is used internally by the Unstoppable Domains team to fund worker accounts.

**Source code:** [contracts/util/MultiSend.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/MultiSend.sol)

### SignatureUtil

`SignatureUtil` is a helper smart contract. Its implementation is used to extend smart contracts that require [Meta Transactions](../managing-domains/meta-transactions.md).

**Source code:** [contracts/util/SignatureUtil.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/util/SignatureUtil.sol)

**Used by:**

* [Resolver](cns-smart-contracts.md#resolver)
* [SignatureController](cns-smart-contracts.md#signaturecontroller)

### Migrations

The [Truffle migrations](https://www.trufflesuite.com/docs/truffle/getting-started/running-migrations) smart contract.

**Source code:** [contracts/Migrations.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Migrations.sol)

## Test smart contracts

There are several smart contracts that are used for testing purposes without being deployed to public networks or imported by other smart contracts.

### LinkTokenMock

The `LinkTokenMock` smart contract is used for testing [TwitterValidationOperator](cns-smart-contracts.md#twittervalidationoperator).

**Source code:** [contracts/test-helpers/LinkTokenMock.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/test-helpers/LinkTokenMock.sol)

### RelayTest

`RelayTest` is used for testing relaying functionality for [Meta Transactions](../managing-domains/meta-transactions.md).

**Source code:** [contracts/test-helpers/RelayTest.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/test-helpers/RelayTest.sol)

### Simple

The `Simple` smart contract is used for testing ERC-721 receiver validation checks.

**Source code:** [contracts/test-helpers/Simple.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/test-helpers/Simple.sol)

