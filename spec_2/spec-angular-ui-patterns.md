---
title: Angular UI Module, Component & Service Patterns
version: 1.0
date_created: 2026-01-06
last_updated: 2026-01-06
owner: ThingsBoard Architecture Team
tags: [architecture, ui, angular, typescript, component, service]
---

# Introduction

This specification details technical implementation patterns for Angular UI modules, components, and services in ThingsBoard. It covers structure, state management, RxJS usage, and best practices for maintainable, scalable UI code.

## 1. Purpose & Scope

Defines how Angular UI code should be organized and implemented. Intended for frontend developers and architects.

## 2. Definitions

- **NgModule**: Angular module
- **Component**: UI element with template and logic
- **Service**: Injectable business/data logic provider
- **RxJS**: Reactive Extensions for async data streams
- **State Management**: Handling UI state via services and observables

## 3. Requirements, Constraints & Guidelines

- **REQ-001**: All business logic must be in services, not components
- **REQ-002**: Use RxJS for all async flows and state
- **REQ-003**: Use `takeUntil` or similar for subscription cleanup
- **CON-001**: Components must be stateless except for UI state
- **GUD-001**: Use Angular CLI for module/component/service generation
- **PAT-001**: Use Singleton pattern for services

## 4. Interfaces & Data Contracts

Example service:
```typescript
@Injectable()
export class DeviceService {
  getDevices(): Observable<Device[]> { ... }
}
```

Example component:
```typescript
@Component({ ... })
export class DeviceListComponent implements OnInit, OnDestroy {
  devices$: Observable<Device[]>;
  private destroy$ = new Subject<void>();
  ngOnInit() {
    this.devices$ = this.deviceService.getDevices().pipe(takeUntil(this.destroy$));
  }
  ngOnDestroy() {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

## 5. Acceptance Criteria

- **AC-001**: All async flows use RxJS and clean up subscriptions
- **AC-002**: All business logic is in services
- **AC-003**: All modules/components/services are generated via CLI

## 6. Test Automation Strategy

- **Unit tests**: Jasmine, Karma for components/services
- **E2E tests**: Cypress, Selenium for UI flows
- **Coverage**: 90%+ for UI logic

## 7. Rationale & Context

Reactive, service-driven UI code improves maintainability and scalability for complex dashboards and widgets.

## 8. Dependencies & External Integrations

- **INF-001**: Angular, RxJS
- **DAT-001**: REST APIs

## 9. Examples & Edge Cases

```typescript
// Edge case: Memory leak from unhandled subscription
ngOnInit() {
  this.devices$ = this.deviceService.getDevices(); // missing takeUntil
}
```

## 10. Validation Criteria

- All UI code passes unit and E2E tests
- All subscription cleanup paths are covered

## 11. Related Specifications / Further Reading

- [spec-architecture-blueprint.md](spec-architecture-blueprint.md)
- [ui-ngx.spec.md](ui-ngx.spec.md)
---
