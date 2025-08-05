# Authentication and Authorization Stack

A modern, production-ready authentication and authorization reference architecture using the Ory ecosystem.

### **OAuth 2.0 & OpenID Connect**
**Ory [Hydra](https://www.ory.sh/hydra)** - OAuth 2.0 and OpenID Connect provider for token-based authentication
- **OAuth 2.0 Flows** - Authorization Code, Client Credentials, Device Flow
- **OpenID Connect** - Certified OIDC implementation with ID tokens
- **JWT Tokens** - Stateless authentication with configurable claims
- **Token Management** - Refresh tokens, introspection, revocation
- **Client Management** - Dynamic client registration and management
- **Consent Management** - Granular scope and audience control

### **Authentication & Identity**
**Ory [Kratos](https://www.ory.sh/kratos)** - Identity and user management with self-service flows
- **Self-Service Flows** - Registration, login, logout, account recovery
- **Multi-Factor Authentication** - TOTP, WebAuthn (FIDO2), SMS, Email
- **Social Logins** - Google, GitHub, Facebook, Microsoft, and more
- **Password Security** - Breach detection, complexity rules, secure hashing
- **Email Verification** - Account activation and email confirmation
- **Account Recovery** - Password reset and account unlock flows

### **Authorization & Permissions**
**Ory [Keto](https://www.ory.sh/keto)** - Fine-grained authorization server with relationship-based access control
- **Fine-Grained Access Control** - Resource-level permission management
- **Relationship-Based Permissions** - Google Zanzibar-inspired model
- **Policy Engine** - Flexible rule-based authorization
- **Namespace Organization** - Multi-tenant permission isolation
- **Real-Time Evaluation** - High-performance permission checking
- **Audit Trail** - Complete authorization decision logging

# Implementation Examples

### (1) Java web application using [**Spring Authorization Server**](https://docs.spring.io/spring-authorization-server/reference/overview.html) leveraging Ory Hydra for OAuth 2.0 and OpenID Connect
- **Spring Boot** - Modern Java web application framework
- **Spring Security** - Comprehensive security framework for authentication and authorization
- **OAuth 2.0 Client** - Integrates with Ory Hydra for token management
- **OpenID Connect** - Supports OIDC flows for user authentication
- **JWT Tokens** - Uses Ory Hydra for stateless authentication
- **Consent Management** - Handles user consent for scopes and permissions 

### (2) [**APISIX**](https://apisix.apache.org/) as a centralized API Gateway for security, routing, and traffic management.

- **OAuth Integration** - Native OAuth 2.0/OIDC token validation
- **JWT Verification** - Automatic token signature and claims validation
- **Rate Limiting** - Per-client and global rate limiting policies
- **Load Balancing** - Multiple algorithms with health checks
- **SSL/TLS Termination** - Centralized certificate management
- **Request/Response Transformation** - Header manipulation and data transformation

---

#### Enterprise Kerberos Authentication integration with [**kerby**](https://directory.apache.org/kerby/)

The Ory ecosystem (with [Polis](https://www.ory.sh/polis)) is unaware of anything beyond [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language).

[SASL](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer), [GSSAPI](https://en.wikipedia.org/wiki/GSSAPI), [SPNEGO](https://en.wikipedia.org/wiki/SPNEGO) are completely alien technologies.

Kerberos is extremely used in Enterprise environments and products like: **Kafka**, **PostgreSQL**, **mariaDB**, **Hadoop(HDFS/YARN)**, **ElasticSearch** (to name a few), has at least GSSAPI or SPNEGO support.

The [kerby](https://directory.apache.org/kerby/) project provides a complete Kerberos and KDC (AS/TGS) implementation, without: LDAP, DNS, keytab for service principal authentication, TGT and service tickets grants. Only a kerberos implementation focused on modern applications and environments, *some features to focus on*:
- KDC with: in-memory or Mavibot(MVCC BTree) or JSON backends to store data (principals and keys).
- Preauth mechanism using JWT or OTP mechanism to request TGT and Service-Tickets.
- SASL framework support

---

### Ways to explore, test and develop the Kerby realm usage

- **(3) Kafka** with **SASL/GSSAPI** authentication. **Clients (consumers/producers)** using kerberos authentication to connect to Kafka brokers. **Golang** and **Python** clients (one consumer other producer). Testing **kerberos client bindings** and **SASL support**.
- [**(4) wildfly**](https://wildfly.org/) Java GSS-API [**Jakarta EE**](https://jakarta.ee/learn/docs/jakartaee-tutorial/current/security/security-intro/security-intro.html) web service with Kerberos SSO.

### Ways to explore, test and develop the Kerby and Ory integration

- **(5)** Oauth Authorization flows, Federation.
- **(6) Ory Kratos integration**: Managing the **lifecycle of kerberos principals** (services and users) and keys (based on current passwords) on kerby realm.

---

*Kerberos observations*

- Kerby *"complete implementation"* (with/without Ory ?) are able to trying a **Realm-as-a-Service** model ? One KDC for multiple tenants or one KDC per tenant ?

Positivive points about Kerberos:


- **Service Session key** - Each client/server connection has its own session key, providing per-connection security (each Kerberos client connected to a MariaDB has its own session key, for example).
- **Mutual Authentication** - Both client and server verify each other's identity.
- **KDC Centralization** - In a Kerberos service/protocol/realm, only KDC servers have TCP/UDP listening ports (for Kerberos). When the client from a file-server or DB "kerberized" has aquired a service-ticket from a KDC using his valid TGT (to access the file-server or DB), the service-ticket is used in the authentication process\step from a given service\protocol (file-share or DB connection, for example).
- **Cross-Platform Support** - Widely supported across various operating systems and applications.
- **Strong Security** - Encrypted tickets and time-limited credentials.
- **Single Sign-On** - Principals authenticate once and gain access to multiple services without re-entering credentials.

---

## Oauth 2.0 & OpenID Connect Resources

- **[OAuth 2.0 Flows Guide](docs/oauth2-flows.md)** - Comprehensive guide to all OAuth 2.0 flows with examples
- **[OAuth 2.0 Visual Diagrams](docs/oauth2-diagrams.md)** - Visual flowcharts and security considerations
>
- **[Security Best Practices](docs/oauth2-flows.md#security-considerations)** - OAuth 2.0 security guidelines
- **[Token Management](docs/oauth2-flows.md#token-storage-recommendations)** - Secure token storage and handling

