# Get test domain
To decrease complexity of integration Unstoppable allows you to get `.crypto` domain for free. Any test domain comes with `udtestdev-` prefix. For example:  
- `udtestdev-freedomainisawesome.crypto`
- `udtestdev-test-wallet-integration.crypto`

## Prerequirements
- [MetaMask](https://metamask.io/) or any wallet with [WalletConnect](https://walletconnect.org/wallets) support
- You need to have ether on your Ethereum wallet to pay transaction fees. 
- To calculate transaction fees use [ETH Gas Station](https://ethgasstation.info/calculatorTxV.php)
- Average transaction gas usage is 180000

## Get your domain using Etherscan 
-  Open the Smart Contract using [Etherscan](https://etherscan.io/address/0x1fC985cAc641ED5846b631f96F35d9b48Bc3b834#writeContract)

![](../.gitbook/assets/integrations/get-test-domain/step-1.png)

- Connect your wallet

![](../.gitbook/assets/integrations/get-test-domain/step-2.png)
![](../.gitbook/assets/integrations/get-test-domain/step-2-1.png)

- Come up with your unique domain name (label) and press “Write” button to initiate transaction

![](../.gitbook/assets/integrations/get-test-domain/step-3.png)

- Sign transaction. 

![](../.gitbook/assets/integrations/get-test-domain/step-4.png)

**IMPORTANT: If transaction takes too much gas (more that 200000 for calling claim method) or failing - a domain name is already claimed and you can try another one.**

![](../.gitbook/assets/integrations/get-test-domain/step-4-1.png)

That's it. Now you’re a proud owner of `.crypto` test domain (`udtestdev-test.crypto` in our case). Happy hacking!
