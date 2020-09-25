# Records Reference

This page contains an overview of all standardized resolver records. The reference is divided into sections, grouping records by their namespaces and use-cases:

* [Crypto payments](records-reference.md#crypto-payments)
* [Browser resolution](records-reference.md#browser-resolution)
  * [DWeb records](records-reference.md#dweb-records)
  * [DNS records](records-reference.md#dns-records)

Developers may also set custom records for the domains. Domain records are stored as a key-value dictionary by Resolvers and are not validated on the smart-contract level. For more details, check out [Architecture Overview](architecture-overview.md#resolver) and [Managing Domain Records](../managing-domains/managing-domain-records.md).

## Crypto payments

For information about crypto payments, see [Managing Domain Records - Crypto Payments Records](../managing-domains/managing-domain-records.md#crypto-payments-records).

#### crypto.ETH.address

Ethereum Address to receive crypto payments.

| Format | Example |
| :--- | :--- |
| `0x[0-9a-fA-F]{40}` | `0x0f4a10a4f46c288cea365fcf45cccf0e9d901b94` |

#### crypto.BTC.address

Bitcoin Address to receive crypto payments.

| Format | Example |
| :--- | :--- |
| `[0-9a-zA-Z]{32}` | `0x0f4a10a4f46c288cea365fcf45cccf0e9d901b94` |

#### crypto.&lt;TICKER&gt;.address

Crypto currency address of the ticker.

## Browser resolution

Browser resolution is described in the [Browser Resolution Algorithm](../browser-resolution/browser-resolution-algorithm.md) article.

#### browser.preferred\_protocols

Protocols that browser should prioritize to display content for.

| Format | Example |
| :--- | :--- |
| JSON serialized array | `["ipfs","http"]` |

#### browser.redirect\_url

An URL where a browser should redirect a person if no other resolution method found.

| Format | Example |
| :--- | :--- |
| [RFC-1738](https://tools.ietf.org/html/rfc1738) | `http://example.com/home.html` |

### DWeb records

For more details, see [Browser Resolution Algorithm - DWeb Records](../browser-resolution/browser-resolution-algorithm.md#distributed-web-records).

#### dweb.ipfs.hash

IPFS network content hash.

| Format | Example |
| :--- | :--- |
| `[0-9a-zA-Z]{46}` | `QmVaAtQbi3EtsfpKoLzALm6vXphdi2KjMgxEDKeGg6wHvK` |

#### dweb.bzz.hash

Swarm network content hash.

TODO: example

### DNS records

For more details, see [Browser Resolution Algorithm - DNS Records](../browser-resolution/browser-resolution-algorithm.md#dns-records).

#### dns.ttl

Default TTL setting for all DNS records.

| Format | Example |
| :--- | :--- |
| `\d+` | `128` |

#### dns.A

DNS A record IP addresses.

| Format | Example |
| :--- | :--- |
| JSON serialized array | `["10.0.0.1","10.0.0.2"]` |

#### dns.A.ttl

TTL setting for all A records.

| Format | Example |
| :--- | :--- |
| `\d+` | `128` |

#### dns.CNAME

DNS CNAME record IP addresses.

| Format | Example |
| :--- | :--- |
| JSON serialized array | `["example.com."]` |

#### dns.CNAME.ttl

TTL setting for all CNAME records.

| Format | Example |
| :--- | :--- |
| `\d+` | `128` |

#### dns.&lt;RECORD&gt;

Specified DNS record values..

| Format | Example |
| :--- | :--- |
| JSON serialized array | `["example.com."]` |

#### dns.&lt;RECORD&gt;.ttl

TTL setting for corresponding type of records.

| Format | Example |
| :--- | :--- |
| `\d+` | `164` |

### Deprecated records

#### ipfs.html.value

Deprecated: use `dweb.ipfs.hash` instead.

| Format | Example |
| :--- | :--- |
| `[0-9a-zA-Z]{46}` | `QmVaAtQbi3EtsfpKoLzALm6vXphdi2KjMgxEDKeGg6wHvK` |

#### ipfs.redirect\_domain.value

Deprecated: use `browser.redirect_url` instead.

| Format | Example |
| :--- | :--- |
| [RFC-1738](https://tools.ietf.org/html/rfc1738) | `http://example.com/home.html` |

