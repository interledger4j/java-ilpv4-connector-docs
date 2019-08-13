# Project Testing

This project includes various tests to ensure the correctness of the product. 

Test are segmented into two broad categories, **Unit Tests** and **Integration Tests.** By default all unit tests are enabled, and _most_ Integration tests are also enabled, although certain classes of integration tests only run inside of the [CI environment](https://circleci.com/gh/sappenin/java-ilpv4-connector).

## Unit Tests

Each class in this implementation should have high levels of unit test coverage. Execution of these tests is orchestrated by the [Maven Surefire Plugin](https://maven.apache.org/surefire/maven-surefire-plugin/), and can be enabled or disabled via command line-switches as detailed below. By default, 

## Integration Tests

This project includes a single module housing all integration tests: [ilpv4-connector-it](https://github.com/sappenin/java-ilpv4-connector/tree/master/ilpv4-connector-it). By default, most integration tests execute in the local build environment, whereas _all_ Integration tests execute in the CI environment.

There are various types of Integration test:

* **Performance ITs**: Validate the performance characteristics of the product.
* **IlpOverHttp ITs**: Validate functionality of two or more nodes peering with each other using [ILP-over-Http](https://github.com/interledger/rfcs/blob/master/0035-ilp-over-http/0035-ilp-over-http.md).
* **Settlement ITs**: Validate functionality of the Settlement subsystems using multiple nodes.

## Project Build Switches

The following commands can be used to build the project while skipping various test suites.

### Run Default IT Suite

```text
mvn verify
```

### Skip Unit Tests

```text
mvn verify -DskipUTs
```

### Skip Integration Tests

```text
mvn verify -DskipITs
```

### Skip All Tests

```text
mvn verify -DskipTests
```

### Run Settlement ITs

```text
mvn verify -Psettlement
```

### Run Performance ITs

```text
mvn verify -Pperformance
```

