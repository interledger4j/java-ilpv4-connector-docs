---
description: >-
  A Java ILP Router forwards packets from source links to destination links;
  applies exchange-rates; and tracks value-balances between itself and each
  peer.
---

# Overall Design

## Account Tracking

In order to efficiently forward ILPv4 packets between its Peers, a Router must track these relationships. Read more about how this implementation tracks account relationships in [Account Model](peering-account-model.md).

## Packet Switching

The primary sub-system in this router is called a Packet Switch. This component filters incoming packets, ensures that each packet satisfies a variety of preconditions, and then forwards this packed to the appropriate outgoing link. Read more about this component in [Packet Switching Fabric](packet-switching-fabric.md).

## Exchange Rates

Each link in this ILPv4 Router tracks an account in a unique asset. In order to switch packets from one link to another, it is sometimes necessary to apply an exchange-rate. Read more about how this is accomplished in [Exchange Rates](exchange-rates.md).

## Balance Tracking

Each link this this Router also tracks real-time debt positions between the Router itself and any peer. Read more about this in [Balance Tracking](balance-tracking.md).

