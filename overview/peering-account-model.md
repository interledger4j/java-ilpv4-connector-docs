# Peering Account Model

Interledger Connectors track relationships with their peers using a concept called an `Account`. Accounts have two primary functions. The first is to track an asset balance, denominated in a single currency/asset-type, between two Interledger parties. The second purpose is to provide a conduit for exchanging ILP packets, which can enable value transfer across the Interledger.

When two ILP nodes \(two Connectors, for example\) enter into an accounting relationship, each Connector will construct a unique identifier to track the account for itself. This implementation calls this identifier an `accoundId`.

Using account Ids, a Connector will track the _concept_ of an account using three different primitives, each of which is described below in more detail. This design is preferred over a single `Account` object because Connectors must be able to support ultra-high packet throughput, and using a single account domain-model would likely not scale well for all use-cases.

## Account Settings

The [`AccountsSettings`](https://github.com/sappenin/java-ilpv4-connector/blob/master/ilpv4-connector-accounts/src/main/java/org/interledger/connector/accounts/AccountSettings.java) object tracks all information necessary for the Connector to _describe_ an account. This includes minimum and balance thresholds, link information, and information about about the underlying asset for the account \(i.e., the asset `code` and `scale`\).

This data is typically stored in a durable data-store, and loaded at various times in a performant yet as-needed basis by the Router. In general, account information is highly cacheable using local-caches with relatively short timeouts \(which works well-enough across a cluster\) so this type of information can easily live in a typical RDBMS. See [Connector Persistence](../operating-a-connector/ilpv4-connector-persistence.md) for more details around supported datastores.

## Balance Tracking

This implementation utilizes [Bilateral Balance Tracking](terminology.md) to track balances for each account. In this balance tracking configuration, each account holder should be thought of as holding discrete types of IOUs \(each from the perspective of the entity tracking its side of the account\). 

For example, if Alice and Bob are tracking a bilateral balance in US Dollars, then from Alice's perspective the unit of account is called an "Alice Owes Bob Dollars" or `AOB`. From Bob's perspective, the unit of account is called a "Bob Owes Alice Dollars" or `BOA`. 

Balances on either side of the account can become positive or negative, but because the sum total of both balances must always equal zero, each balance will always be the inverse of the other side's balance. 

Thus, if Alice has a balance of `-10` , then Bob _must_ have a balance of `+10`, which means that Bob has 10 BOAs and Alice has -10 AOBs. In other words, Bob has a debt position with Alice in which he is holding 10 of her units, payable to Alice. Conversely, Alice has a lending position with Bob in which she has lent him 10 units \(which he might not pay back\). From Alice's perspective, she holds -10 AOBs, which is to say Alice holds -10 obligations to pay Bob $1 USD. Put into different language, this equates to Alice holding 10 obligations for Bob to pay her $1 USD.

## Links

This implementation uses the concept of a `Link` to describe the connection between two peers. Links have their own identifier called a `LinkId` which uniquely identifies a Link. Because a Link is an abstracion over the connection between two peers, a Link can operate over any underlying transport. Examples of this include HTTP, WebSockets, UDP, or any other communications mechanism.

This implementation enforces a single Link per accountId at any given time, and also prohibits a Link from operating over more than a single underlying transport. Because of these restrictions, the `LinkId` in this implementation is always equal to the `AccountId`, which effectively means an packets are only ever being transacted over a given account using a single Link.

