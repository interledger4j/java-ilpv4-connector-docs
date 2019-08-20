---
description: Knobs and switches to adjust before your Connector starts up.
---

# Configuration Properties

Runtime configuration of this Connector is obtained from a variety of potential sources when the connector starts up. This includes property files, environment variables, system properties and more, per the precedence defined by [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

For example, any of the following configuration settings can be overridden at runtime by setting a system or environment variable, or by supplying a runtime switch to the JVM. For example: `-Dredis.host=localhost`.

{% hint style="success" %}
We recommend creating a file called `application.yml` and putting all of your default configuration in there.
{% endhint %}

### Persistence Configuration

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

### HTTP Client Settings

#### Settlement Engine Client

Configures how all underlying HTTP clients will interact with any Settlement Engine service. Currently, the Settlement Engine client creates a default connection pool holding up to 5 idle connections which will be evicted after 5 minutes of inactivity.

{% hint style="info" %}
The root configuration key for Settlement Engine clients is: **`ilpv4.connector.settlementEngines.connectionDefaults`**
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
ilpv4:
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
The root configuration key for Settlement Engine clients is: **`ilpv4.connector.settlementEngines.ilpOverHttp`**
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
ilpv4:
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

