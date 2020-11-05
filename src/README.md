# Introduction

{% hint style="info" %}
Unstoppable Domains has introduced a major documentation update. The old Website API documentation is still available at [apidocs.unstoppabledomains.com](https://apidocs.unstoppabledomains.com/).
{% endhint %}

Unstoppable Domains is a San Francisco-based company building domains on blockchains. These domains replace cryptocurrency addresses with human-readable names and are powered by the Ethereum and Zilliqa blockchains. CNS and ZNS provide users with full control of their domain names, which ensures permanent ownership, censorship resistance, and decentralized access.

By utilizing a Self Custody paradigm for domain ownership Unstoppable Domains removes the centralized nature of traditional domains. Once a user controls their domain, every action must be user-initiated. Transferring and configuring records will always be able to be done by the user without Unstoppable Domain's interference. This is in contrast with the current security model for DNS witch allows TLDs to control their subdomains at will. See the [Architecture overview](domain-registry-essentials/architecture-overview.md) for more.

## Who this documentation is for

This documentation is written for different audiences and is useful for developers and non-developers alike. For those beginning their journey with Unstoppable Domains, we introduce the concept of how CNS domains work. For developers, we have guides and reference documentation explaining how to resolve and manage domains.

## How to read this documentation

In general, it is assumed that a reader has a basic understanding of traditional domains and how blockchain works. Some developer topics will require familiarity with Solidity language. Sections that require knowledge of specific concepts contain links to resources and prerequisites, including both external ones and references to our documentation.

Developer guides contain examples of code, mostly written in JavaScript and Solidity. The choice of the JavaScript language was motivated by the fact that the most mature libraries of our ecosystem are built with JavaScript and we assume that this language is available for the majority of our technical readers, so they can get the examples working without much effort.

### Navigating

* If you want an understanding of how **Unstoppable Domains work, its architecture and capabilities**, check out [Architecture overview](domain-registry-essentials/architecture-overview.md).
* If you're a **developer** interested in **resolving records for cryptocurrency payments**, check out [Resolving domain records](domain-registry-essentials/resolving-domain-records.md).
* If you're a **developer** interested in **how to resolve our domains in a browser**, check out [Resolving domains in a browser](browser-resolution/resolving-domains-in-a-browser.md).
* If you're a **developer** interested in **integrating your product with Unstoppable**, check out the [Getting started](integrations/getting-started.md) guide for integrators.  

### Specs

* [Records reference](domain-registry-essentials/records-reference.md)
* [Browser resolution algorithm](browser-resolution/browser-resolution-algorithm.md)

### Other resources

* JavaScript [@unstoppabledomains/resolution](https://www.npmjs.com/package/@unstoppabledomains/resolution) library \[[github](https://github.com/unstoppabledomains/resolution)\]
* Java [com.unstoppabledomains.resolution](https://search.maven.org/artifact/com.unstoppabledomains.resolution/resolution/1.1.0/jar) library \[[github](https://github.com/unstoppabledomains/resolution-java)\]
* Swift [UnstoppableDomainsResolution](https://cocoapods.org/pods/UnstoppableDomainsResolution) library \[[github](https://github.com/unstoppabledomains/resolution-swift)\]
* [Website API reference](https://apidocs.unstoppabledomains.com/)

