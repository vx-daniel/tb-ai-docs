# ThingsBoard Module: dao

## Language and Context
- **Language:** Java
- **Context:** Data access layer for ThingsBoard, supporting both SQL and NoSQL backends.

## Structure and Key Components
- **DAOs:** Interfaces and implementations for CRUD operations on entities (Device, Asset, Tenant, etc.).
- **Async DAOs:** Use of `ListenableFuture` for non-blocking database operations.
- **Cache:** Entity and profile caches for performance.

## Example: CassandraAbstractAsyncDao
- **Purpose:** Abstract base for Cassandra DAOs, providing async CRUD methods.
- **Logic Flow:**
  1. Accepts queries and returns `ListenableFuture` for async processing.
  2. Uses callback executors for result handling.

## Complex Concepts
- **Async Data Access:** Non-blocking DB operations using Guava futures.
- **Cache Invalidation:** Ensures data consistency between cache and DB.

## Best Practices
- Use async DAOs for high-throughput operations.
- Invalidate caches on entity updates/deletes.

## Common Pitfalls
- Stale cache data if invalidation is missed.
- Unhandled async errors can cause data loss.

## Recommendations
- Monitor cache hit/miss rates.
- Add integration tests for cache and async flows.

---
See also: [thingsboard-architecture.spec.md](thingsboard-architecture.spec.md)
