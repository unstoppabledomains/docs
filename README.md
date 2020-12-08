# Unstoppable Domains Documentation

[![Unstoppable Domains Documentation](https://img.shields.io/badge/docs-unstoppabledomains.com-blue)](https://docs.unstoppabledomains.com/)

Unstoppable Domains Documentation is hosted by [GitBook](https://www.gitbook.com/) and can be accessed at
[docs.unstoppabledomains.com](https://docs.unstoppabledomains.com/).

The live version of the documentation is synced with [live-docs repository](https://github.com/unstoppabledomains/live-docs).

## Contributing

**General rules:**

- Try to follow [Google developer documentation style guide](https://developers.google.com/style).

**Working with templates and auto-generated pieces:**

###### Files that shouldn't be edited directly (follow described flow instead): 
- src/domain-registry-essentials/cns-smart-contracts.md:
	1. Edit *templates/cns-smart-contracts-template.md*
	2. Run `render:cns-contracts`

###### Auto-generated smart-contract address tables:

Contract addresses are pulled from [network-config.json](https://github.com/unstoppabledomains/dot-crypto/blob/master/src/network-config/network-config.json "network-config.json") contained in [dot-crypto](https://github.com/unstoppabledomains/dot-crypto).

- To fetch new config for existing contracts run `render:cns-contracts`. 
- If you need to add a new contract:
	1. Inside of the template insert `#include "templates/contracts/<YourContractName>.md"` where you want to add your table.
	2. Generate tables & compile the template using:
		- `render:cns-contracts` to render *src/domain-registry-essentials/cns-smart-contracts.md*

**Tips for external contributors:**

In order to preview a fork branch of the documentation, you can create a private [GitBook](https://www.gitbook.com/)
space and link your fork when setting up [GitHub integration](https://docs.gitbook.com/integrations/github).

**Tips for Unstoppable team:**

Branch previews are available at the [development GitBook space](https://app.gitbook.com/@unstoppable-domains/s/unstoppable-docs-dev/).