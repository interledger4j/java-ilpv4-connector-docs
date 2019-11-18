---
description: 'Connect to a peer using ILP, HTTP, and some love...'
---

# Peering: ILP-over-HTTP

## Overview

This guide instructs you how to setup a peering relationship between two Connectors \(i.e., `alice` and `bob`\) and also verify connectivity in both directions using the ILP Ping. 

For example, to verify connectivity from Alice to Bob, the `test.alice.peter` account is used to ping Bob at `test.bob`. Likewise, to verify connectivity from Bob to Alice, the `test.bob.pauline` account is used to ping Alice at `test.alice`.

This guide assumes the following network topology:

![](../.gitbook/assets/ilp-over-http%20%283%29.svg)

## Considerations

In order to create an actual Interledger peering relationship, you would need to determine the financial details of the relationship beforehand. This includes:

* Will this account be settled? If so, determine the `settle_threshold` and `settle_to` values \(this will affect how you want to limit what you owe the peer\).
* The amount credit to extend to your peer. This will be the `min_balance` value.
* The currency unit you will use to denominate the account relationship. This guide will use `USD` in all examples.
* See [Account Configuration]() for more details.

{% hint style="info" %}
 This guide does not enable settlement. See [Settlement: XRP Leger](settlement-xrp-ledger.md) for an example of how that could work using this guide as a starting point.
{% endhint %}

## Tutorial Assumptions

For simplicity, this guide makes the following assumptions:

* There are two Connectors that will peer with each other \(i.e., **Alice** and **Bob**\). Each connector will have an account configured for the peer.
* Each connector will have a separate account \(e.g., Peter and Pauline\) that can be used to initiate ILP Ping requests.
* Actor Info
  * **Alice's Info**
    * **ILP Address**: `test.alice`
    * **URL**: [https://alice.example.com/ilp](https://alice.example.com/ilp)
  * **Bob's Info**
    * **ILP Address**: `test.bob`
    * **URL**: https://bob.example.com/ilp
  * **Peter's Info**
    * **ILP Address:** `test.alice.peter`
    * **URL**: n/a
  * **Pauline's Info**
    * **ILP Address:** `test.bob.pauline`
    * **URL**: n/a

{% hint style="success" %}
Reference the [Connector Admin API](../api-references/untitled.md) documentation for more details about actual payloads and their meanings.
{% endhint %}

## Configuration on Alice

### Create Account: \`bob\`

In order to peer with the Bob Connector, we must first tell Alice about Bob. This is done by creating an account for Bob on Alice.

The following payload illustrates the details of this account:

```javascript
{
  "accountId": "bob",
  "accountRelationship": "PEER",
  "linkType": "BLAST",
  "assetCode": "USD",
  "assetScale": "2",
  "isSendRoutes": true,
  "isReceiveRoutes": true
  "customSettings": {
	"blast.incoming.auth_type": "JWT_HS_256",
    "blast.incoming.token_issuer": "https://bob.example.com/",
    "blast.incoming.token_audience": "https://alice.example.com/",
    "blast.incoming.token_subject":"bob",
    "blast.incoming.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",    	

    "blast.outgoing.auth_type": "JWT_HS_256",
    "blast.outgoing.token_issuer": "https://alice.example.com/",
    "blast.outgoing.token_audience": "https://bob.example.com/",
    "blast.outgoing.token_subject": "alice",
    "blast.outgoing.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",
    "blast.outgoing.url": "https://bob.example.com/ilp"
  }
}
```

{% hint style="danger" %}
Note that the `shared_secret` above is encrypted using default keys packaged with the Connector in a Java Keystore \(JKS\) file meant for illustration purposes only.  The decrypted shared secret value is **`shh`** but in a real deployment, you _should_ use a strong shared secret and a new set of keys.   
  
See [Connector Security](../security-guide/crypto.md) for more details.
{% endhint %}

### Create Account: \`peter\`

The following is a sample payload that can be used to create an account for Peter on the Alice Connector:

```javascript
{
  "accountId": "peter",
  "accountRelationship": "CHILD",
  "linkType": "BLAST",
  "assetCode": "USD",
  "assetScale": "2",
  "customSettings": {
	"blast.incoming.auth_type": "JWT_HS_256",
    "blast.incoming.token_issuer": "https://peter.example.com/",
    "blast.incoming.token_audience": "https://peter.example.com/",
    "blast.incoming.token_subject":"peter",
    "blast.incoming.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",    	

    "blast.outgoing.auth_type": "JWT_HS_256",
    "blast.outgoing.token_issuer": "https://alice.example.com/",
    "blast.outgoing.token_audience": "https://alice.example.com/",
    "blast.outgoing.token_subject": "alice",
    "blast.outgoing.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",
    "blast.outgoing.url": "https://peter.example.com/ilp"
  }
}
```

{% hint style="info" %}
Note that the client authenticating as "Peter" may not be an HTTP server. In this case, Alice won't be able to send ILP packets to Peter. However, ILP-over-HTTP will still work from Peter to Alice as long as Alice is operating an ILP-over-HTTP server endpoint.
{% endhint %}

## Configuration on Bob

### Create Account: \`alice\`

In order to peer with the Alice Connector, we must likewise tell Bob about Alice. This is done by creating an account for Alice on Bob.

The following payload illustrates the details of this account:

```javascript
{
  "accountId": "alice",
  "accountRelationship": "PEER",
  "linkType": "BLAST",
  "assetCode": "USD",
  "assetScale": "2",
  "isSendRoutes": true,
  "isReceiveRoutes": true
  "customSettings": {
	"blast.incoming.auth_type": "JWT_HS_256",
    "blast.incoming.token_issuer": "https://alice.example.com/",
    "blast.incoming.token_audience": "https://bob.example.com/",
    "blast.incoming.token_subject": "alice",
    "blast.incoming.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",    	

    "blast.outgoing.auth_type": "JWT_HS_256",
    "blast.outgoing.token_issuer": "https://bob.example.com/",
    "blast.outgoing.token_audience": "https://alice.example.com/",
    "blast.outgoing.token_subject": "bob",
    "blast.outgoing.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",
    "blast.outgoing.url": "https://bob.example.com/ilp"
  }
}
```

### Create Account: \`pauline\`

```javascript
{
  "accountId": "peter",
  "accountRelationship": "CHILD",
  "linkType": "BLAST",
  "assetCode": "USD",
  "assetScale": "2",
  "customSettings": {
	"blast.incoming.auth_type": "JWT_HS_256",
    "blast.incoming.token_issuer": "https://peter.example.com/",
    "blast.incoming.token_audience": "https://peter.example.com/",
    "blast.incoming.token_subject":"peter",
    "blast.incoming.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",    	

    "blast.outgoing.auth_type": "JWT_HS_256",
    "blast.outgoing.token_issuer": "https://alice.example.com/",
    "blast.outgoing.token_audience": "https://alice.example.com/",
    "blast.outgoing.token_subject": "alice",
    "blast.outgoing.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",
    "blast.outgoing.url": "https://peter.example.com/ilp"
  }
}
```

## Verify Connectivity

The administrator of the peer account should create an account for your Connector. The details should be a loose inversion of the above request. For example, from the perspective of your Peer's Connector, teh `blast.outgoing.url` will be your URL.

### Ping Bob

### Ping Alice

In order to verify that connectivity is correctly established, you need a separate account on your Connector that will allow you to send ping packets to the Peer.

