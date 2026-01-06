---
title: Testing & CI/CD Automation Details
version: 1.0
date_created: 2026-01-06
last_updated: 2026-01-06
owner: ThingsBoard Architecture Team
tags: [architecture, testing, ci, cd, automation, quality]
---

# Introduction

This specification details technical patterns for automated testing and CI/CD in ThingsBoard, covering test levels, frameworks, pipelines, and quality gates.

## 1. Purpose & Scope

Defines how automated tests and CI/CD pipelines should be structured and integrated. Intended for all developers and DevOps engineers.

## 2. Definitions

- **Unit Test**: Tests individual functions/classes
- **Integration Test**: Tests interactions between modules/services
- **E2E Test**: Tests full user flows
- **CI/CD**: Continuous Integration/Continuous Deployment
- **Quality Gate**: Minimum requirements for code merge/deploy

## 3. Requirements, Constraints & Guidelines

- **REQ-001**: All code must have unit, integration, and E2E tests
- **REQ-002**: CI pipelines must run all tests and enforce coverage gates
- **REQ-003**: All test data must be isolated and cleaned up
- **CON-001**: No manual deployment to production
- **GUD-001**: Use mocks/stubs for external dependencies
- **PAT-001**: Use pipeline templates for repeatable automation

## 4. Interfaces & Data Contracts

Example CI pipeline (GitHub Actions):
```yaml
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up JDK
        uses: actions/setup-java@v2
        with:
          java-version: '17'
      - name: Build and test
        run: mvn clean verify
      - name: Run UI tests
        run: yarn test
      - name: Check coverage
        run: mvn jacoco:report
```

## 5. Acceptance Criteria

- **AC-001**: All code changes pass all test levels in CI
- **AC-002**: Coverage gates are enforced before merge/deploy
- **AC-003**: All test data is cleaned up after tests

## 6. Test Automation Strategy

- **Unit**: JUnit (Java), Jasmine (Angular)
- **Integration**: Spring Test, in-memory DBs, mock services
- **E2E**: Cypress, Selenium
- **Coverage**: 80%+ for all modules
- **Quality Gates**: SonarQube, coverage checks

## 7. Rationale & Context

Automated testing and CI/CD ensure reliability, maintainability, and rapid delivery for IoT workloads.

## 8. Dependencies & External Integrations

- **INF-001**: GitHub Actions, Jenkins, SonarQube
- **DAT-001**: Test DBs, mock services

## 9. Examples & Edge Cases

```yaml
# Edge case: Flaky E2E test
- name: Retry failed tests
  run: yarn test --retry
```

## 10. Validation Criteria

- All pipelines pass all test levels and quality gates
- All test cleanup paths are covered

## 11. Related Specifications / Further Reading

- [spec-architecture-blueprint.md](spec-architecture-blueprint.md)
---
