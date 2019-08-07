---
description: A Java implementation of an Interledger v4 Router.
---

# Java ILPv4 Router

This site contains documentation for the Java implementation of an Interledger Router.

**Github project source code**: [https://github.com/sappenin/java-ilpv4-connector](https://github.com/sappenin/java-ilpv4-connector)  
**Docs source code**: [https://github.com/sappenin/java-ilpv4-connector-docs](https://github.com/sappenin/java-ilpv4-connector-docs)  
**Documentation Website**: [https://java-router.ilpv4.dev](https://java-router.ilpv4.dev)

## Overview

This implementation is a high-performance Interledger Router that supports _many_ incoming and outgoing connections that are tied together by an ILPv4 packet-switching fabric.

This implementation supports the following features:

* **ILDCP**: Interledger Dynamic Configuration Protocol as specified in [IL-RFC-0031](https://github.com/interledger/rfcs/blob/master/0031-dynamic-configuration-protocol/0031-dynamic-configuration-protocol.md).
* **ILP-over-HTTP**: Also known as BLAST \(**B**i**L**ateral **A**synchronous **S**eder **T**ransport\), defined in [IL-RFC-0030](https://github.com/interledger/rfcs/pull/504).
* **Route Broadcast Protocol**: Defines how Connectors can exchange routing table updates as defined in [Route Broadcast Protocol](https://github.com/interledger/rfcs/pull/455).
* **Balance Tracking**: [Redis](https://redis.io) is used to durably track balance updates in a high-performance manner.
* **Persistent Data Storage**: Account and other data can be stored using Postgres, MySQL, Oracle, MSSQL, and more.

To learn more about how this implementation is designed, see [Connector Design](concepts/architecture-design.md).

To contribute to this project read more in [Connector Development](contributing/development.md).

## Disclaimer

**WARNING:** _**This implementation is currently an "alpha" prototype and SHOULD NOT be used in a production deployment!**_

