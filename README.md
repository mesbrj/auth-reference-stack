# Ory Authentication and Authorization Stack

A modern authentication and authorization reference architecture using the complete Ory ecosystem for OAuth 2.0/OIDC, identity management, and fine-grained authorization.

## Stack Components

- **Ory [Hydra](https://www.ory.sh/hydra)** - OAuth 2.0 and OpenID Connect provider
- **Ory [Kratos](https://www.ory.sh/kratos)** - Identity and user management with authentication flows  
- **Ory [Keto](https://www.ory.sh/keto)** - Authorization server providing RBAC, ABAC, and ACL capabilities

## Architecture Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   API Gateway   │    │   Backend       │
│   Application   │────│     APISIX      │────│   Services      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │
                       ┌─────────────────┐
                       │  Ory Kratos     │ ← Identity Management
                       │  (Identity)     │ ← User Registration/Login
                       └─────────────────┘
                                │
                    ┌───────────┼───────────┐
                    │           │           │
          ┌─────────────────┐   │   ┌─────────────────┐
          │   Ory Hydra     │   │   │    Ory Keto     │
          │  (OAuth/OIDC)   │───┼───│ (Authorization) │
          │   JWT Tokens    │   │   │  Permissions    │
          └─────────────────┘   │   └─────────────────┘
                                │
                       ┌─────────────────┐
                       │   PostgreSQL    │ ← Persistent Storage
                       │   Database      │
                       └─────────────────┘
```

## Authentication Flows

### 1. **User Registration and Login Flow**
```
User → Kratos (Registration/Login UI) → Hydra (OAuth Consent) → JWT Tokens → Protected Resources
```

### 2. **API Authentication Flow**
```
Client App → Hydra (OAuth 2.0/OIDC) → JWT Access Tokens → API Gateway → Backend Services
```

### 3. **Authorization Flow**
```
Authenticated Request → Keto (Policy Check) → Allow/Deny → Resource Access
```

## Implementation Examples

### **API Gateway Integration**
- **[APISIX](https://apisix.apache.org/)** API Gateway with OAuth 2.0/OIDC authentication
  - JWT token validation via Ory Hydra
  - Policy-based routing and request filtering
  - Rate limiting and traffic management

### **Database Integration**
- **[PostgreSQL](https://www.postgresql.org/)** as primary data store
  - User identities and profiles (Kratos)
  - OAuth clients and tokens (Hydra)  
  - Permission policies and relations (Keto)

### **Microservices Integration**
- **RESTful APIs** with JWT bearer token authentication
- **gRPC services** with OAuth 2.0 client credentials flow
- **Event-driven architecture** with secure message publishing

## Key Features

### **Modern Authentication**
- ✅ **OAuth 2.0/OIDC Standards** - Industry-standard authorization and authentication protocols
- ✅ **JWT Tokens** - Stateless, secure token-based authentication
- ✅ **Multi-Factor Authentication** - TOTP, WebAuthn, and recovery codes via Kratos
- ✅ **Social Logins** - Google, GitHub, Facebook integration

### **Identity Management**
- ✅ **Self-Service Flows** - Registration, login, account recovery, and settings
- ✅ **Email Verification** - Secure account activation and email confirmation
- ✅ **Password Policies** - Configurable password strength and breach detection
- ✅ **User Profiles** - Flexible identity schema with custom attributes

### **Authorization & Security**
- ✅ **Fine-Grained Permissions** - Role and attribute-based access control via Keto
- ✅ **Policy-Based Authorization** - Relationship-based permission modeling
- ✅ **API Security** - Comprehensive OAuth 2.0 client and scope management
- ✅ **Audit Logging** - Complete authentication and authorization audit trails

## Quick Start

```bash
# Clone the repository
git clone <repository-url>
cd auth-reference-stack

# Start the complete stack
docker-compose up -d

# Verify services are running
docker-compose ps
```

## Documentation

- **[ARCHITECTURE.md](./ARCHITECTURE.md)** - Detailed system architecture and integration patterns
- **[DEPLOYMENT.md](./DEPLOYMENT.md)** - Deployment guide, configuration, and testing instructions
- **[configs/](./configs/)** - Service configuration files
- **[services/](./services/)** - Custom integration services

## Use Cases

### **Modern Web Applications**
Perfect for SPAs, mobile apps, and progressive web applications requiring secure user authentication and authorization.

### **API-First Architecture**
Ideal for microservices and API-driven applications with fine-grained access control and OAuth 2.0 client management.

### **Multi-Tenant Applications**
Supports complex permission models with tenant isolation and role-based access control across different organizational units.

## Contributing

This reference stack demonstrates integration patterns and can be adapted for specific organizational needs. See individual component documentation for detailed configuration options.
