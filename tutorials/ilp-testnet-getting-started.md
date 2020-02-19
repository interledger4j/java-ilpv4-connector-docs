---
description: Create an account on the ILP TestNet to send and receive money.
---

# ILP TestNet: Getting Started

## Overview

This tutorial describes how to:

1. Create an account in the Xpring.io Interledger Testnet.
2. Fund your account using the TestNet Rainmaker \(our version of a "faucet"\).
3. Check your balance.
4. Pay a Friend.
5. Get paid.

## 1. Get Super Powers

Create a new account using the following command:

```
> curl --location --request POST 'https://hermes-rest.ilpv4.dev/accounts' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data-raw '{
  "assetCode": "XRP",
  "assetScale": "6"
}'
```

{% hint style="info" %}
Super-powers are granted to anyone who asks - after all, this is just a Testnet!
{% endhint %}

The above request will return a payload that contains your `accountId` , an auth token which you will use to authenticate to the connector, as well as a [Payment Pointer](https://paymentpointers.org/) that can be used to receive value in your test account.

Here's an example response payload for reference:

```text
{
    "accountId": "user_qqGV8nij",
    "accountRelationship": "CHILD",
    "assetCode": "XRP",
    "assetScale": "6",
    "maximumPacketAmount": null,
    "linkType": {},
    "connectionInitiator": true,
    "isInternal": false,
    "sendRoutes": true,
    "receiveRoutes": false,
    "balanceSettings": {
        "minBalance": null,
        "settleThreshold": null,
        "settleTo": "0"
    },
    "rateLimitSettings": {
        "maxPacketsPerSecond": null
    },
    "settlementEngineDetails": null,
    "customSettings": {
        "ilpOverHttp.outgoing.url": "https://money.ilpv4.dev/ilp",
        "ilpOverHttp.incoming.auth_type": "SIMPLE",
        "ilpOverHttp.incoming.simple.auth_token": "JQBzzLkbslrsT",
        "ilpOverHttp.outgoing.simple.auth_token": "enc:jks:crypto/crypto.p12:secret0:1:aes_gcm:AAAADDBa7b-nDvY2ydWysQQMoZL7yIOmK-7-3kLiMc9pxhzPw1Ei68OpwcZu6W-j",
        "ilpOverHttp.outgoing.auth_type": "SIMPLE"
    },
    "paymentPointer": "$money.ilpv4.dev/user_qqGV8nij",
    "parentAccount": false,
    "childAccount": true,
    "peerAccount": false,
    "peerOrParentAccount": false
}
```

{% hint style="info" %}
The generated `accountId` can be found at `"accountId"`, and your generated auth token can be found at `"customSettings" -> "ilpOverHttp.incoming.simple.auth_token".`
{% endhint %}

## 2. Make it Rain

The TestNet has a rainmaker accounts that you can use to send yourself some faux XRP. Use the following command to make it rain:

```
> curl --location --request POST \
  'https://hermes-rest.ilpv4.dev/accounts/{your-account-id}/money'
```

{% hint style="warning" %}
In the call above, make sure to replace `{your-account-id}` with your own `accountId` created above.
{% endhint %}

## 3. Check Your Balance

To see how much money is in your account, try the following call:

```
> curl --location --request GET 'https://hermes-rest.ilpv4.dev/accounts/{your-account-id}/balance' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer {auth_token}' \
```

{% hint style="warning" %}
Be sure to replace `{auth_token}` above with the auth token returned when you generated your account!
{% endhint %}

This request will return JSON similar to the JSON below:

```text
{
    "assetCode": "XRP",
    "assetScale": "6",
    "accountBalance": {
        "accountId": "user_qqGV8nij",
        "netBalance": "0",
        "clearingBalance": "0",
        "prepaidAmount": "0"
    }
}
```

## 4. Pay a Friend

Spread the love to a friend by making a payment to a payment pointer. In this case, try sending value to a different wallet on the testnet. Maybe someone at [https://rafiki.money](https://rafiki.money).

```
> curl --location --request POST 'https://hermes-rest.ilpv4.dev/accounts/{your-account-id}/pay' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer {auth_token}' \
--data-raw '{
  "amount": "10",
  "destinationPaymentPointer": "$rafiki.money/p/dfuelling"
}'
```

{% hint style="warning" %}
Be sure to replace `{your-account-id}` and `{auth_token}` with the values returned in Step 1!
{% endhint %}

This request will return JSON similar to the JSON below:

```text
{
    "originalAmount": "10",
    "amountDelivered": "10",
    "amountSent": "10",
    "successfulPayment": true
}
```

{% hint style="info" %}
"originalAmount" is the amount that you wanted to send.

"amountDelivered" is the amount your friend actually received.

"amountSent" is the amount that actually got sent to your friend.
{% endhint %}

## 5. Get Paid

Try sending yourself money from your rafiki.money wallet. Then, check your balance to see that the money has arrived in your account. 

