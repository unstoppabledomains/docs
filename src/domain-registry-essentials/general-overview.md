---
description: >-
  This section explains the components of crypto registry, its core design
  principles and permission model.
---

# General Overview

The essential part of the registry is to allow one to own a domain and associate records to it.

Domain ownership is held in a form of [ERC721 token](https://eips.ethereum.org/EIPS/eip-721). A domain name is converted to an ERC721 token using a [Namehashing ](namehashing.md)algorithm. The records have a key-value form. **Multiple records with the same key are unsupported** at the low level and have to be simulated in higher level. See [Records Reference](reference.md). An attempt to add a record that already exist on resolver will result in record value being overwritten.

In addition to this, the registry design has a capability for flexible records managements that allows to implement any records management permission model. The flexibility is achieved by introducing a Resolver contract as a separated contract that can hold records and associating a domain with a single resolver contract address. Records can be associated to a domain ONLY via a Resolver contract. A single resolver can hold records for multiple domains.

A fragment of Registry contract that shows how ownership and resolvers information is persisted:

```text
// Mapping from ERC721 token ID to a resolver address
mapping (uint256 => address) internal _tokenResolvers;
// Maping from ERC721 token ID to an owner address
// Part of ERC721 standard
mapping (uint256 => address) internal _tokenOwners;
```

There are other permission structures \(approved address and operators\) available as part of ERC721 standard but they do not have any custom functionality on top. See the ERC721 standard for more information on those topics.

Registry is a "singleton" contract and only exists in a single production instance deployed at [0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe](https://etherscan.io/address/0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe). [Source Code](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Registry.sol).

Resolver data structure looks in the following way \(pseudocode\):

```text
//Mapping of ERC721 token ID to key-value records mapping
mapping (uint256 =>  mapping (string => string)) internal _records;
```

Unstoppable Domains provides a default public resolver contract deployed at [0xb66DcE2DA6afAAa98F2013446dBCB0f4B0ab2842](https://etherscan.io/address/0xb66DcE2DA6afAAa98F2013446dBCB0f4B0ab2842). [Source Code](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Resolver.sol).

### Registry Controllers

At the moment when crypto registry was deployed, Ethereum platform had a limitation on the contract size. [Removing Contract Size Limit Issue](https://github.com/ethereum/EIPs/issues/1662).

In order to avoid that limitation, some registry methods are moved to Controller Contracts. A method sent to controller contract will act as if it was sent to original registry. The list of controllers addresses is **irreversibly locked** on the Registry and can not be modified in the future.

A list of controller contracts and their source can be found in [List of Contracts](https://github.com/unstoppabledomains/dot-crypto/blob/master/README.md#deployed-contracts)

### Domains Minting and Hierarchy

Registry comes with a pre-generated top level domain `crypto`. A process of making a new domain is referenced as "minting" in the source code and documentation. Generally any domain owner can mint a subdomain via [Registry\#mintChild](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Registry.sol#L79).

Example: an owner of domain `example.crypto` can mint a domain `home.example.crypto` in the following way:

```text
mintChild(subdomainOwner, namehash('example.crypto'), 'home')
```

Note: `subdomainOwner` usually matches the owner of the original domain.

Parent domain owners are having a full control over child domains by having an ability to transfer the ownership of subdomain via [Registry\#transferFromChild](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Registry.sol#L111).

Example: transferring a subdomain `home.example.crypto` from previous example to original owner can look like:

```text
transferFromChild(subdomainOwner, sldOwner, namehash('example.crypto'), 'home')
```

`crypto` top level domain's owner is set to `0x0000000000000000000000000000000000000000000000000000000000000000` address that no one owns. So, the second level domains minting is done via a [MintingController.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/MintingController.sol) which is only allowed to mint non existing domains without any ability to control those domains after they are minted.

The permanent ownership of second level domains is guaranteed as there is no fee for owning a domain and no permission to revoke the ownership at higher level. People wanting to propagate this permission model to subdomains \(e.g. converting them into zones\) can follow the same pattern for domains they own.

There is no technical limit of how deep subdomains tree can go. There might be limitations on the client side, but they are not recommended.

