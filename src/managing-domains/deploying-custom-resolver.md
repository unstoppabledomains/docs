# Deploying custom resolver

Deploying a custom resolver is a way to implement a flexible permission model over domain records. This may include:

* **Community-owned domains** — where individual members of a community decide on a domain website content.
* **Domain leasing** — where a domain is temporarily managed by a different authority and then comes back to the original authority.

A custom resolver can be implemented as a separate contract and its address can be assigned as a domain resolver via `Registry#resolveTo`.

The custom resolver must implement interfaces [IResolverReader](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IResolverReader.sol) and [ERC165](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-165.md).

