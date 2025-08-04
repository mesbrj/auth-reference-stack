# Ory Authentication and Authorization Stack

A modern, production-ready authentication and authorization reference architecture using the Ory ecosystem with APISIX as a centralized API Gateway for security, routing, and traffic management.

## Stack Components

- **Ory [Hydra](https://www.ory.sh/hydra)** - OAuth 2.0 and OpenID Connect provider for token-based authentication
- **Ory [Kratos](https://www.ory.sh/kratos)** - Identity and user management with self-service flows
- **Ory [Keto](https://www.ory.sh/keto)** - Fine-grained authorization server with relationship-based access control
- **Apache [APISIX](https://apisix.apache.org/)** - High-performance API Gateway with OAuth integration

## Architecture Overview

```
              ┌─────────────────┐
              │   API Gateway   │
              │     APISIX      │
              └─────────────────┘
                        │
              ┌─────────────────┐
              │   Ory Hydra     │
              │  (OAuth/OIDC)   │
              │   JWT Tokens    │
              └─────────────────┘
                        │
            ┌──────────────────────┐
            │                      │
  ┌─────────────────┐    ┌─────────────────┐
  │  Ory Kratos     │    │    Ory Keto     │
  │  (Identity)     │    │ (Authorization) │
  └─────────────────┘    │  Permissions    │
                         └─────────────────┘
```

## Key Features

### 🔐 **Authentication & Identity (Kratos)**
- ✅ **Self-Service Flows** - Registration, login, logout, account recovery
- ✅ **Multi-Factor Authentication** - TOTP, WebAuthn (FIDO2), SMS, Email
- ✅ **Social Logins** - Google, GitHub, Facebook, Microsoft, and more
- ✅ **Password Security** - Breach detection, complexity rules, secure hashing
- ✅ **Email Verification** - Account activation and email confirmation
- ✅ **Account Recovery** - Password reset and account unlock flows

### 🎫 **OAuth 2.0 & OpenID Connect (Hydra)**
- ✅ **OAuth 2.0 Flows** - Authorization Code, Client Credentials, Device Flow
- ✅ **OpenID Connect** - Certified OIDC implementation with ID tokens
- ✅ **JWT Tokens** - Stateless authentication with configurable claims
- ✅ **Token Management** - Refresh tokens, introspection, revocation
- ✅ **Client Management** - Dynamic client registration and management
- ✅ **Consent Management** - Granular scope and audience control

### 🛡️ **Authorization & Permissions (Keto)**
- ✅ **Fine-Grained Access Control** - Resource-level permission management
- ✅ **Relationship-Based Permissions** - Google Zanzibar-inspired model
- ✅ **Policy Engine** - Flexible rule-based authorization
- ✅ **Namespace Organization** - Multi-tenant permission isolation
- ✅ **Real-Time Evaluation** - High-performance permission checking
- ✅ **Audit Trail** - Complete authorization decision logging

### 🚪 **API Gateway & Security (APISIX)**
- ✅ **OAuth Integration** - Native OAuth 2.0/OIDC token validation
- ✅ **JWT Verification** - Automatic token signature and claims validation
- ✅ **Rate Limiting** - Per-client and global rate limiting policies
- ✅ **Load Balancing** - Multiple algorithms with health checks
- ✅ **SSL/TLS Termination** - Centralized certificate management
- ✅ **Request/Response Transformation** - Header manipulation and data transformation
---
- **[OAuth 2.0 Flows Guide](docs/oauth2-flows.md)** - Comprehensive guide to all OAuth 2.0 flows with examples
- **[OAuth 2.0 Visual Diagrams](docs/oauth2-diagrams.md)** - Visual flowcharts and security considerations
>
- **[Security Best Practices](docs/oauth2-flows.md#security-considerations)** - OAuth 2.0 security guidelines
- **[Token Management](docs/oauth2-flows.md#token-storage-recommendations)** - Secure token storage and handling

