# DiningPlate REST API Specifications

This repository is the canonical source for all OpenAPI/Swagger REST API specifications in the DiningPlate platform. It is a multi-module Maven project — each module publishes its API contract as a Maven artifact that downstream services declare as a compile dependency.

## Modules

| Module | Artifact | Description |
|--------|----------|-------------|
| `order-rest-api-spec` | `com.diningplate:order-rest-api-spec` | Orders and menu endpoints |

## API Overview

### Order API (`order-rest-api-spec`)

Spec location: `order-rest-api-spec/src/main/resources/openapi/order-api.yaml`

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/orders` | Create a new order |
| `GET` | `/orders` | List orders (filterable by status, date; paginated) |
| `GET` | `/orders/{orderId}` | Get an order by ID |
| `DELETE` | `/orders/{orderId}` | Cancel an order |
| `PATCH` | `/orders/{orderId}/status` | Update order status |
| `GET` | `/menu` | List menu items (filterable by category, availability; paginated) |

**Order status lifecycle:** `NEW` → `ACCEPTED` → `PREPARING` → `DISPATCHED` → `COMPLETED`

## Build

```bash
# Build all modules and install to local Maven repo
mvn clean install

# Package (produces JARs)
mvn clean package
```

Requires **Java 17** and **Maven 3.x**.

## Consuming a Spec

Add the desired module as a Maven dependency:

```xml
<dependency>
  <groupId>com.diningplate</groupId>
  <artifactId>order-rest-api-spec</artifactId>
  <version>1.0.0</version>
</dependency>
```

The spec YAML files are packaged inside the JAR under `openapi/` and available on the classpath.

## Adding a New API Module

1. Create a new directory at the project root (e.g., `menu-api/`)
2. Add a `pom.xml` with `<parent>` pointing to `com.diningplate:diningplate-rest-api`
3. Register it in the root `pom.xml` under `<modules>`
4. Place spec files under `src/main/resources/openapi/` within the new module
5. Deploy with `mvn deploy` to make the artifact available to consumers
