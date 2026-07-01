# LifePulse Backend

Java Spring Boot backend for local-services discovery, voucher purchasing, and flash-sale ordering under high read traffic.

## Business Problem

Local-services platforms need to support merchant browsing, voucher discovery, order creation, and promotional flash-sale traffic without overwhelming the database or overselling inventory. LifePulse focuses on the backend systems behind that experience: API design, caching, concurrency control, and reliable order workflows.

This repository rebuilds an existing Java takeout-domain codebase into a portfolio-facing backend systems project.

## Product Scope

LifePulse models a local living platform with:

- merchant and shop discovery
- item and voucher browsing
- user authentication
- order creation and status tracking
- flash-sale purchase flows
- AI-assisted merchant workflows as an extension point

## Current Codebase

The current repository is a multi-module Spring Boot project:

- `sky-common`: shared utilities, interceptors, configuration helpers, and infrastructure code
- `sky-pojo`: entities, DTOs, and VOs for user, shop, category, dish, order, and related objects
- `sky-server`: Spring Boot runtime, controllers, mappers, configuration, and application entrypoint

The next rebuild pass will gradually rename and package the public narrative around LifePulse while preserving useful Java backend implementation pieces.

## Target Architecture

```text
client / API caller
        |
        v
Spring MVC controllers
        |
        v
service layer
  - merchant discovery
  - voucher purchase
  - order lifecycle
  - flash-sale validation
        |
        v
persistence + cache
  - MySQL / MyBatis
  - Caffeine local cache
  - Redis distributed cache
        |
        v
concurrency controls
  - Bloom filters
  - cache-aside strategy
  - Redis Lua scripts
  - Redisson locks
```

## Tech Stack

- Java
- Spring Boot
- Spring MVC
- MyBatis
- MySQL
- Redis
- Caffeine
- Redisson
- Redis Lua
- Nginx
- JWT
- JMeter
- Docker / Docker Compose planned

## Backend Focus Areas

### Multi-level caching

- Caffeine for hot local reads.
- Redis for shared cache across application instances.
- Cache-aside strategy for predictable invalidation.
- Bloom filters to reduce penetration from invalid keys.

### Flash-sale concurrency

- Redis Lua scripts for atomic stock and user checks.
- Redisson locks for distributed mutual exclusion where needed.
- One-order-per-user protection.
- Oversell prevention during concurrent purchase attempts.

### Order workflow

- Order creation and status transitions.
- Persistence through MyBatis mappers.
- Structured service layer for business rules.
- Roadmap for async order processing and retryable status updates.

## Planned Metrics

Only measured metrics should be used in resumes or public claims. Current target metrics to verify:

- cache hit rate under repeated read traffic
- DB read reduction after Caffeine + Redis caching
- QPS under JMeter flash-sale load
- p95 latency during concurrent purchase tests
- oversell count under stress tests

## Local Setup

Use JDK 17 for this project. The Maven compiler target is Java 17, and local
verification currently passes with Homebrew `openjdk@17`. Newer JDKs can fail
because the Spring Boot / Lombok toolchain in this rebuild is pinned to the
Java 17 era.

From the repository root:

```sh
mvn -DskipTests install
cd sky-server
mvn spring-boot:run
```

On this machine, the verified command is:

```sh
JAVA_HOME=/opt/homebrew/Cellar/openjdk@17/17.0.18/libexec/openjdk.jdk/Contents/Home \
PATH="$JAVA_HOME/bin:$PATH" \
mvn -DskipTests install
```

The repository includes environment-specific configuration under:

```text
sky-server/src/main/resources
```

## Rebuild Roadmap

### Phase 1: Portfolio Packaging

- Rename public README and docs from OrderFlow to LifePulse.
- Document the real module structure and existing run path.
- Add API examples for merchant, voucher, and order flows.

### Phase 2: Caching and Concurrency

- Add or verify Redis cache flows.
- Add JMeter load-test scenarios.
- Document cache hit rate, DB read reduction, QPS, and p95 latency.
- Add failure cases for duplicate orders and oversell prevention.

### Phase 3: Reliability Polish

- Add service-level tests for critical workflows.
- Add Docker Compose for MySQL and Redis.
- Add CI checks.
- Add architecture diagram and demo screenshots.

## Interview Narrative

LifePulse demonstrates Java backend fundamentals for SDE interviews: layered Spring Boot architecture, MyBatis persistence, Redis caching, flash-sale concurrency, Lua-based atomic operations, and load-test-driven performance discussion.

## Repository Note

This project is being rebuilt from the earlier `orderflow` repository. The old name may still appear in module names or supporting docs while the portfolio packaging is updated.
