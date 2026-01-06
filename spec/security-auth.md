# Security and Authentication Specification

## Overview

This document describes the security, authentication, and authorization model in ThingsBoard, including JWT tokens, access validation, permission enforcement, OAuth2, and MFA.

---

## Authentication Architecture

```mermaid
flowchart TD
  subgraph Clients
    UI[Web UI]
    API[API Client]
    Device[Device]
  end
  subgraph Auth
    JWT[JWT Auth]
    OAuth[OAuth2]
    MFA[MFA Service]
    Device_Auth[Device Auth]
  end
  subgraph Core
    AccessValidator
    TokenFactory
    CredentialsCache
  end
  UI --> JWT
  UI --> OAuth
  UI --> MFA
  API --> JWT
  Device --> Device_Auth
  JWT --> TokenFactory
  OAuth --> TokenFactory
  Device_Auth --> CredentialsCache
  TokenFactory --> AccessValidator
```

---

## User Authentication

### JWT Token Flow

```mermaid
sequenceDiagram
  participant Client
  participant API as REST API
  participant Auth as AuthService
  participant JWT as JwtTokenFactory
  Client->>API: POST /api/auth/login
  API->>Auth: authenticate(credentials)
  Auth->>JWT: createAccessToken(user)
  JWT-->>Auth: token
  Auth-->>API: token + refreshToken
  API-->>Client: 200 OK + tokens
```

### Token Types

| Token | Lifetime | Purpose |
|-------|----------|---------|
| Access Token | Short (15-30 min) | API authentication |
| Refresh Token | Long (hours/days) | Obtain new access tokens |

### Access Token Claims

| Claim | Description |
|-------|-------------|
| sub | User ID (UUID) |
| scopes | Authorities (e.g., TENANT_ADMIN) |
| userId | User ID |
| firstName | User first name |
| lastName | User last name |
| enabled | Account enabled flag |
| tenantId | Tenant ID |
| customerId | Customer ID (if customer user) |
| isPublic | Public user flag |
| iat | Issued at timestamp |
| exp | Expiration timestamp |

### Token Refresh

```mermaid
sequenceDiagram
  participant Client
  participant API
  participant JWT as JwtTokenFactory
  Note over Client: Access token expired
  Client->>API: POST /api/auth/token (refreshToken)
  API->>JWT: refreshTokens(refreshToken)
  JWT-->>API: new accessToken + refreshToken
  API-->>Client: New TokenPair
```

---

## Authorization

### Role-Based Access Control

| Role | Permissions |
|------|-------------|
| SYS_ADMIN | Full system access |
| TENANT_ADMIN | Full tenant access, manage users/devices/assets |
| CUSTOMER_USER | Access to assigned devices/assets only |

### Permission Operations

```java
public enum Operation {
    READ, WRITE, DELETE, 
    RPC_CALL, CLAIM, 
    READ_CREDENTIALS, WRITE_CREDENTIALS,
    READ_ATTRIBUTES, WRITE_ATTRIBUTES,
    READ_TELEMETRY, WRITE_TELEMETRY,
    ASSIGN_TO_CUSTOMER, UNASSIGN_FROM_CUSTOMER
}
```

### Permission Check Flow

```mermaid
flowchart TD
  Request[API Request] --> Extract[Extract User from JWT]
  Extract --> Validate[AccessValidator.validate]
  Validate --> CheckRole{Check Role}
  CheckRole -->|SYS_ADMIN| AllowAll[Allow All]
  CheckRole -->|TENANT_ADMIN| CheckTenant[Verify Tenant Ownership]
  CheckRole -->|CUSTOMER_USER| CheckAssignment[Verify Entity Assignment]
  CheckTenant --> Allow[Allow]
  CheckAssignment --> Allow
  CheckRole -->|Denied| Reject[403 Forbidden]
```

---

## Device Authentication

### Credential Types

| Type | Description |
|------|-------------|
| ACCESS_TOKEN | Simple token for HTTP/CoAP |
| X509_CERTIFICATE | Certificate auth for MQTT/CoAP |
| MQTT_BASIC | Username/password for MQTT |
| LWM2M_CREDENTIALS | LwM2M-specific credentials |

