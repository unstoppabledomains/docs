# Records Reference

This page contains an overview of all standardized resolver records. The reference is divided into sections, grouping records by their namespaces and use-cases:

* [Crypto payments](records-reference.md#crypto-payments)
* [Browser resolution](records-reference.md#browser-resolution)
  * [DWeb records](records-reference.md#dweb-records)
  * [DNS records](records-reference.md#dns-records)

Developers may also set custom records for the domains. Domain records are stored as a key-value dictionary by Resolvers and are not validated on the smart-contract level. For more details, check out [Architecture Overview](architecture-overview.md#resolver).

## Crypto payments

For information about crypto payments, see [Managing Domain Records](../managing-domains/managing-domain-records.md#crypto-payments-records).

#### crypto.ETH.address

Ethereum Address to receive crypto payments

| Format | Example |
| :--- | :--- |
| `0x[0-9a-fA-F]{40}` | `0x0f4a10a4f46c288cea365fcf45cccf0e9d901b94` |

### Browser resolution

### DWeb records

### DNS records

## List of Records

| Domain Name | Description | Docs | Format | Example |
| :--- | :--- | :--- | :--- | :--- |
| `crypto.ETH.address` | Ethereum Address to receive crypto payments | [Link](../managing-domains/managing-domain-records.md#crypto-payments-records) | `0x[0-9a-fA-F]{40}` | `0x0f4a10a4f46c288cea365fcf45cccf0e9d901b94` |
| `crypto.BTC.address` | Bitcoin Address to receive crypto payments | [Link](../managing-domains/managing-domain-records.md#crypto-payments-records) | `[0-9a-zA-Z]{32}` | `1Nb7Mt1EqUqxxrAdmefUovS7aTgMUf2A6m` |
| `crypto.<TICKER>.address` | Crypto currency address of the ticker. [Supported Currencies](https://github.com/crypti/cryptocurrencies) | [Link](../managing-domains/managing-domain-records.md#crypto-payments-records) |  |  |
| `browser.preferred_protocols` | Protocols that browser should prioritize to display content for | [Link](../browser-resolution/browser-resolution-algorithm.md#records-related-to-browser-resolution) | JSON serialized array | `["ipfs","http"]` |
| `browser.redirect_url` | An URL where a browser should redirect a person if no other resolution method found. | [Link](../browser-resolution/browser-resolution-algorithm.md#records-related-to-browser-resolution) | [RFC-1738](https://tools.ietf.org/html/rfc1738) | `http://example.com/home.html` |
| `dweb.ipfs.hash` | IPFS network content hash | [Link](../browser-resolution/browser-resolution-algorithm.md#distributed-web-records) | `[0-9a-zA-Z]{46}` | `QmVaAtQbi3EtsfpKoLzALm6vXphdi2KjMgxEDKeGg6wHvK` |
| `dweb.bzz.hash` | Swarm network content hash | [Link](../browser-resolution/browser-resolution-algorithm.md#distributed-web-records) |  |  |
| `dweb.<PROTOCOL>.hash` | Any other distributed content network content hash | [Link](../browser-resolution/browser-resolution-algorithm.md#distributed-web-records) |  |  |
| `dns.ttl` | Default TTL setting for all DNS records | [Link](../browser-resolution/browser-resolution-algorithm.md#dns-records) | `\d+` | `128` |
| `dns.A` | DNS A record IP addresses | [Link](../browser-resolution/browser-resolution-algorithm.md#dns-records) | JSON serialized array | `["10.0.0.1","10.0.0.2"]` |
| `dns.A.ttl` | TTL setting for all A records | [Link](../browser-resolution/browser-resolution-algorithm.md#dns-records) | `\d+` | `128` |
| `dns.CNAME` | DNS CNAME record IP addresses | [Link](../browser-resolution/browser-resolution-algorithm.md#dns-records) | JSON serialized array | `["example.com."]` |
| `dns.CNAME.ttl` | TTL setting for all CNAME records | [Link](../browser-resolution/browser-resolution-algorithm.md#dns-records) | `\d+` | `128` |
| `dns.<RECORD>` | Specified DNS record values. [List of Records](https://en.wikipedia.org/wiki/List_of_DNS_record_types) | [Link](../browser-resolution/browser-resolution-algorithm.md#dns-records) | JSON serialized array |  |
| `dns.<RECORD>.ttl` | TTL setting for corresponding type of records | [Link](../browser-resolution/browser-resolution-algorithm.md#dns-records) | `\d+` | `164` |
| `ipfs.html.value` | Deprecated: use `dweb.ipfs.hash` instead. | [Link](../browser-resolution/browser-resolution-algorithm.md#legacy-records-support) | `[0-9a-zA-Z]{46}` | `QmVaAtQbi3EtsfpKoLzALm6vXphdi2KjMgxEDKeGg6wHvK` |
| `ipfs.redirect_domain.value` | Deprecated: use `browser.redirect_url` instead. | [Link](../browser-resolution/browser-resolution-algorithm.md#legacy-records-support) | [RFC-1738](https://tools.ietf.org/html/rfc1738) | `http://example.com/home.html` |

