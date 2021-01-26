## Java

Configuration for the [Java resolution library](https://github.com/unstoppabledomains/resolution-java)

### Provider URL

```java
import com.unstoppabledomains.resolution.Resolution

String infuraApiKey = INFURA_PROJECT_ID;
String providerURL = "https://mainnet.infura.io/v3/" + infuraApiKey

DomainResolution resolution = Resolution.builder()
                .providerUrl(NamingServiceType.CNS, providerURL)
                .build();
```
