# Deploying Custom Resolver

Custom Resolver is a way to implement a flexible permission model over domain records. This may include:

* Community owned domains - individual members of a community decide on a domain website content
* Domain leasing - a domain is temporary managed by a different authority and then comes back to original authority

Custom resolver can be implemented as a separated contract and its address can be assigned as a domain resolver via `Registry#resolveTo`.

Custom resolver must implement interfaces [IResolverReader](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IResolverReader.sol) and [ERC165](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-165.md)