### Device Auth Flow

```mermaid
sequenceDiagram
  participant Device
  participant Transport as MQTT Broker
  participant Auth as DeviceAuthService
  participant Cache as CredentialsCache
  Device->>Transport: CONNECT(username, password)
  Transport->>Auth: authenticate(credentials)
  Auth->>Cache: lookup(credentialsId)
  Cache-->>Auth: DeviceCredentials
  Auth->>Auth: validate(credentials)
  Auth-->>Transport: DeviceInfo or UNAUTHORIZED
  Transport-->>Device: CONNACK
```

---

## Key Interfaces

### AccessValidator

| Method | Description |
|--------|-------------|
| validate(...) | Validate user access to an entity |
| validateEntityAndCallback(...) | Validate and invoke callback on result |

### TokenOutdatingService

| Method | Description |
|--------|-------------|
| isOutdated(token) | Check if a token has been invalidated |
| outdateOldUserTokens(userId) | Invalidate all tokens for a user |

---

## OAuth2 Integration

### Supported Providers

| Provider | Configuration |
|----------|---------------|
| Google | OAuth2 client credentials |
| GitHub | OAuth2 client credentials |
| Facebook | OAuth2 client credentials |
| Custom | OpenID Connect configuration |

### OAuth2 Flow

```mermaid
sequenceDiagram
  participant User
  participant TB as ThingsBoard
  participant IDP as Identity Provider
  User->>TB: GET /oauth2/authorize/{provider}
  TB-->>User: Redirect to IDP
  User->>IDP: Authenticate
  IDP-->>User: Redirect with code
  User->>TB: Callback with code
  TB->>IDP: Exchange code for token
  IDP-->>TB: Access token + user info
  TB->>TB: Create/update user
  TB-->>User: ThingsBoard JWT tokens
```

---

## Multi-Factor Authentication (MFA)

### Supported Methods

| Method | Description |
|--------|-------------|
| TOTP | Time-based one-time password (Google Auth) |
| SMS | SMS-based verification code |
| EMAIL | Email-based verification code |
| BACKUP_CODE | Pre-generated backup codes |

### MFA Flow

```mermaid
sequenceDiagram
  participant User
  participant API
  participant MFA as MfaService
  User->>API: Login with credentials
  API->>API: Validate credentials
  API-->>User: MFA required + preVerificationToken
  User->>API: POST /api/auth/mfa/verify
  API->>MFA: verify(token, code)
  MFA-->>API: Valid
  API-->>User: Full access JWT tokens
```

---

## Rate Limiting

| Limit Type | Description |
|------------|-------------|
| login.attempts | Max failed login attempts before lockout |
| login.lockout.duration | Account lockout duration (seconds) |
| api.rate.limit | Max API requests per time window |
| ws.rate.limit | WebSocket message rate limit |

---

## Security Headers

| Header | Value |
|--------|-------|
| X-Content-Type-Options | nosniff |
| X-Frame-Options | DENY |
| X-XSS-Protection | 1; mode=block |
| Content-Security-Policy | Configurable CSP policy |
| Strict-Transport-Security | max-age=31536000; includeSubDomains |

---

## Personal Access Tokens (PAT)

### Use Cases

- API automation without user credentials
- CI/CD pipeline integrations
- Third-party system access

### PAT Management

| Operation | Endpoint |
|-----------|----------|
| Create PAT | POST /api/user/personal-access-token |
| List PATs | GET /api/user/personal-access-tokens |
| Revoke PAT | DELETE /api/user/personal-access-token/{id} |

---

## Best Practices

### Do's

- Use short-lived access tokens with refresh
- Enforce MFA for admin accounts
- Scope API integrations with PATs
- Audit access and permission changes
- Use X.509 certificates for production devices
- Configure rate limiting for public endpoints

### Don'ts

- Don't store tokens in localStorage (use httpOnly cookies)
- Don't disable HTTPS in production
- Don't use shared device credentials
- Don't skip permission validation in custom code
- Don't expose admin endpoints publicly

---

## See Also

- [Transport Layer](transport-layer.md)
- [Device & Asset Management](device-asset-management.md)
- [Architecture Blueprint](architecture-blueprint.md)
