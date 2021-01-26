## Swift

Configuration for the [Swift resolution library](https://github.com/unstoppabledomains/resolution-swift)

### Provider URL

```swift
import UnstoppableDomainsResolution

let infuraApiKey = INFURA_PROJECT_ID

guard let resolution = try? Resolution(providerUrl: "https://mainnet.infura.io/v3/" + infuraApiKey, network: "mainnet") else {
  print ("Init of Resolution instance with custom parameters failed...")
  return
}
```
