# Authentication and Authorization Stack using the Ory ecosystem.

## **OAuth 2.0 & OpenID Connect**
**Ory [Hydra](https://www.ory.sh/hydra)** - OAuth 2.0 and OpenID Connect provider for token-based authentication
- **OAuth 2.0 Flows** - Authorization Code, Client Credentials, Device Flow
- **OpenID Connect** - Certified OIDC implementation with ID tokens
- **JWT Tokens** - Stateless authentication with configurable claims
- **Token Management** - Refresh tokens, introspection, revocation
- **Client Management** - Dynamic client registration and management
- **Consent Management** - Granular scope and audience control

## **Authorization & Permissions**
**Ory [Keto](https://www.ory.sh/keto)** - Fine-grained authorization server with relationship-based access control
- **Fine-Grained Access Control** - Resource-level permission management
- **Relationship-Based Permissions** - Google Zanzibar-inspired model
- **Policy Engine** - Flexible rule-based authorization
- **Namespace Organization** - Multi-tenant permission isolation
- **Real-Time Evaluation** - High-performance permission checking
- **Audit Trail** - Complete authorization decision logging

## **Access Proxy & Gatekeeper**
**Ory [Oathkeeper](https://www.ory.sh/oathkeeper)** - Identity and access proxy for enforcing authentication and authorization
- **Identity & Access Proxy (IAP)** - Act as a reverse proxy or interface with API gateways. Authenticate, authorize and mutate any incoming network traffic
- **Zero Trust** - *BeyondCorp-style* model for secure access
- **Pluggable Authenticators** - Support for various authentication methods

## **Authentication & Identity**
**Ory [Kratos](https://www.ory.sh/kratos)** - Identity and user management with self-service flows
- **Self-Service Flows** - Registration, login, logout, account recovery
- **Multi-Factor Authentication** - TOTP, WebAuthn (FIDO2), SMS, Email
- **Social Logins** - Google, GitHub, Facebook, Microsoft, and more
- **Password Security** - Breach detection, complexity rules, secure hashing
- **Email Verification** - Account activation and email confirmation
- **Account Recovery** - Password reset and account unlock flows

## **Enterprise Identity Integration**
**Ory [Polis](https://www.ory.sh/polis)** - Enterprise single sign-on (SSO) and identity federation
- **SAML & OpenID Connect** - Support for enterprise identity providers
- **Directory Synchronization (SCIM)** - Provisioning/deprovisioning for identity lifecycle management
- **Identity Provider discovery** - Seamless SSO experience across multiple IdPs

## Oauth 2.0 & OpenID Connect Resources

- **[OAuth 2.0 Flows Guide](docs/oauth2-flows.md)** - Comprehensive guide to all OAuth 2.0 flows with examples
- **[OAuth 2.0 Visual Diagrams](docs/oauth2-diagrams.md)** - Visual flowcharts and security considerations
>
- **[Security Best Practices](docs/oauth2-flows.md#security-considerations)** - OAuth 2.0 security guidelines
- **[Token Management](docs/oauth2-flows.md#token-storage-recommendations)** - Secure token storage and handling
