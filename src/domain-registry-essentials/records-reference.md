# Records Reference

This page contains an overview of all standardized `Resolver` records. The reference is divided into sections, grouping records by their namespaces and use-cases:

* [Crypto payments](records-reference.md#cryptocurrency-payments)
* [Browser resolution](records-reference.md#browser-resolution)
  * [DWeb records](records-reference.md#dweb-records)
  * [DNS records](records-reference.md#dns-records)

Developers may also set custom records for the domains. Domain records are stored as a key-value dictionary by `Resolvers` and are not validated on the smart-contract level. For more details, read [Architecture Overview](architecture-overview.md#resolver) and [Managing Domain Records](../allow-my-users-to-manage-existing-domains/managing-domain-records.md).

{% hint style="info" %}
A list of keys supported by Unstoppable Domains can be found in [reference json file](https://github.com/unstoppabledomains/dot-crypto/blob/master/src/supported-keys/supported-keys.json)
{% endhint %}

## Cryptocurrency payments

For information about crypto payments, see [Managing Domain Records - Cryptocurrency Payment Records](../allow-my-users-to-manage-existing-domains/managing-domain-records.md#crypto-payment-records).

#### `crypto.ETH.address`

Ethereum Address to receive cryptocurrency payments.

| Format | Example |
| :--- | :--- |
| `0x[0-9a-fA-F]{40}` | `0x0f4a10a4f46c288cea365fcf45cccf0e9d901b94` |

#### `crypto.BTC.address`

Bitcoin Address to receive cryptocurrency payments.

| Format | Example |
| :--- | :--- |
| [Address - Bitcoin Wiki](https://en.bitcoin.it/wiki/Address#:~:text=A%20Bitcoin%20address%2C%20or%20simply,by%20any%20user%20of%20Bitcoin.) | `1Nb7Mt1EqUqxxrAdmefUovS7aTgMUf2A6m` |

#### `crypto.<TICKER>.address`

Cryptocurrency address of the ticker.

### Multi-chain currencies

Some currencies exist on multiple chains.

#### `crypto.USDT.version.ERC20.address`

| Format | Example |
| :--- | :--- |
| `0x[0-9a-fA-F]{40}` | `0x8aaD44321A86b170879d7A244c1e8d360c99DdA8` |

#### `crypto.USDT.version.TRON.address`

| Format | Example |
| :--- | :--- |
| [Address - Tron Docs](https://developers.tron.network/docs/account#address-format) | `THG9jVSMfKEbg4vYTYWjmLRyga3CKZdDsk` |

#### `crypto.USDT.version.EOS.address`

| Format | Example |
| :--- | :--- |
| `^[a-z][a-z1-5.]{10}[a-z1-5]$` | `unstoppabledomains` |

#### `crypto.USDT.version.OMNI.address`

| Format | Example |
| :--- | :--- |
| [Address - Bitcoin Wiki](https://en.bitcoin.it/wiki/Address#:~:text=A%20Bitcoin%20address%2C%20or%20simply,by%20any%20user%20of%20Bitcoin.) | `16df369whGV8o3DVeGBmfSNwytaqZGWtYJ` |

#### `crypto.<TICKER>.version.<VERSION>.address`

Cryptocurrency address of ticker version

## Browser resolution

Browser resolution is described in the [Browser Resolution Algorithm](../support-unstoppable-domains-in-a-web-browser/browser-resolution-algorithm.md) page.

#### `browser.preferred_protocols`

Protocols that browser should prioritize to display content for.

| Format | Example |
| :--- | :--- |
| JSON serialized array | `["ipfs","http"]` |

#### `browser.redirect_url`

A fallback URL, to which a user will be redirected if no other resolution method is supported.

| Format | Example |
| :--- | :--- |
| [RFC-1738](https://tools.ietf.org/html/rfc1738) | `http://example.com/home.html` |

### DWeb records

For more details, read [Browser Resolution Algorithm - DWeb Records](../support-unstoppable-domains-in-a-web-browser/browser-resolution-algorithm.md#distributed-web-records).

#### `dweb.ipfs.hash`

IPFS network content hash.

| Format | Example |
| :--- | :--- |
| `[0-9a-zA-Z]{46}` | `QmVaAtQbi3EtsfpKoLzALm6vXphdi2KjMgxEDKeGg6wHvK` |

#### `dweb.bzz.hash`

Swarm network content hash.

| Format | Example |
| :--- | :--- |
| `[0-9a-f]{64}` | `d1f25a870a7bb7e5d526a7623338e4e9b8399e76df8b634020d11d969594f24a` |

### DNS records

For more details, see [Browser Resolution Algorithm - DNS Records](../support-unstoppable-domains-in-a-web-browser/browser-resolution-algorithm.md#dns-records).

#### `dns.ttl`

Default TTL setting for all DNS records.

| Format | Example |
| :--- | :--- |
| `\d+` | `128` |

#### `dns.A`

DNS A record IP addresses.

| Format | Example |
| :--- | :--- |
| JSON serialized array | `["10.0.0.1","10.0.0.2"]` |

#### `dns.A.ttl`

TTL setting for all A records.

| Format | Example |
| :--- | :--- |
| `\d+` | `128` |

#### `dns.CNAME`

DNS CNAME record IP addresses.

| Format | Example |
| :--- | :--- |
| JSON serialized array | `["example.com."]` |

#### `dns.CNAME.ttl`

TTL setting for all CNAME records.

| Format | Example |
| :--- | :--- |
| `\d+` | `128` |

#### `dns.<RECORD>`

Specified DNS record values.

| Format | Example |
| :--- | :--- |
| JSON serialized array | `["example.com."]` |

#### `dns.<RECORD>.ttl`

TTL setting for corresponding type of records.

| Format | Example |
| :--- | :--- |
| `\d+` | `164` |

### Deprecated records

#### `ipfs.html.value`

Deprecated: use `dweb.ipfs.hash` instead.

| Format | Example |
| :--- | :--- |
| `[0-9a-zA-Z]{46}` | `QmVaAtQbi3EtsfpKoLzALm6vXphdi2KjMgxEDKeGg6wHvK` |

#### `ipfs.redirect_domain.value`

Deprecated: use `browser.redirect_url` instead.

| Format | Example |
| :--- | :--- |
| [RFC-1738](https://tools.ietf.org/html/rfc1738) | `http://example.com/home.html` |

