# Managing domain ownership

Management of second-level domains is performed via the [Registry smart contract](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Registry.sol). The smart contract is built upon [OpenZeppelin's implementation](https://docs.openzeppelin.com/contracts/2.x/api/token/erc721#ERC721Burnable) of the [ERC-721](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-721.md) token standard.

Entities that can control domains are defined by the ERC-721 standard:

* **Owner.** This is a direct owner of a domain, which has full control in managing domain ownership and records.
* **Operator.** Registry allows any user to set their operators, that can control all domains owned by a user. There can be multiple operators per user.
* **Approved address.** A domain owner can set an approved address, that can control one particular domain. ERC-721 allows only one approved address per token \(domain\).

There are five basic operations that affect domain ownership:

* **Minting.** When a domain is first created, an initial domain owner is assigned. Minting domains is a separate topic on its own and won't be covered in this article.
* **Transferring.** There are two possible ways of transferring a domain: the one that keeps resolution settings, and the one that resets them.
* **Setting an operator.** Registry allows setting one operator per domain which has equal privileges with a domain owner.
* **Setting an approved address.** This operation allows other Ethereum address to control all domains owned by a caller.
* **Burning.** Burns a domain, clearing all associated metadata and Resolver settings.

This article covers all the Registry methods that can be used for managing domain ownership.

## Transferring

Methods that change a direct owner of a domain can be called by either a domain owner, an operator or an approved address.

Registry smart contract supports the following ERC-721 functions for transferring:

```text
transferFrom(address from, address to, uint256 tokenId)

safeTransferFrom(address from, address to, uint256 tokenId)

safeTransferFrom(address from, address to, uint256 tokenId, bytes _data)
```

If one of these methods is called, both an approved operator and a Resolution address for a domain get reset.

{% hint style="info" %}
**Note:** the current implementation of transferring only resets a Resolver address, but doesn't reset records stored by a Resolver smart contract. It means that after setting a new Resolver address for a transferred domain, if the Resolver address matches the previous one, a new domain owner will get Resolution settings of a previous owner.

After receiving a domain, along with setting a Resolver address, [`reconfigure`](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Resolver.sol) method should be called, which resets all previous records.
{% endhint %}

Registry smart contract also implements `setOwner` function, which is not a part of the ERC-721 standard:

```text
setOwner(address to, uint256 tokenId)
```

`setOwner` keeps a Resolver address and resets an approved operator. This method makes it possible to preconfigure a domain with certain records and transfer it to another owner, keeping all resolution settings.

## Setting an operator

Any Ethereum address can set multiple operators, allowing them to manage domains that a caller owns directly. This is an operation defined by ERC-721:

```text
setApprovalForAll(address to, bool approved)
```

## Setting an approved address

An approved address can be set by either a domain owner or an operator. This method is defined by ERC-721 as well:

```text
approve(address to, uint256 tokenId)
```

Approved addresses have equal rights as domain owners and operators, being able to both transfer ownership and manage resolution.

## Burning

Registry smart contract supports "burning" operation. After burning, a domain becomes available for minting again.

```text
burn(tokenId)
```

