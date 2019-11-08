---
description: >-
  Covers everything you need to configure and startup a Connector in an initial
  state.
---

# Starting the Connector

## Default Configuration

In order to startup the Connector, you'll need a minimal one, like this:

{% tabs %}
{% tab title="application.yml" %}
```yaml
logging:
  level:
    ROOT: INFO
    com.sappenin: INFO
    org.interledger: INFO
    org.springframework: INFO
    org.springframework.security: INFO
server:
  port: 8080
  http2:
    enabled: true
  jetty:
    max-http-post-size: 32767

spring:
  output:
    ansi:
      enabled: ALWAYS
  http:
    multipart:
      enabled: false
      max-file-size: 32KB
      max-request-size: 32KB
  resources:
    add-mappings: false
  mvc:
    throw-exception-if-no-handler-found: true
  jpa:
    properties:
      hibernate:
        temp:
          use_jdbc_metadata_defaults: false

interledger:
  connector:
    adminPassword: password
    keystore:
      jks:
        enabled: true
        filename: crypto/crypto.p12
        password: password
        secret0_alias: secret0
        secret0_password: password
    enabledProtocols:
      blastEnabled: true
      pingProtocolEnabled: true
      peerConfigEnabled: true
      peerRoutingEnabled: true
    defaultJwtTokenIssuer: https://connie.example.com
    nodeIlpAddress: test.connie
    globalRoutingSettings:
      # Represents the plaintext value of `shh`, encrypted.
      routingSecret: enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=
```
{% endtab %}
{% endtabs %}

Next, startup the Connector using this command:

```text
java -jar ilpv4-connector-server-0.1.0-SNAPSHOT-exec.jar
```

{% hint style="success" %}
Success! You're Connector is now running.
{% endhint %}

{% hint style="danger" %}
While this tutorial helps you start a minimally configured Connector, for actual usage, you should configure persistent data storage and balance tracking. For more details, see the [Persistence](../operating-a-connector/ilpv4-connector-persistence.md) page.
{% endhint %}



