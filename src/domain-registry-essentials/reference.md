# Records Reference

Records Reference is an overview of all standardized resolver record, that have a defined interpretation. Any other custom records are not forbidden to be set, however their interpretation remains custom to specific clients.

## List of Records

| Domain Name | Description | Format | Example | Docs |
| :--- | :--- | :--- | :--- | :--- |
| `crypto.ETH.address` | Ethereum Address to receive crypto payments | `0x[0-9a-fA-F]{40}` | `0x0f4a10a4f46c288cea365fcf45cccf0e9d901b94` | [Link](../managing-domains/managing-domain-records.md#crypto-payments-records) |
| `crypto.BTC.address` | Bitcoin Address to receive crypto payments | `[0-9a-zA-Z]{32}` | `1Nb7Mt1EqUqxxrAdmefUovS7aTgMUf2A6m` | [Link](../managing-domains/managing-domain-records.md#crypto-payments-records) |
| `crypto.<TICKER>.address` | Crypto currency address of the ticker. [Supported Currencies](https://github.com/crypti/cryptocurrencies) |  |  | [Link](../managing-domains/managing-domain-records.md#crypto-payments-records) |
| `browser.preferred_protocols` | Protocols that browser should prioritize to display content for | JSON serialized array | `["ipfs","http"]` | [Link](../browser-resolution/records.md) |
| `browser.redirect_url` | An URL where a browser should redirect a person if no other resolution method found. | [RFC-1738](https://tools.ietf.org/html/rfc1738) | `http://example.com/home.html` | [Link](../browser-resolution/records.md) |
| `dweb.ipfs.hash` | IPFS network content hash | `[0-9a-zA-Z]{46}` | `QmVaAtQbi3EtsfpKoLzALm6vXphdi2KjMgxEDKeGg6wHvK` | [Link](../browser-resolution/records.md) |
| `dweb.bzz.hash` | Swarm network content hash |  |  | [Link](../browser-resolution/records.md) |
| `dweb.<PROTOCOL>.hash` | Any other distributed content network content hash |  |  | [Link](../browser-resolution/records.md) |
| `dns.ttl` | Default TTL setting for all DNS records | `\d+` | `128` | [Link](../managing-domains/managing-domain-records.md#dns-records) |
| `dns.A` | DNS A record IP addresses | JSON serialized array | `["10.0.0.1","10.0.0.2"]` | [Link](../managing-domains/managing-domain-records.md#dns-records) |
| `dns.A.ttl` | TTL setting for all A records | `\d+` | `128` | [Link](../managing-domains/managing-domain-records.md#dns-records) |
| `dns.CNAME` | DNS CNAME record IP addresses | JSON serialized array | `["example.com."]` | [Link](../managing-domains/managing-domain-records.md#dns-records) |
| `dns.CNAME.ttl` | TTL setting for all CNAME records | `\d+` | `128` | [Link](../managing-domains/managing-domain-records.md#dns-records) |
| `dns.<RECORD>` | Specified DNS record values. [List of Records](https://en.wikipedia.org/wiki/List_of_DNS_record_types) | JSON serialized array |  | [Link](../managing-domains/managing-domain-records.md#dns-records) |
| `dns.<RECORD>.ttl` | TTL setting for corresponding type of records | `\d+` | `164` | [Link](../managing-domains/managing-domain-records.md#dns-records) |
| `ipfs.html.value` | Deprecated: use `dweb.ipfs.hash` instead. | `[0-9a-zA-Z]{46}` | `QmVaAtQbi3EtsfpKoLzALm6vXphdi2KjMgxEDKeGg6wHvK` | [Link](../browser-resolution/records.md) |
| `ipfs.redirect_domain.value` | Deprecated: use `browser.redirect_url` instead. | [RFC-1738](https://tools.ietf.org/html/rfc1738) | `http://example.com/home.html` | [Link](../browser-resolution/records.md) |
