# Namehashing

Namehashing is an algorithm that converts a domain name in a classical format \(like `www.example.crypto`\) to ERC721 token id. All .crypto ecosystem contracts accept domain name as a method argument in a form of ERC721 token. Namehashing is defined as a part of [EIP-137](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-137.md#namehash-algorithm) standard. See the standard for a text description of the algorithm.

One can verify his implementation of namehashing algorithm using the following reference table:

| Domain Name | ERC721 Token |
| :--- | :--- |
| `.` | `0x0000000000000000000000000000000000000000000000000000000000000000` |
| `crypto` | `0x0f4a10a4f46c288cea365fcf45cccf0e9d901b945b9829ccdb54c10dc3cb7a6f` |
| `example.crypto` | `0xd584c5509c6788ad9d9491be8ba8b4422d05caf62674a98fbf8a9988eeadfb7e` |
| `www.example.crypto` | `0x3ae54ac25ccd63401d817b6d79a4a56ae7f79a332fe77a98fa0c9d10adf9b2a1` |
| `a.b.c.crypto` | `0x353ea3e0449067382e0ea7934767470170dcfa9c49b1be0fe708adc4b1f9cf13` |

## Inverse namehashing

Fundamentally namehashing is built to be a one way operation, unless one is possessing a dictionary of all namehashes and corresponding domains. There are a few methods that can perform invert operation this way.

### JSON RPC

Crypto registry remembers all the domain names that were ever minted with their corresponding namehash: [source code](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Registry.sol#L17). That makes it possible to obtain an original domain name from a namehash via ETH RPC call to [Registry\#tokenURI](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Registry.sol#L51).

### Public API

There is a way to obtain domain information by namehash from APIs. In order to do this you need to call `https://unstoppabledomains.com/api/v1/{namehash}`

Example:

GET request: [https://unstoppabledomains.com/api/v1/0x0f4a10a4f46c288cea365fcf45cccf0e9d901b945b9829ccdb54c10dc3cb7a6f](https://unstoppabledomains.com/api/v1/0x0f4a10a4f46c288cea365fcf45cccf0e9d901b945b9829ccdb54c10dc3cb7a6f)

Response:
```
{
  "addresses": {},
  "whois": {},
  "ipfs": {},
  "gundb": {},
  "social": {},
  "meta": {
    "owner": "0x000000000000000000000000000000000000dead",
    "type": "CNS",
    "ttl": 0,
    "domain": "crypto",
    "namehash": "0x0f4a10a4f46c288cea365fcf45cccf0e9d901b945b9829ccdb54c10dc3cb7a6f"
  },
  "records": {}
}
```
