---
description: Create an account on the ILP TestNet to send and receive money.
---

# ILP TestNet: Getting Started

## Overview

This tutorial describes how to:

1. Create an account at [https://faucet.ilpv4.dev](https://faucet.ilpv4.dev).
2. Grab an API token.
3. Fund your account using the TestNet Rainmaker.
4. Check your balance.
5. Pay a Friend.
6. Get paid.

## 1. Get Super Powers

Create a new, programmable TestNet account at [https://faucent.ilpv4.dev](https://faucent.ilpv4.dev).

{% hint style="warning" %}
If you want to experiment with a Wallet UI instead of a programmable account, you can experiment with https://wallet.ilpv4.dev. 

Note that this account is distinct from any accounts created by the faucet above, and wallet accounts _**do not**_ support bearer-token programmability \(only the faucet supports that type of interaction\).
{% endhint %}

## 2. Make it Rain

To send your new faucet account faux XRP, issue the following command:

```text
curl --location --request POST 'https://hermes-rest.ilpv4.dev/accounts/{your-account-id}/money' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data-raw ''
```

This will add 10 faux XRP to your faucet account.

{% hint style="info" %}
The Rainmaker is available to any anyone who asks - after all, this is just a TestNet!
{% endhint %}

If you prefer a UI instead, the testnet wallet at [https://wallet.ilpv4.dev](https://wallet.ilpv4.dev) has a rainmaker button that you can use to send yourself some faux XRP. Click the button in that wallet to grant yourself some XRP.

{% hint style="danger" %}
As noted above, the Faucet and the Wallet UI accounts are distinct.
{% endhint %}

## 3. Grab an API Token

In the ilpv4.dev [Faucet](https://faucet.ilpv4.dev/), you can obtain an API token by pressing the `Generate ILP Testnet Credentials` button.

{% hint style="danger" %}
Make sure to capture the generated credentials because once you close your browser window, the API token and other account information will be lost, and is otherwise unrecoverable.
{% endhint %}

## 4. Check Your Balance

To see how much money is in your account, try the following call:

```bash
curl --location --request GET 'https://jc.ilpv4.dev/accounts/{your-faucet-account-id}/balance' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer {auth_token}'
```

{% hint style="warning" %}
Be sure to replace **`{your-account-id}`**and**`{auth_token}`**with values from the ilpv4.dev [Faucet](https://faucet.ilpv4.dev).
{% endhint %}

This request will return a JSON payload similar to this one:

```javascript
{
    "assetCode": "XRP",
    "assetScale": "9",
    "accountBalance": {
        "accountId": "user_ykqwwe2b",
        "netBalance": "0",
        "clearingBalance": "0",
        "prepaidAmount": "0"
    }
}
```

## 5. Pay a Friend

Spread the love to a friend by making a payment to a payment pointer. In this case, try sending value to a different wallet on the testnet. Maybe someone at [https://rafiki.money](https://rafiki.money).

```bash
curl --location \
--request POST 'https://hermes-rest.ilpv4.dev/accounts/{your-account-id}/pay' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer {auth_token}' \
--data-raw '{
  "amount": "1000000",
  "destinationPaymentPointer": "$rafiki.money/p/{receiver-email-address}"
}'
```

{% hint style="warning" %}
Be sure to replace **`{your-account-id}, {auth_token}, and {receiver-email-address}`**with appropriate values from Step 1 and from the Rafiki wallet.
{% endhint %}

This request will return JSON similar to the JSON below, representing 1,000 XRP drops paid:

```text
{
    "originalAmount": "1000000",
    "amountDelivered": "1000000",
    "amountSent": "1000000",
    "successfulPayment": true
}
```

{% hint style="info" %}
Note the meaning of the following fields:

**originalAmount**: the amount that you wanted to send.  
**amountDelivered**:  the amount your friend actually received.  
**amountSent**: is the amount that actually got sent to your friend.
{% endhint %}

## 6. Get Paid

Try sending money back to your Faucet wallet using the PaymentPointer obtained in step 1. Then, check your balance programmatically to see that the money has arrived in your account. 

