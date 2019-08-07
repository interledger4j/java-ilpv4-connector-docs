---
description: >-
  This page describes available persistence stores and how to configure the
  Router to operate using them.
---

# Persistence

## Overview

This implementation classifies persistence data into two broad categories: data collected during performance-sensitive operations, and all other data.

Data collected during performance-sensitive operations is limited to the data required to facilitate the ILPv4 packet flow. In general, this type of data is limited to balance tracking information, and this implementation supports both Redis and Postgres for this use-case. 

All other data, such as non-balance account configuration, Router Configuration, etc., is stored into one of several supported RDBMS datastores. This implementation currently supports [Posgresql](https://www.postgresql.org/), [MS-SQL](https://www.microsoft.com/en-us/sql-server/default.aspx), [MySQL](https://www.mysql.com/), and [Oracle](https://www.oracle.com/database/12c-database/).

## Postgresql

This section details how to use Postgresql as the underlying Router datastore.

### Generate DDL

To generate the DDL for the Connector database, execute the following commands:

```bash
>  mvn -DskipTests clean package -P liquibase-pg-sql liquibase:updateSQL
```

This will emit a file `/target/liquibase/migrate.sql` that can be used to populate your database. Alternatively, executing this command will actully connect to the database and run this DDL for you:

```bash
> mvn -DskipTests clean package -P liquibase-pg-sql liquibase:update
```

### Reinitialization

If you want to reinitialize the database, for example during development, the following commands can be used:

```bash
> mvn -DskipTests clean package -P liquibase-pg-sql liquibase:dropAll
> mvn -DskipTests clean package -P liquibase-pg-sql liquibase:update
```

## Oracle

_Coming soon._

## MySQL

_Coming soon._

## MS-SQL

_Coming soon._

## Redis

_Coming soon._

