---
description: >-
  This page describes available persistence stores and how to configure the
  Router to operate using them.
---

# Persistence Initialization

## Overview

This implementation classifies persistence data into two broad categories: data collected during performance-sensitive operations, and all other data.

Data collected during performance-sensitive operations is limited to the data required to facilitate the ILPv4 packet flow. In general, this type of data is limited to balance tracking information, and this implementation supports both Redis and Postgres for this use-case. 

All other data, such as non-balance account configuration, runtime configuration, and more is stored into one of several supported RDBMS datastores. This implementation currently supports [Posgresql](https://www.postgresql.org/), [MS-SQL](https://www.microsoft.com/en-us/sql-server/default.aspx), [MySQL](https://www.mysql.com/), and [Oracle](https://www.oracle.com/database/12c-database/).

{% hint style="info" %}
This page details how to initialize a given persistence store for usage by the Connector. To set connection details and other runtime configuration properties, see [here](configuration.md) instead.
{% endhint %}

## Postgresql

This section details how to use Postgresql as the underlying Router datastore.

{% hint style="success" %}
This section assumes that you have created a database named `connector`inside of your Postgres installation. Note that this naming is used as an example only -- you can choose _any_ database name.
{% endhint %}

### Generate DDL

To generate the DDL for the Connector database, execute the following commands:

```bash
mvn -DskipTests clean package -P liquibase-pg-sql liquibase:updateSQL
```

This will emit a file `/target/liquibase/migrate.sql` that can be used to populate your database. 

### Direct Initialization via Maven

As an alternative during development, you can utilize the liquibase-maven-plugin to directly connect to  Postgres and initialize the database for you. 

However, for this to work, you first need to update the `pom.xml` file in the `ilpv4-connector-persistence` module to conform to your connection parameters by setting the `configuration.url` to a value for your environment, like this:

{% code-tabs %}
{% code-tabs-item title="ilpv4-connector-persistence/pom.xml" %}
```markup
...
<profiles>
    <profile>
      <id>liquibase-pg-sql</id>
      ...
        <configuration>
          <url>jdbc:postgresql://localhost:5432/connector</url>
        </configuration>        
      ...
</profiles>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Once the liquibase-maven-plugin is configured properly, the following commands will update the database directly:

```bash
cd ./java-ilpv4-connector/ilpv4-connector-persistence
mvn -DskipTests clean package -P liquibase-pg-sql liquibase:update
```

If you want to reinitialize the database, for example during development, the following commands can be used:

```bash
mvn -DskipTests clean package -P liquibase-pg-sql liquibase:dropAll
mvn -DskipTests clean package -P liquibase-pg-sql liquibase:update
```

## Oracle

_Coming soon._

## MySQL

_Coming soon._

## MS-SQL

_Coming soon._

## Redis

Redis is only used for balance tracking, so outside of configuring [runtime configuration properties](configuration.md), no further initialization is required.

