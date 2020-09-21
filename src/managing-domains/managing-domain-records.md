# Managing Domain Records

Domain records can be managed via the default public resolver, or one can develop a custom resolver with any management permissions defined.

## Using The Default Public Resolver

The default public resolver allows you to manage all domain records for any address, given permission over the domain as per the [ERC721 "Transfer Mechanism"](https://eips.ethereum.org/EIPS/eip-721) section. These includes:

* Owner address of a domain
* Approved address for a domain
* Owner's operator addresses

See ERC721 on how those permissions can be granted and revoked. Any records change is submitted as an [Ethereum Blockchain Transaction](https://ethereum.org/en/whitepaper/#messages-and-transactions).

Records Management can be done via [Resolver methods](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IResolver.sol).

## Main Records

Records on the top level are stored in a simple key-value mapping of a string to string. The Crypto registry doesn't forbid a user to assign any record to any value. However, there is a list of standard records that have a defined standard interpretation by clients. A full list of standardized records can be found in [Records Reference](../domain-registry-essentials/reference.md).

Standard record keys are split by namespaces with a `.` used as a separator.

Main namespaces are:

* `crypto.*` - records related to crypto payments
* `dns.*` - DNS records
* `dweb.*` - records related to distributed content network protocols
* `browser.*` - hint records for web browsers

### Crypto Payments Records

One essential feature of crypto domains is the ability to specify a domain instead of a destination address for your crypto payment. Crypto Wallets that need this feature should revolve a domain to crypto address under the hook in the same way a browser resolves a domain to IP address.

All crypto addresses are stored within `crypto.*` namespace. Each currency address is stored as `crypto.<TICKER>.address` record. Example: Bitcoin address is stored in `crypto.BTC.address`. Addresses are stored in plain text format according to an address space standard established by each currency. The currency's namespace can contain additional currency-specific attributes to facilitate payment delivery. Example: [Ripple Destination Tag](https://xrpl.org/source-and-destination-tags.html). However, key names for those attributes are not yet standardized. Please contact [Unstoppable Domains Support](mailto:support@unstoppabledomains.com) if you need such attributes to be added to the standard.

Some tickers of very popular cryptocurrencies are not yet standardized. Example: `LINK` for [Chainlink](https://coinmarketcap.com/currencies/chainlink). A standardized list of tickers can be found in [SLIP-0044](https://github.com/satoshilabs/slips/blob/master/slip-0044.md). However, a more extended list of conventional tickers is available at [cripti/cryptocurrencies](https://github.com/crypti/cryptocurrencies/blob/master/cryptocurrencies.json).

Example crypto records setup:

| Key | Value |
| :--- | :--- |
| `crypto.ETH.address` | `0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe` |
| `crypto.BTC.address` | `bc1qkd4um2nn2uyzmsch5y86wsa2pfh8xl445lg9nv` |
| `crypto.ZIL.address` | `zil1yu5u4hegy9v3xgluweg4en54zm8f8auwxu0xxc` |

