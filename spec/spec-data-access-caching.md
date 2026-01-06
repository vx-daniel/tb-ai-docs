---
title: Data Access & Caching Strategies
version: 1.0
date_created: 2026-01-06
last_updated: 2026-01-06
owner: ThingsBoard Architecture Team
tags: [architecture, dao, cache, data, java, performance]
---

# Introduction

This specification details technical patterns for data access and caching in ThingsBoard, focusing on DAOs, async operations, and cache invalidation for performance and consistency.

## 1. Purpose & Scope

Defines how DAOs and caches should be implemented and integrated. Intended for backend developers and architects.

## 2. Definitions

- **DAO**: Data Access Object
- **Cache**: In-memory store for entities/profiles
- **Async DAO**: DAO using ListenableFuture for non-blocking operations
- **Entity**: Persistent domain object

## 3. Requirements, Constraints & Guidelines

- **REQ-001**: All DAOs must provide async CRUD methods
- **REQ-002**: Caches must be invalidated on entity update/delete
- **REQ-003**: DAOs must not leak database exceptions
- **CON-001**: Caches must be thread-safe
- **GUD-001**: Use Guava or Caffeine for cache implementation
- **PAT-001**: Use Factory pattern for DAO instantiation

## 4. Interfaces & Data Contracts

Example async DAO:
```java
public interface DeviceDao {
    ListenableFuture<Device> findByIdAsync(UUID id);
    ListenableFuture<List<Device>> findAllAsync(PageLink pageLink);
    ListenableFuture<Void> saveAsync(Device device);
    ListenableFuture<Void> deleteAsync(UUID id);
}
```

Example cache:
```java
public class DeviceCache {
    private final LoadingCache<UUID, Device> cache;
    // ...methods for get, invalidate, refresh...
}
```

## 5. Acceptance Criteria

- **AC-001**: All DAOs provide async methods
- **AC-002**: All caches are invalidated on update/delete
- **AC-003**: All cache and DAO errors are handled

## 6. Test Automation Strategy

- **Unit tests**: JUnit, Mockito for DAO/cache logic
- **Integration tests**: DB and cache consistency checks
- **Coverage**: 90%+ for DAO/cache methods

## 7. Rationale & Context

Async DAOs and robust caching improve throughput and scalability for IoT workloads.

## 8. Dependencies & External Integrations

- **INF-001**: Guava, Caffeine (cache)
- **DAT-001**: SQL/NoSQL DBs

## 9. Examples & Edge Cases

```java
// Edge case: Stale cache
public void updateDevice(Device device) {
    deviceDao.saveAsync(device);
    deviceCache.invalidate(device.getId());
}
```

## 10. Validation Criteria

- All DAOs and caches pass unit and integration tests
- All cache invalidation paths are covered

## 11. Related Specifications / Further Reading

- [spec-architecture-blueprint.md](spec-architecture-blueprint.md)
- [dao.spec.md](dao.spec.md)
---
