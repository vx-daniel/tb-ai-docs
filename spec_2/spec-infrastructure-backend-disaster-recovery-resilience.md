---
title: Backend Disaster Recovery & Resilience
version: 1.0
date_created: 2026-01-06
owner: ThingsBoard Architecture Team
tags: [backend, infrastructure, resilience]
---

# Introduction

Describes strategies for fault tolerance, failover, and recovery.

## 1. Purpose & Scope

Minimize downtime and data loss through resilient designs and tested procedures.

## 2. Definitions
- RTO: Recovery Time Objective.
- RPO: Recovery Point Objective.

## 3. Requirements, Constraints & Guidelines
- REQ-001: Define RTO/RPO targets per service.
- REQ-002: Implement fallbacks, retries, and circuit breaking.
- GUD-001: Prefer stateless services and externalized state.

## 4. Interfaces & Data Contracts
Health and status interfaces facilitate orchestration and failover.

```mermaid
flowchart LR
    A[Detect failure] --> B[Isolate fault]
    B --> C[Failover to standby]
    C --> D[Restore service]
    D --> E[Verify data integrity]
    E --> F[Postmortem & improve]
```

## 5. Acceptance Criteria
- AC-001: DR drills meet RTO/RPO targets.
- AC-002: Automatic failover verified in staging.

## 6. Test Automation Strategy
- Chaos tests; scheduled DR drills; integrity validation.

## 7. Rationale & Context
Structured resilience reduces impact of incidents.

## 8. Dependencies & External Integrations
- Orchestrators; stateful stores; backup services.

## 9. Examples & Edge Cases
- Edge: Split-brain during failover → quorum and fencing mechanisms.

## 10. Validation Criteria
- Drill reports archived; remediation actions tracked.

## 11. Related Specifications / Further Reading
- [spec/spec-backend-service-implementation.md](spec/spec-backend-service-implementation.md)
