---
description: Knobs and switches to adjust before your Connector starts up.
---

# Connector Configuration Properties

Runtime configuration of this Connector is obtained from a variety of potential sources when the connector starts up. This includes property files, environment variables, system properties and more, per the precedence defined by [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

For example, any of the following configuration settings can be overridden at runtime by setting a system or environment variable, or by supplying a runtime switch to the JVM. For example: `-Dredis.host=localhost`.

{% hint style="success" %}
We recommend creating a file called `application.yml` and putting all of your default configuration in there.
{% endhint %}

## Global Configuration Properties

This section details discrete Connector properties that can be configured, with examples.

### Root Properties

* `nodeIlpAddress`: ILP address of the connector. This property can be omitted if an account with relation `parent` is configured under accounts.
* `globalPrefix`: The global prefix for the Connector. For production environments, this should be `g`. For test environments, consider `test`.
* `adminPassword`: A plain-text password used to authenticate to the admin API.
* `defaultJwtTokenIssuer`: The issuer identifier for any tokens generated in accordance with [IL-RFC-38](https://github.com/interledger/rfcs/pull/559) \(i.e., `JWT_HS_256` and `JWT_RS_256`\).
* `minMessageWindowMillis`: Default \(`1000`\) The minimum time the connector wants to budget for getting a message to the accounts its trading on. Budget is mainly to cover the latency to send the fulfillment packet to the downstream node.
* `maxHoldTimeMillis`: \(Default: `30000`\) The amount of time that the Connector will wait  for a fulfillment/rejection. This is equivalent to outgoing link's timeout duration.

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
interledger:
  connector:
    # For dev purposes this is fine, but not for real use-cases. Encrypt this value instead.
    adminPassword: password
    nodeIlpAddress: test.example
    globalPrefix: test
    defaultJwtTokenIssuer: https://connector.example.com
    minMessageWindowMillis: 1000
    maxHoldTimeMillis: 30000
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### JKS Key Management Properties

The Connector supports storing keys and secrets in a [Java Keystore](https://en.wikipedia.org/wiki/Java_KeyStore) \(JKS\) file. To enable this mode, the following properties are supported:

* `enabled`: Enables or disables this keystore.
* `filename`: A filename for the JKS file \(this file needs to be on the classpath\).
* `password`: The password required to open the JKS file.
* `secret0_alias`: The alias name of they secret that is used by the Connector to encrypt, decrypt, and HMAC all other secret values.
* `secret0_password`: The password required to unlock the `secret0` alias in the Keystore.

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
interledger:
  connector:
    keystore:
      jks:
        enabled: true
        filename: crypto/crypto.p12
        # For dev purposes this is fine, but not for real use-cases. Encrypt this value instead.
        password: password
        secret0_alias: secret0
        # For dev purposes this is fine, but not for real use-cases. Encrypt this value instead.
        secret0_password: password
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### KMS Key Management Properties

The Connector supports storing keys and secrets in various Key Management Services \(KMS\). Currently, [Google Cloud KMS](https://cloud.google.com/kms/) is supported. To enable this mode, the following properties are supported:

* `enabled`: Enables or disables this keystore.
* `locationId`: The GCP [locationId](https://cloud.google.com/kms/docs/locations) for your configured KMS instance.

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
interledger:
  connector:
    keystore:
      enabled: false
      locationId: global
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Enabled Feature Properties

* `rateLimitingEnabled`: \(Default value: `true`\) Determines if rate-limiting is applied to any Connector accounts. If enabled, each account's `maxPacketsPerSecond` setting will be enforced.

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
interledger:
  connector:
    rateLimitingEnabled: true
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Enabled Protocol Properties

* `ilpOverHttpEnabled`: \(Default value: `true`\) Determines if [Ilp-over-Http](https://github.com/interledger/rfcs/blob/master/0035-ilp-over-http/0035-ilp-over-http.md) is enabled for Connector account peering.
* `peerRoutingEnabled`: \(Default: `true`\) Determines if Connector-to-Connector \([CCP](https://github.com/interledger/rfcs/pull/455)\) protocol is enabled to allow this Connector to exchange routing table information with a peer.
* `pingProtocolEnabled`: \(Default: `true`\) Determines if [ILP Ping](https://github.com/interledger/rfcs/pull/516) is enabled.
* `ildcpEnabled`: \(Default: `true`\) Determines if [IL-DCP](https://github.com/interledger/rfcs/blob/master/0031-dynamic-configuration-protocol/0031-dynamic-configuration-protocol.md) is enabled.

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
interledger:
  connector:
    enabledProtocols:
      ilpOverHttpEnabled: true
      peerRoutingEnabled: true
      pingProtocolEnabled: true
      ildcpEnabled: true
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Persistence Configuration Properties

#### Redis

Redis is used to track balances for every account operated by this Connector. The following properties may be used to configure Reds:

* `spring.redis.host`: \(Default: `localhost`\) The host that Redis is operating on.
* `spring.redis.port`: \(Default: `6379`\) The port that Redis is operating on. 
* `spring.redis.password`: \(Default: none\) An encrypted password String containing the password that can be used access Redis.

In the `application.yml` file, a sample configuration might look like this:

```yaml
spring:
  redis:
    host=localhost
    port=6379
    password=enc:JKS:crypto.p12:redis_pw:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=
```

{% hint style="danger" %}
The Redis password should be encrypted, especially if it will reside in a property file per the above example. To generate this encrypted value, you can use the Connector Crypto CLI. 

For more details, read more in [Connector Crypto](../security-guide/crypto.md).
{% endhint %}

#### Postgres Configuration

Postgres can be used to store all non-balance tracking information, including account settings, routing tables, FX rates, and more.

* `spring.datasource.url`: The datasource URL used to connect to a Postgres instance.

In addition, the following two properties can be used to supply the Connector with Authentication credentials to connect to the database:

* `spring.datasource.username` \(Default: `postgres`\) The username to connect to the database as.
* `spring.datasource.password` The password to connect to the database as.

In the `application.yml` file, a sample configuration might look like this:

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/connector_db
    username: postgres
    password: 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="danger" %}
If no security credentials are not required by your database, then the `username` and `password` properties may be omitted. **However, such a configuration is not recommended**.
{% endhint %}

### HTTP Client Properties

#### Settlement Engine Client

Configures how all underlying HTTP clients will interact with any Settlement Engine service. Currently, the Settlement Engine client creates a default connection pool holding up to 5 idle connections which will be evicted after 5 minutes of inactivity.

{% hint style="info" %}
The root configuration key for Settlement Engine clients is: **`interledger.connector.settlementEngines.connectionDefaults`**
{% endhint %}

* **`maxIdleConnections`**: The maximum number of idle connections that the underlying OkHttp client will hold open with no traffic flowing through them \(Default: `5`_\)._
* **`keepAliveMinutes`**: The number of minutes to hold an inactive HTTP connection open before evicting the connection from the connection pool \(Default: `5 mins`\).
* **`connectTimeoutMillis`**: Applied when connecting a TCP socket to the target host. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `10000`\).
* **`readTimeoutMillis`**: Applied to both the TCP socket and for individual read IO operations. A value of 0 means no timeout, otherwise values must be between 1 `Integer#MAX_VALUE` \(Default: `30000`\).
* **`writeTimeoutMillis`**: Applied to individual write IO operations. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `30000`\).

In the `application.yml` file, a sample configuration might look like this:

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
interledger:
  connector:
    settlementEngines:
      connectionDefaults:
        maxIdleConnections: 5
        keepAliveMinutes: 5
        connectTimeoutMillis: 10000
        readTimeoutMillis: 10000
        writeTimeoutMillis: 30000
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### Ilp-over-Http Clients

Configures how all underlying HTTP clients will interact with any peer using [Ilp-over-Http](https://github.com/interledger/rfcs/blob/master/0035-ilp-over-http/0035-ilp-over-http.md). Currently, the Ilp-over-Http client creates a default connection pool holding up to 5 idle connections which will be evicted after 5 minutes of inactivity.

{% hint style="info" %}
The root configuration key for Settlement Engine clients is: **`interledger.connector.settlementEngines.ilpOverHttp`**
{% endhint %}

* **`maxIdleConnections`**: The maximum number of idle connections that the underlying OkHttp client will hold open with no traffic flowing through them \(Default: `5`_\)._
* **`keepAliveMinutes`**: The number of minutes to hold an inactive HTTP connection open before evicting the connection from the connection pool \(Default: `5 mins`\).
* **`connectTimeoutMillis`**: Applied when connecting a TCP socket to the target host. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `10000`\).
* **`readTimeoutMillis`**: Applied to both the TCP socket and for individual read IO operations. A value of 0 means no timeout, otherwise values must be between 1 `Integer#MAX_VALUE` \(Default: `60000`\).
* **`writeTimeoutMillis`**: Applied to individual write IO operations. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `60000`\).

In the `application.yml` file, a sample configuration might look like this:

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
interledger:
  connector:
    ilpOverHttp:
      connectionDefaults:
        maxIdleConnections: 5
        keepAliveMinutes: 5
        connectTimeoutMillis: 10000
        readTimeoutMillis: 10000
        writeTimeoutMillis: 30000
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### FX Http Client Properties

Configures how all underlying HTTP clients for Foreign Exchange \(FX\) will interact with any peer. Currently, the FX client creates a default connection pool holding up to 5 idle connections which will be evicted after 5 minutes of inactivity.

{% hint style="info" %}
The root configuration key for Http FX clients is: **`interledger.connector.fx`**
{% endhint %}

* **`maxIdleConnections`**: The maximum number of idle connections that the underlying OkHttp client will hold open with no traffic flowing through them \(Default: `5`_\)._
* **`keepAliveMinutes`**: The number of minutes to hold an inactive HTTP connection open before evicting the connection from the connection pool \(Default: `5 mins`\).
* **`connectTimeoutMillis`**: Applied when connecting a TCP socket to the target host. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `10000`\).
* **`readTimeoutMillis`**: Applied to both the TCP socket and for individual read IO operations. A value of 0 means no timeout, otherwise values must be between 1 `Integer#MAX_VALUE` \(Default: `60000`\).
* **`writeTimeoutMillis`**: Applied to individual write IO operations. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `60000`\).

In the `application.yml` file, a sample configuration might look like this:

{% code-tabs %}
{% code-tabs-item title="application.yml" %}
```yaml
interledger:
  connector:
    fx:
      connectionDefaults:
        maxIdleConnections: 5
        keepAliveMinutes: 5
        connectTimeoutMillis: 10000
        readTimeoutMillis: 10000
        writeTimeoutMillis: 30000
```
{% endcode-tabs-item %}
{% endcode-tabs %}

