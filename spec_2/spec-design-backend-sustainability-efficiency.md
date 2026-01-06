---
title: Backend Sustainability & Resource Efficiency
version: 1.0
date_created: 2026-01-06
owner: ThingsBoard Architecture Team
tags: [backend, design, efficiency]
---

# Introduction

Guidance for minimizing CPU, memory, and storage usage while maintaining service quality.

## 1. Purpose & Scope

Define practices for efficient data structures, algorithms, and workload management.

## 2. Definitions
- Efficiency Budget: Resource targets per service.
- Hot Path: Most frequently executed path requiring optimization.

## 3. Requirements, Constraints & Guidelines
- REQ-001: Establish efficiency budgets and monitor adherence.
- REQ-002: Optimize hot paths with measured improvements.
- GUD-001: Prefer backpressure to uncontrolled resource growth.

## 4. Interfaces & Data Contracts
Document payload sizes, batching policies, and compression expectations.

```mermaid
flowchart LR
    A[Measure resource use] --> B[Identify hot paths]
    B --> C[Optimize implementation]
    C --> D[Validate improvements]
    D --> E[Update budgets]
```

## 5. Acceptance Criteria
- AC-001: Budgets defined and met for key services.
- AC-002: Optimization gains demonstrated in metrics.

## 6. Test Automation Strategy
- Resource profiling tests; regression checks for budgets.

## 7. Rationale & Context
Efficiency reduces costs and environmental impact.

## 8. Dependencies & External Integrations
- Profilers; metrics systems; capacity planners.

## 9. Examples & Edge Cases
- Edge: Unbounded queue growth → apply backpressure and monitoring.

## 10. Validation Criteria
- Budgets met consistently; alerts not triggered under normal load.

## 11. Related Specifications / Further Reading
- [spec/spec-backend-service-implementation.md](spec/spec-backend-service-implementation.md)
