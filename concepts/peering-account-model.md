---
description: >-
  Interledger Routers track peer relationships using a concept called an
  Account.
---

# Account Model

Router Accounts have two primary functions. 

The first is to track a debt position, denominated in a single fungible asset, between two Interledger parties \(read more about this in [Balance Tracking](balance-tracking.md)\). 

The second purpose is to provide a conduit for exchanging ILP packets. When chained together to form a payment path, these relationships can enable value transfer across the Interledger.

When two Interledger nodes \(two ILP Routers, for example\) enter into an account arrangement \(sometimes called a Peering Relationship\), each Router will construct a unique identifier to track the relationship for itself. This implementation calls this identifier an `accoundId`.

Using account Ids, a Connector can correlate each details about the relationship using three different primitives \(described below\). 

This design is preferred over a single `Account` object because Connectors must be able to support ultra-high packet throughput, and using a single domain-model object would likely not scale well for all use-cases that a Router must fulfill.

## Account Settings

The [`AccountsSettings`](https://github.com/interledger4j/ilpv4-connector/blob/master/connector-accounts/src/main/java/org/interledger/connector/accounts/AccountSettings.java) object tracks all information necessary for the Connector to _describe_ an account. This includes minimum and balance thresholds, link information, and information about about the underlying asset for the account \(i.e., the asset `code` and `scale`\).

This data is typically stored in a durable data-store, and loaded at various times in a performant yet as-needed basis by the Router. In general, account information is highly cacheable using local-caches with relatively short timeouts \(which works well-enough across a cluster\) so this type of information can easily live in a typical RDBMS. See [Connector Persistence](../ilpv4-connector-persistence.md) for more details around supported datastores.

## Balance Tracking

This implementation currently supports [Bilateral Balance Tracking](terminology.md) to track balances for each account. 

In this configuration, each account holder should be thought of as holding discrete types of IOUs. For example, if Alice and Bob are tracking a bilateral balance in US Dollars, then from Alice's perspective the unit of account is called an "Alice Owes Bob Dollars" or `AOB`. From Bob's perspective, the unit of account is called a "Bob Owes Alice Dollars" or `BOA`. 

Balances on either side of the account can become positive or negative, but the sum total of both balances _must_ always equal zero. Additionally, each balance-pair will always be the inverse of the other side's balance. 

Thus, if Alice has a balance of `-10` , then Bob _must_ have a balance of `+10`, which means that Bob has 10 BOAs and Alice has -10 AOBs. In other words, Bob has a debt position with Alice in which he is holding 10 of her units, payable to Alice. Conversely, Alice has a lending position with Bob in which she has lent him 10 units \(which he might not pay back\). From Alice's perspective, she holds -10 AOBs, which is to say Alice holds -10 obligations to pay Bob $1 USD. Put into different language, this equates to Alice holding 10 obligations for Bob to pay her $1 USD.

Grokking the meaning of a bidirectional accounting relationship like this can be difficult. The simplest way to think about this is that generally two account holders are creating debt obligations between each other.

## Links

This implementation uses the concept of a [`Link`](https://github.com/sappenin/java-ilpv4-connector/blob/master/ilpv4-connector-link/src/main/java/org/interledger/connector/link/Link.java) to describe the connection between two peers. Links have their own identifier called a `LinkId` which uniquely identifies each Link. 

Because a Link is an abstraction over the connection between two peers, a Link can operate over any underlying transport. Examples of this include HTTP, WebSockets, UDP, or any other communications mechanism.

This implementation enforces a single Link per account at any given time, and also prohibits a Link from operating over more than a single underlying transport. Because of these restrictions, the `LinkId` in this implementation is always equal to the `AccountId`, which effectively means an packets are only ever being transacted over a given account using a single Link.

