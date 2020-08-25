# Meta Transactions

Most Registry and Resolver methods have a [meta-transaction](https://docs.openzeppelin.com/learn/sending-gasless-transactions) support. Generally meta-transactions allow to separate a transaction signer address from a transaction funding address. So an address used to generate a meta-signature is used to check a permission for an operation and classical signer address is used to withdraw a transaction fee.

For each management method, there is a method with meta-transaction support that has `For` suffix at the end. Example: `resetFor` is a meta-transaction version of `reset`. This method has an additional `signature` argument as a last parameter beside all original parameters. A meta-transaction method checks the permission for a domain against the address that generated the signature argument, unlike base method that checks it against Solidity `_sender` keyword.

Note that a meta-transaction version of a function of the Registry can be implemented in controller contract but not registry itself. See [Registry Controllers](https://github.com/unstoppabledomains/dot-crypto/blob/master/ARCHITECTURE.md#registry-controllers).

Meta transaction methods are bound to domain based nonce \(instead of [Account Nonce](https://ethereum.stackexchange.com/questions/27432/what-is-nonce-in-ethereum-how-does-it-prevent-double-spending) of traditional transactions\). It protects from [Double-spending](https://en.wikipedia.org/wiki/Double-spending) in the same way as account based nonce in traditional transactions.

A source code of signature validation can be found in [SignatureUtil.sol](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/utils/SignatureUtil.sol)

## **Meta transaction signature generation**

Meta transaction requires 2 signature: one passed as a method argument and one classical. Classical signature is generated in a standard way. Meta signature requires a domain owner \(or a person approved by owner\) to sign a special message formed from:

* Domain based meta-transaction nonce
* [Function Selector](https://solidity.readthedocs.io/en/v0.7.0/abi-spec.html#function-selector) of the original method
* Original Method parameters \(the one without signature\)

Example Signature generation for a `reset` method call for a domain:

```text
const domain = 'example.crypto';
const methodName = 'reset';
const methodParams = ['uint256'];
const contractAddress = '0xb66DcE2DA6afAAa98F2013446dBCB0f4B0ab2842';
// Can be different or the same as contractAddress
const controllerContractAddress = '0xb66DcE2DA6afAAa98F2013446dBCB0f4B0ab2842';
const tokenId = namehash(domain);

function generateMessageToSign(
  contractAddress: string,
  signatureContract: string,
  methodName: string,
  methodParams: string[],
  tokenId: string,
  params: any[],
) {
  return solidityKeccak256(
    ['bytes32', 'address', 'uint256'],
    [
      solidityKeccak256(
        ['bytes'],
        [encodeContractInterface(contractAddress, method, methodParams, params)],
      ),
      controllerContractAddress,
      ethCallRpc(controllerContractAddress, 'nonceOf', tokenId),
    ],
  );
}

const message = generateMessageToSign(
  contractAddress,
  signagureContractAddress,
  methodName,
  methodParams,
  tokenId,
  [tokenId]
);
```

Functions Reference:

* `namehash` - [Namehashing Function](../domain-registry-essentials/namehashing.md) algorithm implementation
* `ethCallRpc` - Ethereum `eth_call` JSON RPC implementation
* `encodeContractInterface` - [Solidity ABI](https://solidity.readthedocs.io/en/v0.7.0/abi-spec.html#argument-encoding) interface parameters encoder
* `solidityKeccak256` - [Solidity ABI](https://solidity.readthedocs.io/en/v0.7.0/abi-spec.html#argument-encoding) parameters encoder

