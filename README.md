---
description: A Java implementation of an Interledger Connector supporting ILPv4.
---

# Java Connector

This site contains documentation for the Java implementation of an Interledger Router.

**Github project source code**: [https://github.com/sappenin/java-ilpv4-connector](https://github.com/sappenin/java-ilpv4-connector)  
**Docs source code**: [https://github.com/sappenin/java-ilpv4-connector-docs](https://github.com/sappenin/java-ilpv4-connector-docs)  
**Documentation Website**: [https://java-connector.ilpv4.dev](https://java-connector.ilpv4.dev)

## Overview

This implementation is a high-performance Interledger Connector that supports _many_ incoming and outgoing connections that are tied together by an ILPv4 packet-switching fabric. 

This implementation supports the following features:

* **ILPv4:** Interledger Protocol version for as defined in [IL-RFC-27](https://github.com/interledger/rfcs/blob/master/0027-interledger-protocol-4/0027-interledger-protocol-4.md).
* **ILDCP**: Interledger Dynamic Configuration Protocol as specified in [IL-RFC-31](https://github.com/interledger/rfcs/blob/master/0031-dynamic-configuration-protocol/0031-dynamic-configuration-protocol.md).
* **ILP-over-HTTP**: Allows for sending and receiving ILPv4 packets over HTTP as defined in [IL-RFC-30](https://github.com/interledger/rfcs/pull/504).
* **Route Broadcast Protocol**: Defines how Connectors can exchange routing table updates as defined in [Route Broadcast Protocol](https://github.com/interledger/rfcs/pull/455).
* **Balance Tracking**: Durably tracks account balance updates in a high-performance manner using [Redis](https://redis.io).
* **Persistent Data Storage**: Account and other data can be stored using Postgres, MySQL, Oracle, MSSQL, and more.

To learn more about how this implementation is designed, see [Connector Design](concepts/architecture-design.md).

To contribute to this project read more in [Connector Development](contributing/development.md).

## Disclaimer

**WARNING:** _**This implementation is currently an "alpha" prototype and SHOULD NOT be used in a production deployment!**_

