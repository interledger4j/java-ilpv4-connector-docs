---
description: 'Words, words, words...let''s all get on the same page.'
---

# Terminology

**Account**  
A mechanism to track debt between two parties. Debt positions are denominated in a single, mutually agreed upon asset such as `USD`, `EUR`, `BTC`, `XRP` or any other type of asset code like `WIDGETS`. The balance of an account represents the total number of units that one party owes the other, depending upon the balance tracking method \(see more below\).

**Account Balance**  
The total debt position between two peers represented in a particular fungible unit of account. Account balances can be tracked in arbitrary ways, although the two most common forms are bilateral \(both parties track an independent view of the balance\) or unilateral \(one party tracks the balance for both sides of the Account\).

**Balance Tracking \(Bilateral\)**  
A mechanism for tacking balances where both parties to an account track the balance independently, with the sum of both sides' balances always equaling zero. For example, this type of balance will always start off with each party having a balance of 0. If Alice Pays Bob 10 units, then Alice's balance tracker will show -10, whereas Bob's balance tracker will show +10.  If Bob then sends Alice 20 units, then Alice's balance tracker will show +10 and Bob's balance tracker will show -10.

**Balance Tracking \(Unilateral\)**  
A mechanism for tacking balances where only one party to an account tracks the balance for both parties. Generally speaking, the entity holding actual assets should be the party that tracks a unilateral account balance. As such, the unit of account on such a balance is an IOU payable from the balance tracking entity to the counterparty.

For example, if Alice tracks a balance for an account between herself and Bob, then a positive balance \(e.g., 10 USD\) indicates that Alice owes Bob $10. Conversely, a negative balance \(e.g., -10 USD\) indicates that Bob owes alice $10.

**Link**  
A connection between two peers, involving a single account, where ILPv4 packets can be exchanged over some network transport \(such as a Websocket or a regular HTTP connection\).

**Peer**  
Any entity that has agreed to exchange Interledger packets with another entity. This sometimes refers to two individuals who share an account. However, this can also refer to software programs that are connected to each other in a peering relationship. See **Peer Node** for more details.

**Peer Node**  
An Interledger sender, router, or receiver that has an accounting relationship with another server.

