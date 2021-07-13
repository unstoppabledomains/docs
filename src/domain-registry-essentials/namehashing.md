# Namehashing

Namehashing is an algorithm that converts a domain name in a classical format \(like `www.example.crypto`\) to ERC-721 token id. All `.crypto` ecosystem contracts accept a domain name as a method argument in the form of an ERC-721 token. Namehashing is defined as a part of the [EIP-137](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-137.md#namehash-algorithm) standard. See the standard for a text description of the algorithm.

To verify an implementation of the namehash algorithm, use the following reference table:

| Domain Name | ERC721 Token |
| :--- | :--- |
| `.` | `0x0000000000000000000000000000000000000000000000000000000000000000` |
| `crypto` | `0x0f4a10a4f46c288cea365fcf45cccf0e9d901b945b9829ccdb54c10dc3cb7a6f` |
| `example.crypto` | `0xd584c5509c6788ad9d9491be8ba8b4422d05caf62674a98fbf8a9988eeadfb7e` |
| `www.example.crypto` | `0x3ae54ac25ccd63401d817b6d79a4a56ae7f79a332fe77a98fa0c9d10adf9b2a1` |
| `a.b.c.crypto` | `0x353ea3e0449067382e0ea7934767470170dcfa9c49b1be0fe708adc4b1f9cf13` |

## Reverse lookup

Fundamentally, namehashing is a one-way operation. It recursively hashes the labels using the SHA-256 hash function.

If one possesses a precomputed table of all hashes and corresponding domains reverse lookups are possible. This table can be reconstructed using the events on the CNS Registry `NewURI` event.

### JSON RPC

The `.crypto` registry tracks all domain names with their corresponding namehash: [source code](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Registry.sol#L17). That makes it possible to obtain an original domain name from a namehash via ETH RPC call to [Registry\#tokenURI](https://github.com/unstoppabledomains/dot-crypto/blob/master/contracts/Registry.sol#L51).

### Public API

Unstoppable Domains maintains a public-facing API which can be used to obtain domain information with a namehash. To do this, call `https://unstoppabledomains.com/api/v1/{namehash}`

Example request:

```bash
$ curl https://unstoppabledomains.com/api/v1/0x0f4a10a4f46c288cea365fcf45cccf0e9d901b945b9829ccdb54c10dc3cb7a6f
```

Example Response:

```javascript
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

