---
description: Knobs and switches to adjust before your Connector starts up.
---

# Runtime Configuration

Runtime configuration of this Connector is obtained from a variety of potential sources when the connector starts up. This includes property files, environment variables, system properties and more, per the precedence defined by [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

For example, any of the following configuration settings can be overridden at runtime by setting a system or environment variable, or by supplying a runtime switch to the JVM. For example: `-Dredis.host=localhost`.

### Persistence Configuration

#### Redis

Redis is used to track balances for every account operated by this Connector. The following properties may be used to configure Reds:

* `redis.host`: \(Default: `localhost`\) The host that Redis is operating on.
* `redis.port`: \(Default: `6379`\) The port that Redis is operating on. 
* `redis.password`: \(Default: none\) An encrypted password String containing the password that can be used access Redis.

### HTTP Client Settings

#### Settlement Engine Client

Configures how all underlying HTTP clients will interact with any Settlement Engine service. Currently, the Settlement Engine client creates a default connection pool holding up to 5 idle connections which will be evicted after 5 minutes of inactivity.

{% hint style="info" %}
The root configuration key for Settlement Engine clients is: **`ilpv4.connector.settlementEngines.connectionDefaults`**
{% endhint %}

* **`maxIdleConnections`**: The maximum number of idle connections that the underlying OkHttp client will hold open with no traffic flowing through them \(Default: `5`_\)._
* **`keepAliveMinutes`**: The number of minutes to hold an inactive HTTP connection open before evicting the connection from the connection pool \(Default: `5 mins`\).
* **`connectTimeoutMillis`**: Applied when connecting a TCP socket to the target host. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `10000`\).
* **`readTimeoutMillis`**: Applied to both the TCP socket and for individual read IO operations. A value of 0 means no timeout, otherwise values must be between 1 `Integer#MAX_VALUE` \(Default: `10000`\).
* **`writeTimeoutMillis`**: Applied to individual write IO operations. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `10000`\).

#### Ilp-over-Http Clients

Configures how all underlying HTTP clients will interact with any peer using [Ilp-over-Http](https://github.com/interledger/rfcs/blob/master/0035-ilp-over-http/0035-ilp-over-http.md). Currently, the Ilp-over-Http client creates a default connection pool holding up to 5 idle connections which will be evicted after 5 minutes of inactivity.

{% hint style="info" %}
The root configuration key for Settlement Engine clients is: **`ilpv4.connector.settlementEngines.ilpOverHttp`**
{% endhint %}

* **`maxIdleConnections`**: The maximum number of idle connections that the underlying OkHttp client will hold open with no traffic flowing through them \(Default: `5`_\)._
* **`keepAliveMinutes`**: The number of minutes to hold an inactive HTTP connection open before evicting the connection from the connection pool \(Default: `5 mins`\).
* **`connectTimeoutMillis`**: Applied when connecting a TCP socket to the target host. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `10000`\).
* **`readTimeoutMillis`**: Applied to both the TCP socket and for individual read IO operations. A value of 0 means no timeout, otherwise values must be between 1 `Integer#MAX_VALUE` \(Default: `10000`\).
* **`writeTimeoutMillis`**: Applied to individual write IO operations. A value of 0 means no timeout, otherwise values must be between 1 and `Integer#MAX_VALUE` \(Default: `10000`\).

