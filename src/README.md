# Introduction

{% hint style="info" %}
Unstoppable Domains has introduced a major documentation update. The old Website API documentation is available at [apidocs.unstoppabledomains.com](https://apidocs.unstoppabledomains.com/).
{% endhint %}

Unstoppable Domains is a San Francisco-based company building blockchain-based domains. These domains replace cryptocurrency addresses with human-readable names and are powered by the Ethereum and Zilliqa blockchains.

Unstoppable Domains are decentralized. This makes them, in a word, unstoppable. Once a user purchases a domain, they have absolute control over that domain. Domains can be transferred, updated, and linked to other services completely without Unstoppable Domains' involvement. Unstoppable Domains cannot deactivate, change, or transfer a domain's records without a user's permission.

This approach stands in stark contrast to traditional domain systems where takedowns and seizures are a fact of life. This is a bold approach, but we believe complete decentralization is the only way to guarantee censorship resistance, irrevocable access, and permanent ownership for our users.

See the [Architecture overview](domain-registry-essentials/architecture-overview.md) for more detail on our Crypto and Zilliqa Name Services \(CNS and ZNS, respectively\).

## Who this documentation is for

This documentation is both an educational and a technical resource. We hope it will be equally useful for both technical and non-technical readers. First-time readers may want to start with our overview of [how CNS domains work](domain-registry-essentials/cns-smart-contracts.md). Those building an integration may want to start with our guides and documentation for [resolving and managing domains](domain-registry-essentials/resolving-domain-records.md).

## How to read this documentation

To get the most out of this documentation, we recommend a basic familiarity with how traditional domains and blockchain systems work. For some topics, developers should be familiar with the Solidity smart contract language. Sections that require knowledge of specific concepts contain links to resources and prerequisites. These include both external resources and references to our documentation.

Most of our code examples are written in JavaScript and, of course, Solidity. If you are developing in Java or Swift, our language-specific [resolution libraries](https://github.com/unstoppabledomains?q=resolution) may be helpful in translating the examples.

### Quick reference

* **How Unstoppable Domains work** — [Architecture overview](domain-registry-essentials/architecture-overview.md).
* **How to resolve records for payments** — [Resolving domain records](domain-registry-essentials/resolving-domain-records.md)
* **How to resolve domains in a browser** — [Resolving domains in a browser](browser-resolution/resolving-domains-in-a-browser.md).
* **How to integrate your product with Unstoppable** — [Getting started with an integration](integrations/getting-started.md)

### Technical specifications

* [Records reference](domain-registry-essentials/records-reference.md)
* [Browser resolution algorithm](browser-resolution/browser-resolution-algorithm.md)

### Other resources

* [Website API reference](https://apidocs.unstoppabledomains.com/)
* Resolution Libraries
  * JavaScript [\[library\]](https://www.npmjs.com/package/@unstoppabledomains/resolution), [\[github\]](https://github.com/unstoppabledomains/resolution)
  * Java [\[library\]](https://search.maven.org/artifact/com.unstoppabledomains.resolution/resolution/1.1.0/jar), [\[github\]](https://github.com/unstoppabledomains/resolution-java)
  * Swift [\[library\]](https://cocoapods.org/pods/UnstoppableDomainsResolution), [\[github\]](https://github.com/unstoppabledomains/resolution-swift)

