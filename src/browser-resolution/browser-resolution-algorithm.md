# Browser Resolution Algorithm

!\[Resolve Dweb website by direct reading from Ethereum and Decentralized network\]\(./documentation/diagrams/browser-resolution/Resolve DWeb website by direct reading from Ethereum and Decentralized network.png\)

This section explains how different domain record configurations should be interpreted by browsers.

A browser can select a protocol it has a support for. If a domain is configured for multiple protocols, it should prioritize a protocol based on `browser.preferred_protocols` record that can be set to a list of the defined protocols.

If `browser.preferred_protocols` is not set, a browser should use the following value as a default `["bzz", "ipfs", "https", "http", "ftp"]`. If `browser.preferred_protocols` is set but is not complete a browser should fullfill the absent protocols at the end in the default order specified above. A domain can have a single content identifier for each distributed protocol stored in `dweb.<protocol>.hash`. Ex: `dweb.bzz.hash` for Swarm's `bzz` protocol. See [Dweb Records](../managing-domains/managing-domain-records.md#distributed-web-records) for more information.

If none of `dweb` hash records is set, a browser should fall back to DNS resolution that is set within `dns.*` namespace. See [DNS Records](../managing-domains/managing-domain-records.md#distributed-web-records) for more information

Generally browsers automatically add `http://` prefix for any domain in the address bar if the protocol is not specified explicitly by a user. In case of blockchain domain names \(assuming a browser supports many protocols\), it is preferred to determine a protocol only after resolving domain records.

![](../.gitbook/assets/browser_resolution_algorithm.png)

### Legacy Records Support

As of Q3 2020, most .crypto domains are configured using legacy record names for IPFS hash and redirect domain:

1. `ipfs.html.value` deprecated in favor of `dweb.ipfs.hash`
2. `ipfs.redirect_domain` deprecated in favor of `browser.redirect_url`

Browsers are strongly recommended to support those records as a fallback when corresponding replacement records are not set.

