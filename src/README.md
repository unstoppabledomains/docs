# Introduction

{% hint style="info" %}
Unstoppable Domains has introduced a major documentation update. The old Website API documentation is available at [apidocs.unstoppabledomains.com](https://apidocs.unstoppabledomains.com/).
{% endhint %}

Unstoppable Domains is a San Francisco-based company building blockchain-based domains. These domains replace cryptocurrency addresses with human-readable names and are powered by the Ethereum and Zilliqa blockchains.

Unstoppable Domains are decentralized. This makes them, in a word, unstoppable. Once a user claims a domain to a wallet, they have absolute control over that domain. Domains can be transferred, updated, and linked to other services without any involvement from Unstoppable Domains. Unstoppable Domains cannot deactivate, change, or transfer a domain's records without a user's permission.

See the [Architecture overview](domain-registry-essentials/architecture-overview.md) for more detail on our Crypto and Zilliqa Name Services \(CNS and ZNS, respectively\).

## Who this documentation is for

This documentation is both an educational and a technical resource. We hope it will be equally useful for both technical and non-technical readers. First-time readers may want to start with our overview of [how CNS domains work](domain-registry-essentials/cns-smart-contracts.md). Those building an integration may want to start with our guides and documentation for [resolving and managing domains](domain-registry-essentials/resolving-domain-records.md).

## How to read this documentation

To get the most out of this documentation, we recommend a basic familiarity with how traditional domains and blockchain systems work. For some topics, developers should be familiar with the Solidity smart contract language. Sections that require knowledge of specific concepts contain links to resources and prerequisites. These include both external resources and references to our documentation.

Most of our code examples are written in JavaScript and, of course, Solidity. If you are developing in Java or Swift, our language-specific [resolution libraries](https://github.com/unstoppabledomains?q=resolution) may be helpful in translating the examples.

### Quick reference

#### Essentials

* [How Unstoppable Domains work](domain-registry-essentials/architecture-overview.md)
* [Resolving domain records](domain-registry-essentials/resolving-domain-records.md)
* [Namehashing algorithm](domain-registry-essentials/namehashing.md)
* [CNS smart contracts](domain-registry-essentials/cns-smart-contracts.md)

#### Wallets / Exchanges / Applications

* [Getting started](https://github.com/unstoppabledomains/docs/tree/d09d68165473b214e638f0951f2c2e38ee66a983/src/integrations/getting-started.md)
* [Crypto payments](https://github.com/unstoppabledomains/docs/tree/d09d68165473b214e638f0951f2c2e38ee66a983/src/integrations/crypto-payments.md)
* [Library configuration](wallets-exchanges-applications/library-configuration.md)
* [Resolve .crypto without libraries](https://medium.com/unstoppabledomains/how-to-resolve-crypto-domain-names-82046db0404a)
* [Resolve .zil without libraries](https://medium.com/unstoppabledomains/how-to-resolve-zil-domain-names-f43da8fe37a9)

#### Browsers

* [Resolving domains in a browser](browser-resolution/resolving-domains-in-a-browser.md)
* [Browser resolution algorithm](browser-resolution/browser-resolution-algorithm.md)
* [Browser resolution test domains](browser-resolution/test-domains.md)

### Technical specifications

* [Records reference](domain-registry-essentials/records-reference.md)
* [Browser resolution algorithm](browser-resolution/browser-resolution-algorithm.md)  

### Other resources

* [Get test domain](get-test-domain.md)
* [Website API reference](https://apidocs.unstoppabledomains.com/)
* Resolution Libraries
  * JavaScript [\[library\]](https://www.npmjs.com/package/@unstoppabledomains/resolution), [\[github\]](https://github.com/unstoppabledomains/resolution)
  * Java [\[library\]](https://search.maven.org/artifact/com.unstoppabledomains.resolution/resolution/1.1.0/jar), [\[github\]](https://github.com/unstoppabledomains/resolution-java)
  * Swift [\[library\]](https://cocoapods.org/pods/UnstoppableDomainsResolution), [\[github\]](https://github.com/unstoppabledomains/resolution-swift)
  * Golang [github](https://github.com/unstoppabledomains/resolution-go)

