# Get a test domain

To make integrations easier, Unstoppable Domains allows developers to request a free `.crypto` domain. To distinguish these test domains from paid domains, all test domains are prefixed with `udtestdev-`. For example:

- `udtestdev-freedomainisawesome.crypto`
- `udtestdev-test-wallet-integration.crypto`

To prevent abuse, we ask that developers cover the cost of minting and transferring these domains to their wallets. **Unstoppable Domains does not make any money from issuing test domains.**

## Requirements

- [MetaMask](https://metamask.io/) or any wallet with [WalletConnect](https://walletconnect.org/wallets) support
- Ether on your Ethereum wallet to pay transaction fees

To calculate the transaction fee you'd like to target you can use services like [ETH Gas Station](https://ethgasstation.info/calculatorTxV.php). The average transaction gas usage to mint and transfer a new domains is around 180,000 gas.

## Get a domain

-  Open the domain request Smart Contract using [Etherscan](https://etherscan.io/address/0x1fC985cAc641ED5846b631f96F35d9b48Bc3b834#writeContract)

![](../.gitbook/assets/integrations/get-test-domain/step-1.png)

- Connect your wallet

![](../.gitbook/assets/integrations/get-test-domain/step-2.png)

- Pick the suffix you'd like to use for the domain and press the “Write” button to start the transaction

![](../.gitbook/assets/integrations/get-test-domain/step-3.png)

- Sign the transaction

![](../.gitbook/assets/integrations/get-test-domain/step-4.png)

**IMPORTANT: If transaction takes more gas than expected (more than 200,000 gas to call the claim method) or fails, that domain name may be already claimed. Double check it's available and [contact us](https://discord.gg/b6ZVxSZ9Hn) if you continue to have problems.**

![](../.gitbook/assets/integrations/get-test-domain/step-4-1.png)

That's it! You're the new proud owner of a `.crypto` test domain. Happy hacking!

If you have any questions, we're ready and willing to help on our developer [Discord Community](https://discord.gg/b6ZVxSZ9Hn).