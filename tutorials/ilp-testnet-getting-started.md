---
description: Create an account on the ILP TestNet to send and receive money.
---

# ILP TestNet: Getting Started

## Overview

This tutorial describes how to:

1. Create an account in at [xpring.io](https://xpring.io).
2. Fund your account using the TestNet Rainmaker \(our version of a "faucet"\).
3. Grab an API token.
4. Check your balance.
5. Pay a Friend.
6. Get paid.

## 1. Get Super Powers

Create a new account at [xpring.io](https://xpring.io). Once signed-in, navigate to your Interledger [Testnet Wallet](https://xpring.io/portal/ilp-wallet).

## 2. Make it Rain

The Xpring Testnet wallet has a rainmaker accounts that you can use to send yourself some faux XRP. Click the button in the wallet to grant yourself some XRP.

{% hint style="info" %}
The Rainmaker is available to any anyone who asks - after all, this is just a Testnet!
{% endhint %}

## 3. Grab an API Token

In the Xpring Wallet, you can get an API token by pressing the "Create a Token" button and copy the value somewhere safe. Once you close the modal window, the API token will be removed from the browser's DOM, and is otherwise unrecoverable via the Xpring Wallet.

{% hint style="danger" %}
Every time you push the "**Create a Token**" button, any previously created tokens are invalidated, so if you click this button, **anything using an older token will stop working**.
{% endhint %}

## 4. Check Your Balance

To see how much money is in your account, try the following call:

```bash
curl --location 
--request GET 'https://xpring.io/portal/ilp/hermes/accounts/{your-account-id}/balance' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer {auth_token}'
```

{% hint style="warning" %}
Be sure to replace **`{your-account-id}`**and**`{auth_token}`**with values from your Xpring Wallet account.
{% endhint %}

This request will return a JSON payload similar to this one:

```javascript
{
    "assetCode": "XRP",
    "assetScale": "9",
    "accountBalance": {
        "accountId": "caesar",
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
--request POST 'https://xpring.io/portal/ilp/hermes/accounts/{your-account-id}/pay' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer {auth_token}' \
--data-raw '{
  "amount": "1000000",
  "destinationPaymentPointer": "$rafiki.money/p/{receiver-email-address}"
}'
```

{% hint style="warning" %}
Be sure to replace **`{your-account-id}`** and **`{auth_token}`**with the values returned in Step 1!
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

Try sending money back to your Xpring wallet using the PaymentPointer in the Xpring wallet UI. Then, check your balance, either programmatically or in the UI, to see that the money has arrived in your account. 

