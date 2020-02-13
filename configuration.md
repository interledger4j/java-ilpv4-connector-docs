---
description: Knobs and switches to adjust before your Connector starts up.
---

# Connector Configuration Properties

Runtime configuration of this Connector can be obtained from a variety of potential sources when the connector starts up. This includes property files, environment variables, system properties and more, per the precedence defined by [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

For example, any of the following configuration settings can be overridden at runtime by setting a system or environment variable, or by supplying a runtime switch to the JVM. For example: **`-Dredis.host=localhost`**.

{% hint style="info" %}
For advanced configurations with many overrides, it is recommend to place a file called **`application.yml`** onto the classpath.  Any values defined in this file will override the default values packaged with the application binary.
{% endhint %}

## Global Configuration Properties

This section details discrete Connector properties that can be configured, with examples.

### Root Properties

{% hint style="info" %}
The configuration property prefix for root properties is**`interledger.connector`**
{% endhint %}

* **Required Properties**
  * **`nodeIlpAddress`**: ILP address of the connector. This property can be omitted if an account with relation `parent` is configured using the [Admin API](api-references/admin-api.md).
  * **`adminPassword`**: A plain-text password used to authenticate to the admin API.
* **Optional Properties**
  * **`globalPrefix`**: The global prefix for the Connector. For production environments, this should be `g`. For test environments, consider `test`.\(**Default**: `g`\).
  * **`defaultJwtTokenIssuer`**: The issuer identifier for any tokens generated in accordance with [HTTP Auth Profiles RFC Proposal](https://github.com/interledger/rfcs/blob/master/proposals/0000-http-auth-profiles.md) \(i.e., `JWT_HS_256` and `JWT_RS_256`\).
  * **`minMessageWindowMillis`**: Default \(`1000`\) The minimum time the connector wants to budget for getting a message to the accounts its trading on. Budget is mainly to cover the latency to send the fulfillment packet to the downstream node \(**Default**: `1000`\).
  * **`maxHoldTimeMillis`**:  The amount of time that the Connector will wait for a fulfillment/rejection. This value is used to set any outgoing link's timeout duration \(**Default**: `30000`\).

{% code title="application.yml" %}
```yaml
interledger:
  connector:
    nodeIlpAddress: test.example
    adminPassword: shh # For prod usage, encrypt this value instead.
    globalPrefix: test
    defaultJwtTokenIssuer: https://connector.example.com
    minMessageWindowMillis: 1000
    maxHoldTimeMillis: 30000
```
{% endcode %}

### Properties: Global Routing Settings

{% hint style="info" %}
The configuration property prefix for Global Routing Settings is**`interledger.connector.globalRoutingSettings`**
{% endhint %}

* **`routingSecret`**: A 32-byte secret seed value used to authenticate routing-table updates. If unspecified, an ephemeral random set of bytes will be generated and used.
* **`localAccountAddressSegment`**: An ILP address segment that will be used to route packets to any local accounts defined in the Connector. For example, if an account exists in the Connector with id of `alice`, then nodes wanting to send packets to that account would use the ILP address `{connector-operator-address}.accounts.alice`. Typically this will be used by the Connector to support IL-DCP, but will also be used to make routing decisions for any local accounts that might connect to the Connector. For example, `g.connector.accounts.alice.bob` would route to `alice`, allowing that node to figure out how to route to "bob" \(**Default**: `accounts`\).
* **`defaultRoute`**: An optionally-defined accountId that should be used as the default route for all un-routed traffic. If empty, the default route is disabled.
* **`routeBroadcastEnabled`**: Whether to broadcast known routes \(**Default**: `true`\).
* **`useParentForDefaultRoute`**: Determines if the parent-account should be used as the default route \(**Default**: `false`\).
* **`routeBroadcastInterval`**: Frequency, in milliseconds, at which the connector broadcasts its routes to adjacent connectors \(**Default**: `30000`\).
* **`routeCleanupInterval`**: The frequency, in milliseconds, at which the connector checks for expired routes \(**Default**: `30000`\).
* **`routeExpiry`**: The maximum age, in milliseconds, of a route provided by this connector \(**Default**: `30000`\).
* **`maxEpochsPerRoutingTable`**: The maximum number of epochs per routing table update \(**Default**: `50`\).

```yaml
interledger:
  connector:
    globalRoutingSettings:
      routingSecret: shh # For prod usage, encrypt this value instead.
      routeBroadcastEnabled: true
      useParentForDefaultRoute: true
      defaultRoute: parent-account
      routeBroadcastInterval: 30000
      routeCleanupInterval: 1000
      routeExpiry: 30000
      maxEpochsPerRoutingTable: 50
```

### Properties: Enabled Features

{% hint style="info" %}
The configuration property prefix for Enabled Features is**`interledger.connector.enabledFeatures`**
{% endhint %}

* **`require32ByteSharedSecrets`**: Ensure that any shared secret values are at least 32 bytes, for security purposes. In non-production modes, this setting may be set to `false`, but in general it should always be `true` . \(**Default** `true`\).
* **`rateLimitingEnabled`**: Determines if rate-limiting is applied to any Connector accounts. If enabled, each account's `maxPacketsPerSecond` setting will be enforced \(**Default**: `true`\).
* **`localSpspFulfillmentsEnabled`** : Determines if this Connector will attempt to fulfill SPSP payments locally as opposed to forwarding them out to an upstream peer \(**Default**: `false`\). See "[Properties: Local SPSP Fulfillment](configuration.md#properties-http-clients)" for more details.

{% code title="application.yml" %}
```yaml
interledger:
  connector:
    enabledFeatures:
      require32ByteSharedSecrets: true
      rateLimitingEnabled: true
      localSpspFulfillmentsEnabled: true
```
{% endcode %}

### Properties: Enabled Protocols

* **`ilpOverHttpEnabled`**: Determines if [Ilp-over-Http](https://github.com/interledger/rfcs/blob/master/0035-ilp-over-http/0035-ilp-over-http.md) is enabled for Connector account peering \(Default: **`true`**\).
* **`peerRoutingEnabled`**: Determines if Connector-to-Connector \([CCP](https://github.com/interledger/rfcs/pull/455)\) protocol is enabled to allow this Connector to exchange routing table information with a peer \(Default: **`true`**\).
* **`pingProtocolEnabled`**: Determines if [ILP Ping](https://github.com/interledger/rfcs/pull/516) is enabled \(Default: **`true`**\).
* **`ildcpEnabled`**: Determines if [IL-DCP](https://github.com/interledger/rfcs/blob/master/0031-dynamic-configuration-protocol/0031-dynamic-configuration-protocol.md) is enabled \(Default: **`true`**\).

{% code title="application.yml" %}
```yaml
interledger:
  connector:
    enabledProtocols:
      ilpOverHttpEnabled: true
      peerRoutingEnabled: true
      pingProtocolEnabled: true
      ildcpEnabled: true
```
{% endcode %}

### Properties: JKS Key Management

{% hint style="info" %}
The configuration property prefix for JKS properties is**`interledger.connector.keystore.jks`**
{% endhint %}

The Connector supports storing keys and secrets in a [Java Keystore](https://en.wikipedia.org/wiki/Java_KeyStore) \(JKS\) file. To enable this mode, the following properties are supported:

* **`enabled`**: Enables or disables this keystore.
* **`filename`**: A filename for the JKS file \(this file needs to be on the classpath\).
* **`password`**: The password required to open the JKS file.
* **`secret0_alias`**: The alias name of they secret that is used by the Connector to encrypt, decrypt, and HMAC all other secret values.
* **`secret0_password`**: The password required to unlock the `secret0` alias in the Keystore.

{% code title="application.yml" %}
```yaml
interledger:
  connector:
    keystore:
      jks:
        enabled: true
        filename: crypto/crypto.p12
        password: password # For prod usage, encrypt this value instead.
        secret0_alias: secret0
        secret0_password: password # For prod usage, encrypt this value instead.
```
{% endcode %}

### Properties: KMS Key Management

{% hint style="info" %}
The configuration property prefix for Google KMS properties is**`interledger.connector.keystore.gcpkms`**
{% endhint %}

The Connector supports storing keys and secrets in various Key Management Services \(KMS\). Currently, [Google Cloud KMS](https://cloud.google.com/kms/) is supported. To enable this mode, the following properties are supported:

* **`enabled`**: Enables or disables this keystore.
* **`locationId`**: The GCP [locationId](https://cloud.google.com/kms/docs/locations) for your configured KMS instance.

{% code title="application.yml" %}
```yaml
interledger:
  connector:
    keystore:
      gcpkms:
        enabled: false
        locationId: global
```
{% endcode %}

### Properties: Persistence Configuration 

#### Redis

Redis is used to track balances for every account operated by this Connector. The following properties may be used to configure Reds:

* **`spring.redis.host`**: \(Default: `localhost`\) The host that Redis is operating on.
* **`spring.redis.port`**: \(Default: `6379`\) The port that Redis is operating on. 
* **`spring.redis.password`**: \(Default: none\) An encrypted password String containing the password that can be used access Redis.

In the `application.yml` file, a sample configuration might look like this:

```yaml
spring:
  redis:
    host: localhost
    port: 6379
    password: enc:JKS:crypto.p12:redis_pw:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=
```

{% hint style="danger" %}
The Redis password should be encrypted, especially if it will reside in a property file per the above example. To generate this encrypted value, you can use the Connector Crypto CLI. 

For more details, read more in [Connector Crypto](security-guide/crypto.md).
{% endhint %}

#### Postgres Configuration

Postgres can be used to store all non-balance tracking information, including account settings, routing tables, FX rates, and more.

* **`spring.datasource.url`**: The datasource URL used to connect to a Postgres instance.

In addition, the following two properties can be used to supply the Connector with Authentication credentials to connect to the database:

* **`spring.datasource.username`** \(Default: **`postgres`**\) The username to connect to the database as.
* **`spring.datasource.password`** The password to connect to the database as.

In the `application.yml` file, a sample configuration might look like this:

{% code title="application.yml" %}
```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/connector_db
    username: postgres
    password: 
```
{% endcode %}

{% hint style="danger" %}
If security credentials are not required by your database, then the `username` and `password` properties may be omitted. **However, such a configuration is not recommended**.
{% endhint %}

### Properties: HTTP Clients

#### Settlement Engine Client

Configures how all underlying HTTP clients will interact with any Settlement Engine service. Currently, the Settlement Engine client creates a default connection pool holding up to 5 idle connections which will be evicted after 5 minutes of inactivity.

{% hint style="info" %}
The configuration property prefix for Settlement Engine clients is: **`interledger.connector.settlementEngines.connectionDefaults`**
{% endhint %}

* **`maxIdleConnections`**: The maximum number of idle connections that the underlying OkHttp client will hold open with no traffic flowing through them \(Default: `5`_\)._
* **`keepAliveMinutes`**: The number of minutes to hold an inactive HTTP connection open before evicting the connection from the connection pool \(Default: `5 mins`\).
* **`connectTimeoutMillis`**: Applied when connecting a TCP socket to the target host. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `10000`\).
* **`readTimeoutMillis`**: Applied to both the TCP socket and for individual read IO operations. A value of 0 means no timeout, otherwise values must be between 1 `Integer#MAX_VALUE` \(Default: `30000`\).
* **`writeTimeoutMillis`**: Applied to individual write IO operations. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `30000`\).

In the `application.yml` file, a sample configuration might look like this:

{% code title="application.yml" %}
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
{% endcode %}

#### ILP-over-HTTP Clients

Configures how all underlying HTTP clients will interact with any peer using [Ilp-over-Http](https://github.com/interledger/rfcs/blob/master/0035-ilp-over-http/0035-ilp-over-http.md). Currently, the Ilp-over-Http client creates a default connection pool holding up to 5 idle connections which will be evicted after 5 minutes of inactivity.

{% hint style="info" %}
The configuration property prefix for ILP-over-HTTP clients is: **`interledger.connector.settlementEngines.ilpOverHttp`**
{% endhint %}

* **`maxIdleConnections`**: The maximum number of idle connections that the underlying OkHttp client will hold open with no traffic flowing through them \(Default: `5`_\)._
* **`keepAliveMinutes`**: The number of minutes to hold an inactive HTTP connection open before evicting the connection from the connection pool \(Default: `5 mins`\).
* **`connectTimeoutMillis`**: Applied when connecting a TCP socket to the target host. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `10000`\).
* **`readTimeoutMillis`**: Applied to both the TCP socket and for individual read IO operations. A value of 0 means no timeout, otherwise values must be between 1 `Integer#MAX_VALUE` \(Default: `60000`\).
* **`writeTimeoutMillis`**: Applied to individual write IO operations. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `60000`\).
* **`maxRequests`** : The maximum number of HTTP requests to execute concurrently. Above this, requests queue in memory, waiting for the running calls to complete \(Default: `100`\).
* **`maxRequestsPerHost`**: The maximum number of requests for each host to execute concurrently. This limits requests by the URL's host name. Note that concurrent requests to a single IP address may still exceed this limit: multiple hostnames may share an IP address or be routed through the same HTTP proxy. \(Default: `50`\).

In the `application.yml` file, a sample configuration might look like this:

{% code title="application.yml" %}
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
        maxRequests: 100
        maxRequestsPerHost: 50
```
{% endcode %}

#### FX HTTP Clients

Configures how all underlying HTTP clients for Foreign Exchange \(FX\) will interact with any peer. Currently, the FX client creates a default connection pool holding up to 5 idle connections which will be evicted after 5 minutes of inactivity.

{% hint style="info" %}
The configuration property prefix for Http FX clients is:**`interledger.connector.fx`**
{% endhint %}

* **`maxIdleConnections`**: The maximum number of idle connections that the underlying OkHttp client will hold open with no traffic flowing through them \(Default: **`5`**_\)._
* **`keepAliveMinutes`**: The number of minutes to hold an inactive HTTP connection open before evicting the connection from the connection pool \(Default: **`5 mins`**\).
* **`connectTimeoutMillis`**: Applied when connecting a TCP socket to the target host. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: **`10000`**\).
* **`readTimeoutMillis`**: Applied to both the TCP socket and for individual read IO operations. A value of 0 means no timeout, otherwise values must be between 1 `Integer#MAX_VALUE` \(Default: **`60000`**\).
* **`writeTimeoutMillis`**: Applied to individual write IO operations. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: **`60000`**\).

In the `application.yml` file, a sample configuration might look like this:

{% code title="application.yml" %}
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
{% endcode %}

### Properties: Keys and shared secrets

{% hint style="info" %}
The configuration property prefix for keys/secrets is:**`interledger.connector.keys`**
{% endhint %}

The connector uses key aliases and versions to determine what to use when handling encrypted shared secrets such as those for incoming and outgoing account settings links.

Following keys are configurable:

* **`secret0`**: Master encryption key for the connector.
* **`accountSettings`**: Encryption key for account settings shared secrets.

By default, the connector requires the shared secrets to be at least 32 bytes. To remove this requirement in non-production environments so that any secret length is allowed, set the following property to false:

* **`require32ByteSharedSecrets`**: Set to **`false`** to allow smaller shared-secrets. \(Default: **`true`**\).

In the `application.yml` file, a sample configuration might look like this.

```yaml
application.yml

interledger:
  connector:
    require32ByteSharedSecrets: false
    keys:
      secret0:
        alias: secret0
        version: 1
      accountSettings:
        alias: accounts
        version: 1
```

The connector makes use of keys to encrypt and decrypt shared secrets. By default, the plain text value of a shared secret must be 32 bytes but this can be 

### Properties: GCP Pub/Sub

{% hint style="info" %}
The configuration property prefix for GCP Pub/Sub configuration is: **`interledger.connector.pubsub`**
{% endhint %}

Publishing fulfillment and/or rejection packets to GCP Pub/Sub can be enabled by setting the following properties:

* **`spring.cloud.gcp.credentials.location`**: Path to GCP service account JSON. This service account should have permissions to publish to pubsub.
* **`spring.cloud.gcp.credentials.encoded_key`**: An alternative way to provide the service account JSON as a base-64 encoded string instead of as a file location. For example, on linux, using the output of the command `cat /path/to/credentials.json | base64`
* **`spring.cloud.gcp.project_id`**: Your GCP project id.
* **`spring.cloud.gcp.pubsub.enabled`** : `true` or `false` depending on whether pubsub is enabled \(Default: `false`\).
* **`interledger.connector.pubsub.topics.fulfillment_event`**: The GCP pub/sub topic name where fulfillment events should be published. If not set, fulfillments will not be published \(Default: `ilp-events`\).
* **`interledger.connector.pubsub.topics.rejection_event`**: The GCP pub/sub topic name where rejection events should be published. If not set, rejections will not be published. Note: you can publish both fulfillments and rejections to the same topic by using the same name \(Default: `ilp-events`\).

```yaml
interledger:
  connector:
    pubsub:
      topics:
        fulfillment-event: ilp-events
        rejection-event: ilp-events
        
spring:
  cloud:
    gcp:
      ## must set project-id and credentials
      project-id: {your-project-id}
      credentials:
        encoded-key: {your-base64-encoded-service-account-key-json}
      pubsub:
        enabled: true
```

### Properties: Local SPSP Payment Fulfillment

{% hint style="info" %}
The configuration property prefix for Local SPSP Payment Fulfillment configuration is: **`interledger.connector.spsp`**
{% endhint %}

The Connector can be configured to fulfill SPSP payments that are destined for accounts in the Connector itself. This functionality can be enabled via the following properties:

* **`server-secret`** A base64-encoded String of bytes used to derive any values used in encryption/decryption for SPSP.
* **`addressPrefixSegment`**: A string that will be appended to the Connector's operating address in order to form an address prefix that the Connector will key off for fulfilling SPSP payments locally \(Default: `spsp`\).

```yaml
interledger:
  connector:
    enabledFeatures:
      localSpspFulfillmentEnabled: true
    spsp:
      # For prod usage, encrypt this value instead.
      serverSecret: aQLR5IWAGV2vKnBhnFFsl2cXOCh9u0IWz3PiA64KlJ8= 
      addressPrefixSegment: spsp
```

### Aggregate Profiles

Several Spring profiles are available to make it easier to enable certain features. Some commonly used profiles are:

* \*\*\*\*[**dev**](https://github.com/interledger4j/ilpv4-connector/tree/master/connector-server/src/main/resources/application-dev.yml)**:** single profile that includes other profiles for easily starting up a connector for dev purposes
* \*\*\*\*[**migrate**](https://github.com/interledger4j/ilpv4-connector/tree/master/connector-server/src/main/resources/application-migrate.yml): runs database migrations \(via liquibase\) before connector is started
* \*\*\*\*[**migrate-only**](https://github.com/interledger4j/ilpv4-connector/tree/master/connector-server/src/main/resources/application-migrate-only.yml): only runs database migrations \(via liquibase\) but does not start the connector \(application will terminate after migrations complete\)
* \*\*\*\*[**management**](https://github.com/interledger4j/ilpv4-connector/tree/master/connector-server/src/main/resources/application-management.yml): enables the Spring management endpoints
* \*\*\*\*[**h2**](https://github.com/interledger4j/ilpv4-connector/tree/master/connector-server/src/main/resources/application-h2.yml): enables hypersonic in-memory SQL database
* \*\*\*\*[**postgres**](https://github.com/interledger4j/ilpv4-connector/tree/master/connector-server/src/main/resources/application-postgres.yml)**:** enables postgres driver with defaults \(must override url, username, pw\)

A complete list of profiles can be found [here](https://github.com/interledger4j/ilpv4-connector/tree/master/connector-server/src/main/resources/).

These profiles can be enabled by from the command-line using `-Dspring.profiles.active=h2,management,...`, via an environment variable `SPRING_PROFILES_ACTIVE=h2,management,...` or by adding the following to your application.yaml:

{% code title="application.yaml" %}
```yaml
spring:
  profiles:
    active: h2,management,...
```
{% endcode %}

