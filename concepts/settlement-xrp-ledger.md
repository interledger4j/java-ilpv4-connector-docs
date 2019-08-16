---
description: >-
  Peers accrue debt with each other by processing Interledger packets.
  Settlement allows peers to pay down that debt to enable more interledger
  packet processing.
---

# Settlement

## Overview

Payment systems like Interledger can generally be separated into two layers: A higher-performance _**clearing**_ layer, where value is transferred in a _provisional_ manner; and a lower-performance _**settlement**_ layer, which resolves any liabilities between counterparties accrued in the clearing layer. 

Most payments systems are organized in this way, and because many payment systems are often stacked on top of each other, it is common to see that one system's "settlement layer" is comprised of another system with its own clearing and settlement system. For example, from a consumer perspective, credit-card processors like Visa and Mastercard can be viewed as a "clearing systems" \(swiping the card at a merchant\) with an underlying settlement system to allow you to settle your debt to the credit card company each month \(e.g., by making a payment from your bank account\). What's interesting to see is that the system that appears to be a "settlement" system \(i.e., the banking system\) is actually comprised of its own clearing and settlement layers. For example, the bank's databases \(i.e., the clearing system\) and the Federal Reserve system \(i.e., the settlement system\).

As in the real world, settlement layers in Interledger can be based upon any medium of exchange that both parties in an Interledger peering relationship consider to be final, or "settled." 

Example settlement layer systems include:

* Offline fiat systems \(e.g., cash or paper check\)
* Direct asset transfers \(e.g., a shipment of gold\)
* Electronic ledgers \(e.g., bank ledger, PayPal, Venmo\)
* Electronic payment networks \(e.g., SWIFT, ACH, SEPA, Visa, Mastercard\)
* Cryptocurrency ledgers \(e.g., Bitcoin or XRPL\)
* Signing and exchanging payment channel claims \(e.g., XRP Paychan\)

### Settlement In Action

The Java ILP Connector [build system](https://circleci.com/gh/sappenin/java-ilpv4-connector) currently runs a nightly integration test to exercise and validate the its ability to interact with settlement engines that are compatible with IL-RFC-40 \[TODO Link\]. These tests simulate a payment made from a hypothetical user `paul` who pings a connector identified as `bob` by traversing the `alice` connector. The inverse flow is also tested \(`peter` pings `alice`\) allowing settlement payments to 

![This topology allows Paul to bing the &quot;Bob&quot; Connector, and Peter to ping the Alice Connector.](../.gitbook/assets/settlementenginetopologies.svg)

In this setup, the `alice` and `bob` connectors are able to clear payments using Interledger, accruing bilateral debt between themselves. When this debt reaches some threshold agreed between Alice and Bob, their Connectors "settle" this debt by making a payment on the XRP Ledger. 

{% hint style="info" %}
**Interledger doesn't require the usage of XRP, or any particular underlying settlement system.** To wit, in this example, Alice and Bob could settle their outstanding debts using _any_ underlying settlement system: fiat, crypto system, paper checks, or something else.
{% endhint %}

You can view the actual settlement transaction history of our integration test system by inspecting the XRP Ledger accounts here:

* **Alice's XRPL Account**: [rPtZP6jEcAMo2v1aVAJqoqqoZThRTx2voE](https://testnet.xrpl.org/accounts/rPtZP6jEcAMo2v1aVAJqoqqoZThRTx2voE) 
* **Bob's XRP Ledger Account**: [rdVEU3RfkW4q9Fg3BTBhaNJr8gy9pZRrw](https://testnet.xrpl.org/accounts/rdVEU3RfkW4q9Fg3BTBhaNJr8gy9pZRrw)





