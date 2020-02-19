---
description: Create an account on the ILP TestNet to send and receive money.
---

# ILP TestNet: Getting Started

## Overview

This tutorial describes how to:

* Create an account in the [xpring.io](https://xpring.io) Interledger test-net.
* Send and receive value using the Xpring's Interledger RESTful API.
* Check your balance.

## Getting Super Powers

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

This request will return a payload that contains your `accountId` as well as a [Payment Pointer](https://paymentpointers.org/) that can be used to receive value in your test account.

{% hint style="info" %}
Super-powers are granted to anyone who asks - after all, this is just a TestNet!
{% endhint %}

## Make it Rain!

The TestNet has a rainmaker accounts that you can use to send yourself some faux XRP. Use the following command to make it rain:

```
> curl --location --request POST \
  'https://hermes-rest.ilpv4.dev/accounts/{your-account-id}/money'
```

{% hint style="warning" %}
In the call above, make sure to replace `{your-account-id}` with your own `accountId` created above.
{% endhint %}

## Check Your Balance

To see how much money is in your account, try the following call:

```
> curl --location --request GET 'https://hermes-rest.ilpv4.dev/accounts/{your-account-id}/balance' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer shh' \
```

{% hint style="warning" %}
Be sure to replace `shh` above with the auth token returned when you generated your account!
{% endhint %}

## Pay a Friend

Spread the love to a friend by making a payment to a payment pointer. In this case, try sending value to a different wallet on the testnet. Maybe someone at [https://rafiki.money](https://rafiki.money).

```
> curl --location --request POST 'https://hermes-rest.ilpv4.dev/accounts/{your-account-id}/payments' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer shh' \
--data-raw '{
  "amount": "10",
  "recipient": "$rafiki.money/p/dfuelling"
}'
```

