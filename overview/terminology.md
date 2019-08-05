---
description: 'Words, words, words...let''s all get on the same page.'
---

# Terminology

**Account**  
Interledger supports transactions on any type of fungible asset, and uses _accounts_ to track debt positions  in a particular asset between two parties. Accounts are always denominated in a single, mutually agreed upon asset type and only ever include two parties.

**Account Assets**  
Asset types tracked in an account include any type of fungible asset. This includes fiat and crypto currencies like U.S. Dollars, Euros, Bitcoin, or XRP. Accounts are not limited to tracking currency, however, and can represent other fungible things like equities or commodities.

**Accounts Asset Identifiers**  
Interledger tracks assets using an identifier that can represent an asset. For example, `USD`, `EUR`, `BTC`, `XRP`or even stock symbols like `SBUX` or `AAPL`can be used. Interledger even supports contrived asset identifiers for specific use-cases, such as `WIDGET`. 

**Account Amounts**  
Each account includes a unit of account for the asset being tracked called an _amount_, which may be represented using any denomination. For example, one U.S. dollar may be represented as 1 USD or 100 cents, each of which is an equivalent unit of value. Likewise, one Bitcoin may be represented as 1 BTC or 100,000,000 satoshis.

**Account Standard Units**  
The **standard unit** represents the typical unit of account for a particular asset. For example $1 in the case of U.S. dollars, or 1 BTC in the case of Bitcoin. 

Note that peers are free to define this value in any way, but participants in an Interledger accounting relationship _must_ be sure to use the same value. Thus, it is suggested to use _typical_ values when possible.

**Account Fractional Units**  
A **fractional unit** represents some unit smaller than its corresponding standard unit, but with greater precision. Examples of fractional monetary units include one cent \($0.01 USD\), or 1 satoshi \(0.00000001 BTC\).

**Account Balance**  
The total debt position between two peers represented in a particular fungible unit of account. Account balances can be tracked in arbitrary ways, although the two most common forms are bilateral \(both parties track an independent view of the balance\) or unilateral \(one party tracks the balance for both sides of the Account\).

**Account Balance Tracking \(Bilateral\)**  
A mechanism for tacking balances where both parties to an account track the balance independently, with the sum of both sides' balances always equaling zero. 

For example, this type of balance will always start off with each party having a balance of 0. If Alice Pays Bob 10 units, then Alice's balance tracker will show -10, whereas Bob's balance tracker will show +10.  If Bob then sends Alice 20 units, then Alice's balance tracker will show +10 and Bob's balance tracker will show -10.

**Account Balance Tracking \(Unilateral\)**  
A mechanism for tacking balances where only one party to an account tracks the balance for both parties. Generally speaking, the entity holding actual assets should be the party that tracks a unilateral account balance. As such, the unit of account on such a balance is an IOU payable from the balance tracking entity to the counterparty.

For example, if Alice tracks a balance for an account between herself and Bob, then a positive balance \(e.g., 10 USD\) indicates that Alice owes Bob $10. Conversely, a negative balance \(e.g., -10 USD\) indicates that Bob owes alice $10.

**Link**  
A connection between two peers, involving a single account, where ILPv4 packets can be exchanged over some network transport \(such as a Websocket or a regular HTTP connection\).

**Peer**  
Any entity that has agreed to exchange Interledger packets with another entity. This sometimes refers to two individuals who share an account. However, this can also refer to software programs that are connected to each other in a peering relationship. See **Peer Node** for more details.

**Peer Node**  
An Interledger sender, router, or receiver that has an accounting relationship with another server.

