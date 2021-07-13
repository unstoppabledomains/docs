# Resolving domains in a browser

This document describes how to resolve blockchain domains using a traditional HTTP Web Browser or a Dapp Browser. The document assumes that a reader has a basic understanding of Unstoppable domains resolution. See [Resolving domain records](../domain-registry-essentials/resolving-domain-records.md).

## End-user features

### HTTP website browsing

```text
Given a blockchain domain has a DNS record configured.
When user enters the domain name into a browser address bar.
Then the browser resolves the domain and gets the specified DNS records.
Then the browser requests and displays the content using DNS protocol and HTTP protocol.
```

### Distributed website browsing

```text
Given a blockchain domain has a dweb content identifier record configured (e.g.: an IPFS hash).
When a user enters the domain name into a browser address bar.
Then the browser resolves the domain and gets the content hash of a domain.
Then the browser retrieves the content by the hash using a related protocol and displays the content.
```

![](../.gitbook/assets/overview_read_dweb_website_from_ethereum_and_decentralized_network%20%284%29.png)

### Domain-level redirect

```text
Given a blockchain domain has both a redirect URL and IPFS hash configured, and the user's browser doesn't support IPFS protocol.
When a user enters the domain name into a browser address bar.
Then the browser resolves the domain and gets both the redirect URL and IPFS hash records.
Then browser redirects the user to the redirect URL because the IPFS protocol is not supported.
```

### Resolution configuration

```text
Given a user wants to change ETH provider services.
When the user goes to the browser settings crypto domains section.
  - User changes the Ethereum node URL from default to another.
  - User changes Registry Address for each support crypto registry.
  - User changes network for Ethereum node.
  - User changes DNS gateway
  - User changes Dweb gateway
Then the browser uses new settings to make requests to Ethereum blockchain
  - If the network is not specified explicitly, it can be retrieved from the Ethereum node URL.
  - If the Registry Address is not specified, it can use a default for the specified network
```

![](../.gitbook/assets/configure_dns_gateway%20%284%29.png)

## Hypermedia protocol

In addition to base browser hypermedia protocols like `http`, blockchain domains can also be configured for distributed content protocols like `ipfs`. These hypermedia protocols can be associated with a crypto domain:

* Traditional
  * HTTP
  * HTTPS
  * FTP
* Distributed
  * [IPFS](https://en.wikipedia.org/wiki/InterPlanetary_File_System) - `ipfs://`
  * [Swarm](https://swarm-guide.readthedocs.io/en/stable/architecture.html#the-bzz-protocol) - `bzz://`

A browser may support any subset of traditional or distributed protocols that still make crypto domain websites displayable.

## Gateway to simplify the integration

While it is possible to resolve a domain via a call to ETH RPC and support distributed content protocols in a browser, it might be easier to make those calls via gateways using protocols already supported by all browsers: HTTP and DNS. A gateway may simplify the integration to a browser but comes at the downside of decreased decentralization \(if the gateway is hosted by a third party\) or a more complex user experience \(if the gateway is hosted by the user\).

There are 2 possible gateways for each problem:

* Distributed content \(Dweb\) gateway
* Resolution over DNS gateway

See a description of how they work below

![](../.gitbook/assets/overview_dweb_website_via_dns_dweb_gateways%20%284%29.png)

### Distributed content gateway

A gateway is an HTTP Server that acts as a proxy between HTTP and a distributed content protocol. Basic functionality of such a gateway:

1. Receive HTTP request to a blockchain domain \(like `http://example.crypto`\)
2. Resolve the domain into crypto records
3. Get the content based on [Browser resolution algorithm](browser-resolution-algorithm.md)
4. Return the content to the client via HTTP

### Resolution over DNS gateway

A gateway is a DNS Server that resolves not just traditional domains but also `.crypto` domains. Basic functionality of such a gateway:

1. Receive a domain resolution request
2. Resolve a domain using classical DNS system if is in classical TLD \(like `.com`\)
3. Resolve a domain using [Browser resolution algorithm](browser-resolution-algorithm.md) if it is in crypto TLD
   * If a domain is set using DNS, transform [Crypto DNS records](browser-resolution-algorithm.md#dns-records) into classical records
   * If a domain is set using distributed content
     * If the client requests `A` record, resolve to [Distributed content gateway](resolving-domains-in-a-browser.md#distributed-content-gateway) IP Address
     * If the client requests a `TXT` record, resolve to all crypto records in JSON encoded key-value format
4. Send resolution to client

![](../.gitbook/assets/resolve_dweb_website_via_dns_gateway_and_dweb_gateway%20%284%29.png)

