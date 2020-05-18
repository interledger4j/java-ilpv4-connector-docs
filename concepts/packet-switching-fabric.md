---
description: >-
  ILPv4 packets are forwarded to an outgoing link using a high-performance
  packet-switching fabric.
---

# Packet Switching Fabric

## Overall Design

This implementation relies upon a packet-switching fabric that applies business logic to each packet in order to ensure that packets meet various preconditions and can be properly routed. At a high level, this fabric looks like this:

![Packets enter the switching fabric on an inbound link, are processed, and then forwarded out on an outbound link. Responses traverse the fabric in the opposite direction.](../.gitbook/assets/image%20%283%29.png)

### Packet Filters

A packet filter applies business logic to a particular ILPv4 Prepare packet. The contract allows the filter to modify an in-flight packet, forward it onward to the next filter, or reject the packet outright. Read more about the contract [here](https://github.com/sappenin/java-ilpv4-connector/blob/aa84b650426817d9db322f9d0889ab80505f3c90/ilpv4-connector-service-api/src/main/java/com/sappenin/interledger/ilpv4/connector/packetswitch/filters/PacketSwitchFilter.java). 

### Packet Filter Chain

Packet filters are assembled together into a filter chain that is applied to every incoming packet. This implementation has a filter chain that looks like this:

![Each incoming ILPv4 packet is &quot;filtered&quot; according to the business logic of each Packet Filter.](../.gitbook/assets/image%20%281%29.png)

### Link Filters

A Link Filter is similar to a Packet Filter in that it applies business logic to a particular ILPv4 Prepare packet. However, these filters are only engaged before sending packets into an Outbound Link.

Like Packet Filters, the contract allows the filter to modify an in-flight packet, forward it onward, or reject the packet outright without sending it into the Outbound link. Read more about the contract [here](https://github.com/sappenin/java-ilpv4-connector/blob/aa84b650426817d9db322f9d0889ab80505f3c90/ilpv4-connector-service-api/src/main/java/com/sappenin/interledger/ilpv4/connector/links/filters/LinkFilter.java). 

### Link Filter Chain

Link filters are assembled together into a filter chain that is applied to every outgoing packet. This implementation has a filter chain that looks like this:

![Each outgoing ILPv4 packet is &quot;filtered&quot; according to the business logic of each Link Filter.](../.gitbook/assets/image%20%282%29%20%281%29.png)

