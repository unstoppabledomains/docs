# Managing Domain Records

Domain records can be managed via default public resolver. One can develop its own custom resolver with any management permissions defined.

### Using Default Public Resolver

Default public resolver allows to manage all domain records for any address given a permission over domain as per [ERC721 "Transfer Mechanism"](https://eips.ethereum.org/EIPS/eip-721) section. These includes:

* Owner address of a domain
* Approved address for a domain
* Owner's operator addresses

See ERC721 on how those permissions can be granted and revoked. Any records change is submitted as a [Ethereum Blockchain Transaction](https://ethereum.org/en/whitepaper/#messages-and-transactions).

Records Management can be done via [Resolver methods](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/IResolver.sol).

### Main Records

Records on top level are stored in a simple key-value mapping of string to string. Crypto registry doesn't forbid a user to assign any record to any value. However, there is a list of standard records that have a defined standard interpretation by clients. A full list of standardized records can be found in [Records Reference](https://github.com/unstoppabledomains/dot-crypto/blob/master/RECORDS_REFERENCE.md).

Standard record keys are split by namespaces with `.` used as a separator.

Main namespaces are:

* `crypto.*` - records related to crypto payments
* `dns.*` - DNS records
* `dweb.*` - records related to distributed content network protocols
* `browser.*` - hint records for web browsers

#### Crypto Payments Records

One of essential feature of crypto domains is the ability to make specify a domain instead of a destination address for your crypto payment. Crypto Wallets that need this feature should revolve a domain to crypto address under the hook in the same way browser resolves a domain to IP address.

All crypto addresses are stored within `crypto.*` namespace. Each currency address is stored as `crypto.<TICKER>.address` record. Example: Bitcoin address is stored in `crypto.BTC.address`. Addresses are stored in plain text format according to an address space standard established by each individual currency Currency namespace can contain additional currency specific attributes to facilitate payment delivery. Example: [Ripple Destination Tag](https://xrpl.org/source-and-destination-tags.html). However, key names for those attributes are not yet standardized. Please contact [Unstoppable Domains Support](mailto:support@unstoppabledomains.com) if you need such attributes to be added to the standard.

Some tickers of very popular crypto currencies are not yet standardized. Example: `LINK` for [Chainlink](https://coinmarketcap.com/currencies/chainlink). Standardized list of tickers can be found in [SLIP-0044](https://github.com/satoshilabs/slips/blob/master/slip-0044.md). However, more extended list of conventional tickers is available at [cripti/cryptocurrencies](https://github.com/crypti/cryptocurrencies/blob/master/cryptocurrencies.json).

Example crypto records setup:

| Key | Value |
| :--- | :--- |
| `crypto.ETH.address` | `0xD1E5b0FF1287aA9f9A268759062E4Ab08b9Dacbe` |
| `crypto.BTC.address` | `bc1qkd4um2nn2uyzmsch5y86wsa2pfh8xl445lg9nv` |
| `crypto.ZIL.address` | `zil1yu5u4hegy9v3xgluweg4en54zm8f8auwxu0xxc` |

#### DNS records

Resolver records may contain classical DNS records besides other records. In order to distinguish those from other crypto records, the `dns.*` namespace is used. So DNS `A` corresponds to `dns.A` crypto record. Any [listed DNS record](https://en.wikipedia.org/wiki/List_of_DNS_record_types) described in RFC standards is supported. All record names must follow upper case naming convention.

As crypto resolver doesn't support multiple records with the same key, but DNS does allow that. Therefore, DNS record value must always be stored as [JSON](http://json.org/) serialized array of strings. Example 1: a domain that needs one `CNAME` record set to `example.com.` must be configured as one crypto record `dns.CNAME` set to `["example.com."]`. Example 2: a domain that needs two `A` records set to `10.0.0.1` and `10.0.0.2` must be configured as one crypto record `dns.A` set to `["10.0.0.1","10.0.0.2"]`.

No other data transformation is required when converting a traditional DNS record into Crypto record other than aggregating records with the same name to one record using serialization as JSON array of strings.

Crypto records do not have a domain name associated to them. That is why there is no feature of storing your subdomain records inside a parent domain. Example: `www.example.com` record can only be set inside a resolver of `www.example.com` but never inside `example.com`.

A recommended way to display content in a browser for crypto domains is explained in [Resolving Domains in a Browser](../browser-resolution/resolving-domains-in-a-browser)

**TTL**

Records TTL can be set for all records or for individual type of record. TTL for all records can be set in `dns.ttl`. TTL for individual record type can be set in `dns.<RECORD>.ttl`. If `ttl` for individual record type is not set, a default `dns.ttl` need to be applied.

Example crypto records setup:

| Record | Value |
| :--- | :--- |
| dns.A | \["10.0.0.1", "10.0.0.2"\] |
| dns.A.ttl | 168 |
| dns.AAAA | \["2a00:1450:401b:805::200e"\] |
| dns.MX | \["10 aspmx.example.com."\] |
| dns.ttl | 128 |

Should be transformed into the following DNS records:

| Record | Value | TTL |
| :--- | :--- | :--- |
| A | 10.0.0.1 | 168 |
| A | 10.0.0.2 | 168 |
| AAAA | 2a00:1450:401b:805::200e | 128 |
| MX | 10 aspmx.example.com. | 128 |

TTL for individual records of the same type is currently unsupported due to the necessity to change the record value format and increased gas cost. Setting `dns.ttl` instead of TTL for individual records is recommended due to higher gas efficiency.

**Authority responses**

It is a common practice in DNS to have an authority of a subdomain delegated to a parent domain. This mechanism is not necessary for crypto domains because the cost of subdomain registration is comparable to setting records. In other words, configuring subdomain using the parent domain has no benefit and may result in even higher gas cost due to the necessity to store associated subdomain name to each record.

Therefore, authority configurations are not supported by crypto domains at the moment.

#### Distributed Web records

Distributed Web \(Dweb\) records are designed to allow one to configure a domain for distributed websites protocols like IPFS or Swarm. Such records are stored in `dweb.*` namespace. Each protocol has its own sub-namespace for its data using canonic name. Example: Swarm's protocol canonic name is `bzz` so its records are stored at `dns.bzz.*` namespace.

Record structure can be different based on the protocol. However, all protocols have a common `.hash` record used to reference a content in the distributed network. Example: `dns.ipfs.hash` for IPFS protocol.

See [Resolving Domains in a Browser](../browser-resolution/resolving-domains-in-a-browser.md) for an information how to interpret those records.

