---
description: 'Connect to a peer using ILP, HTTP, and some love...'
---

# Peering: ILP-over-HTTP

## Overview

This guide instructs you how to setup a peering relationship between your Connector and a peer. In addition, you'll also be able to verify connectivity in both directions using the ILP Ping.

This guide assumes you're operating the `test.alice` Connector. Once completed, your ILP network topology will look like this:

![](.gitbook/assets/ilp-over-http%20%283%29.svg)

## Considerations

In order to create an actual Interledger peering relationship, you need to first determine the financial details of the relationship. This includes:

* The amount of credit to extend to your peer. This will be dictated by the `min_balance` value.
* The currency unit you will use to denominate the account relationship. This guide will use `USD` in all examples.
* Whether or not you want to rate-limit your peer.
* The network connection details you will use to communicate with your peer. This guide assumes ILP-over-HTTP, and as such will specify both incoming and outgoing Link settings using HTTP.

{% hint style="danger" %}
 This guide does not enable settlement. Read more in the [Settlement](concepts/settlement-xrp-ledger.md) section for conceptual details, and consult [Settlement: XRP Leger](settlement-xrp-ledger.md) for an example.
{% endhint %}

{% hint style="success" %}
Reference the [Connector Admin API](api-references/admin-api.md) documentation for more details about actual payloads and their meanings.
{% endhint %}

## Accounts on the Alice Connector

On the `test.alice` Connector, we need to create two accounts: one for a hypothetical user named **Peter** and one for the peering relationship with the **Bob** Connector.

### Create Account: "bob"

In order to peer with the Bob Connector, we must first tell Alice about Bob. This is done by creating an account for Bob, on Alice. In this case, we want to use a relatively strong token scheme for security purposes, called `JWT_HS_256` \(See [ILP-over-HTTP Auth Profiles](https://github.com/interledger/rfcs/blob/master/proposals/0000-http-auth-profiles.md) for more details\). To effect this change, we can use the Connector Admin API by making the following call:

```javascript
curl --location --request POST 'http://alice.example.com/accounts' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data-raw '{
  "accountId": "bob",
  "accountRelationship": "PEER",
  "linkType": "ILP_OVER_HTTP",
  "assetCode": "USD",
  "assetScale": "3",
  "customSettings": {
    "ilpOverHttp.incoming.auth_type": "JWT_HS_256",
    "ilpOverHttp.incoming.token_subject":"bob",
    "ilpOverHttp.incoming.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",    	
    "ilpOverHttp.outgoing.auth_type": "JWT_HS_256",
    "ilpOverHttp.outgoing.token_subject": "alice",
    "ilpOverHttp.outgoing.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",
    "ilpOverHttp.outgoing.url": "https://bob.example.com/accounts/alice/ilp"
   }
}'
```

In the above example, the account is configured to send outgoing ILP-over-HTTP requests to URL indicated in `outgoing.url`, which should be a valid URL on the Bob Connector.

{% hint style="danger" %}
Note that the `shared_secret` above is encrypted using default keys packaged with the Connector in a Java Keystore \(JKS\) file meant for illustration purposes only.  The decrypted shared secret value is **`shh`** but in a real deployment, you should use a strong shared secret and a new set of encryption keys.   
  
See [Connector Security](security-guide/crypto.md) for more details.
{% endhint %}

### Create Account: \`peter\`

Next, we need an account on the Alice Connector that can be used to initiate pings and payments. In this case, we want to use a simpler authentication scheme, so we choose `SIMPLE` \(See [ILP-over-HTTP Auth Profiles](https://github.com/interledger/rfcs/blob/master/proposals/0000-http-auth-profiles.md) for more details\). To effect this change, we can use the Connector Admin API by making the following call:

The following is a sample payload that can be used to create an account for Peter on the Alice Connector:

```javascript
curl --location --request POST 'http://alice.example.com/accounts' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data-raw '{
  "accountId": "peter",
  "accountRelationship": "CHILD",
  "linkType": "ILP_OVER_HTTP",
  "assetCode": "USD",
  "assetScale": "3",
  "customSettings": {
    "ilpOverHttp.incoming.auth_type": "SIMPLE",
    "ilpOverHttp.incoming.simple.auth_token": "shh"
   }
}'
```

{% hint style="info" %}
Note that the client authenticating as "Peter" may not be an HTTP server. In this case, Alice won't be able to send ILP packets to Peter. However, ILP-over-HTTP will still work from Peter to Alice as long as Alice because Alice's Connector is operating an ILP-over-HTTP server endpoint.
{% endhint %}

## Accounts on the Bob Connector

Next, let's mirror this setup on the Bob Connector.

### Create Account: \`alice\`

```javascript
curl --location --request POST 'http://bob.example.com/accounts' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data-raw '{
  "accountId": "alice",
  "accountRelationship": "PEER",
  "linkType": "ILP_OVER_HTTP",
  "assetCode": "USD",
  "assetScale": "3",
  "customSettings": {
    "ilpOverHttp.incoming.auth_type": "JWT_HS_256",
    "ilpOverHttp.incoming.token_subject":"alice",
    "ilpOverHttp.incoming.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",    	
    "ilpOverHttp.outgoing.auth_type": "JWT_HS_256",
    "ilpOverHttp.outgoing.token_subject": "bob",
    "ilpOverHttp.outgoing.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",
    "ilpOverHttp.outgoing.url": "https://alice.example.com/accounts/bob/ilp"
   }
}'
```

### Create Account: \`pauline\`

```javascript
curl --location --request POST 'http://bob.example.com/accounts' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data-raw '{
  "accountId": "pauline",
  "accountRelationship": "CHILD",
  "linkType": "ILP_OVER_HTTP",
  "assetCode": "USD",
  "assetScale": "3",
  "customSettings": {
    "ilpOverHttp.incoming.auth_type": "SIMPLE",
    "ilpOverHttp.incoming.simple.auth_token": "shh"
   }
}'
```

## Verify Connectivity

Now that we have accounts configured on both Connectors, we can use the Ping protocol to verify connectivity from Alice to Bob, and vice-versa.

### Ping Bob

Coming soon...

### Ping Alice

Coming soon...

