# Documentation Index

This directory contains comprehensive documentation for the Ory + APISIX Authentication Stack.

## 📖 Available Documentation

### OAuth 2.0 Reference
- **[oauth2-flows.md](oauth2-flows.md)** - Complete guide to OAuth 2.0 flows including:
  - Authorization Code Flow
  - Authorization Code Flow with PKCE
  - Client Credentials Flow
  - Device Authorization Flow
  - Implicit Flow (deprecated)
  - Resource Owner Password Credentials Flow
  - Refresh Token Flow
  - Security considerations and best practices

- **[oauth2-diagrams.md](oauth2-diagrams.md)** - Visual reference with:
  - Mermaid sequence diagrams for each flow
  - Security threat models
  - Implementation checklists
  - Flow selection decision trees

### Quick Reference

#### Most Common Flows
1. **Authorization Code + PKCE** - For SPAs, mobile apps, and public clients
2. **Authorization Code** - For traditional web applications with server-side backend
3. **Client Credentials** - For machine-to-machine authentication
4. **Device Authorization** - For IoT devices, smart TVs, and limited-input devices

#### Security Recommendations
- ✅ Always use HTTPS in production
- ✅ Implement PKCE for public clients
- ✅ Use short-lived access tokens (15-60 minutes)
- ✅ Store refresh tokens securely
- ✅ Validate redirect URIs strictly
- ✅ Implement proper CSRF protection

#### Token Storage Best Practices
- **Web Apps**: Server-side sessions, HTTP-only cookies
- **SPAs**: Memory storage for access tokens, HTTP-only cookies for refresh tokens
- **Mobile Apps**: Platform secure keystores
- **Desktop Apps**: OS-specific secure storage

## 🔗 External Resources

### Official Documentation
- [OAuth 2.0 RFC 6749](https://tools.ietf.org/html/rfc6749)
- [OAuth 2.1 Draft](https://datatracker.ietf.org/doc/draft-ietf-oauth-v2-1/)
- [PKCE RFC 7636](https://tools.ietf.org/html/rfc7636)
- [OpenID Connect Core](https://openid.net/specs/openid-connect-core-1_0.html)

### Ory Documentation
- [Ory Hydra Documentation](https://www.ory.sh/docs/hydra/)
- [Ory Kratos Documentation](https://www.ory.sh/docs/kratos/)
- [Ory Keto Documentation](https://www.ory.sh/docs/keto/)

### APISIX Documentation
- [Apache APISIX Documentation](https://apisix.apache.org/docs/)
- [APISIX OAuth Plugin](https://apisix.apache.org/docs/apisix/plugins/oauth/)
