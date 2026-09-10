# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repository is the canonical source for all Swagger/OpenAPI REST API specifications in the DiningPlate platform. It is a multi-module Maven project. Each module publishes its API contracts as a Maven artifact — downstream services declare it as a compile dependency to consume the specs.

## Build Commands

```bash
# Build all modules and install to local Maven repo
mvn clean install

# Build without running tests
mvn clean install -DskipTests

# Run tests only
mvn test

# Run a single test class
mvn test -Dtest=MyTestClass

# Run a single test method
mvn test -Dtest=MyTestClass#myTestMethod

# Package (produces JARs)
mvn clean package
```

## Tech Stack

- **Java 17** (compiler target: `maven.compiler.release=17`)
- **JUnit 5.11.0** (Jupiter) for tests
- **Maven** for build and dependency management

## Architecture

This project acts as an API contract library, not a runnable service. The intended structure is:

- Each submodule corresponds to a bounded domain (e.g., `menu-api`, `order-api`)
- Each module contains Swagger/OpenAPI YAML/JSON spec files under `src/main/resources/`
- Modules are published via `mvn deploy` and consumed by service projects as:

```xml
<dependency>
  <groupId>com.diningplate</groupId>
  <artifactId>some-domain-api</artifactId>
  <version>...</version>
</dependency>
```

The parent `pom.xml` (group `com.diningplate`, artifact `diningplate-rest-api`) acts as the aggregator and should declare submodules via `<modules>`. Plugin versions are pinned in `<pluginManagement>` in the parent so all submodules inherit consistent build behavior.

## Adding a New API Module

1. Create a new directory at the project root (e.g., `menu-api/`)
2. Add a `pom.xml` with `<parent>` pointing to `com.diningplate:diningplate-rest-api`
3. Register it in the root `pom.xml` under `<modules>`
4. Place spec files under `src/main/resources/` within the new module
