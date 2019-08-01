# Features

This implementation is intended for operation as a server-based ILSP, meaning it will listen for, and accept, and respond to potentially _many_ incoming connections. Specifically, this implementation supports the following ILP features:

* **ILDCP**: Interledger Dynamic Configuration Protocol as specified in [IL-RFC-0031](https://github.com/interledger/rfcs/blob/master/0031-dynamic-configuration-protocol/0031-dynamic-configuration-protocol.md).
* **ILP-over-HTTP**: Also known as BLAST \(**B**i**L**ateral **A**synchronous **S**eder **T**ransport\), defined in [IL-RFC-0030](https://github.com/interledger/rfcs/pull/504).
* **Route Broadcast Protocol**: Defines how Connectors can exchange routing table updates as defined in [Route Broadcast Protocol](https://github.com/interledger/rfcs/pull/455).
* **Balance Tracking**: [Redis](https://redis.io) is used to durably track balance updates in a high-performance manner.
* **Persistent Data Storage**: Account and other data can be stored using Postgres, MySQL, Oracle, MSSQL, and more.

