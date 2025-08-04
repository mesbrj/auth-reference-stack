# Ory Stack Deployment Guide

This guide walks you through deploying and testing the complete Ory authentication and authorization stack.

## Quick Start

### 1. Start the Authentication Stack
```bash
# Clone and navigate to the repository
cd auth-reference-stack

# Start all services
docker-compose up -d

# Check service status
docker-compose ps
```

### 2. Verify Services are Running
```bash
# Check PostgreSQL
curl http://localhost:5432 # Should show connection info

# Check Ory Kratos
curl http://localhost:4433/health/ready

# Check Ory Hydra  
curl http://localhost:4444/health/ready

# Check Ory Keto
curl http://localhost:4466/health/ready

# Check Demo Service
curl http://localhost:8080/public/health
```

## Service Overview

### Core Services

| Service | Port | Purpose | Admin Port |
|---------|------|---------|------------|
| **Kratos** | 4433 | Identity Management | 4434 |
| **Hydra** | 4444 | OAuth 2.0/OIDC Provider | 4445 |
| **Keto** | 4466 | Authorization Server | 4467 |
| **APISIX** | 9080 | API Gateway | 9091 |
| **UI** | 3000 | Self-Service UI | - |
| **Demo** | 8080 | Example Backend | - |

### Supporting Services

| Service | Port | Purpose |
|---------|------|---------|
| **PostgreSQL** | 5432 | Database |
| **ETCD** | 2379 | APISIX Configuration |

## Testing Authentication Flows

### 1. User Registration and Login

#### Register a New User
```bash
# Get registration flow
curl -X GET http://localhost:4433/self-service/registration/api

# Or visit the UI
open http://localhost:3000/registration
```

#### Login with Existing User
```bash
# Get login flow
curl -X GET http://localhost:4433/self-service/login/api

# Or visit the UI  
open http://localhost:3000/login
```

### 2. OAuth 2.0 Flow Testing

#### Create OAuth Client
```bash
# Create a client in Hydra
curl -X POST http://localhost:4445/admin/clients \
  -H "Content-Type: application/json" \
  -d '{
    "client_id": "demo-client",
    "client_secret": "demo-secret", 
    "grant_types": ["authorization_code", "refresh_token"],
    "response_types": ["code"],
    "scope": "openid offline profile email",
    "redirect_uris": ["http://localhost:3000/callback"]
  }'
```

#### Authorization Code Flow
```bash
# Step 1: Get authorization code
open "http://localhost:4444/oauth2/auth?client_id=demo-client&response_type=code&scope=openid%20profile%20email&redirect_uri=http://localhost:3000/callback&state=some-random-state"

# Step 2: Exchange code for tokens (after getting code from callback)
curl -X POST http://localhost:4444/oauth2/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=authorization_code&code=AUTHORIZATION_CODE&redirect_uri=http://localhost:3000/callback&client_id=demo-client&client_secret=demo-secret"
```

### 3. Authorization Testing with Keto

#### Create Permission Relations
```bash
# Create a namespace
curl -X PUT http://localhost:4467/admin/relation-tuples \
  -H "Content-Type: application/json" \
  -d '{
    "namespace": "files",
    "object": "document-1", 
    "relation": "owner",
    "subject_id": "user-123"
  }'

# Grant read permission
curl -X PUT http://localhost:4467/admin/relation-tuples \
  -H "Content-Type: application/json" \
  -d '{
    "namespace": "files",
    "object": "document-1",
    "relation": "viewer", 
    "subject_set": {
      "namespace": "files",
      "object": "document-1", 
      "relation": "owner"
    }
  }'
```

#### Check Permissions
```bash
# Check if user can view document
curl -X POST http://localhost:4466/relation-tuples/check \
  -H "Content-Type: application/json" \
  -d '{
    "namespace": "files",
    "object": "document-1",
    "relation": "viewer",
    "subject_id": "user-123"
  }'
```

### 4. API Gateway Testing

#### Public Endpoints (No Authentication)
```bash
# Access public API
curl http://localhost:9080/public/health

# Should return without authentication
curl http://localhost:8080/public/health
```

#### Protected Endpoints (Requires OAuth Token)
```bash
# Try accessing without token (should fail)
curl http://localhost:9080/api/v1/protected

# Access with valid JWT token
curl -H "Authorization: Bearer YOUR_JWT_TOKEN" \
     http://localhost:9080/api/v1/protected
```

## Configuration

### Environment Variables

#### Kratos Configuration
```bash
export KRATOS_PUBLIC_URL=http://localhost:4433
export KRATOS_ADMIN_URL=http://localhost:4434
```

#### Hydra Configuration  
```bash
export HYDRA_PUBLIC_URL=http://localhost:4444
export HYDRA_ADMIN_URL=http://localhost:4445
```

#### Keto Configuration
```bash
export KETO_READ_URL=http://localhost:4466
export KETO_WRITE_URL=http://localhost:4467
```

### Database Migrations

#### Initialize Kratos Database
```bash
docker exec ory-kratos kratos migrate sql -e --yes
```

#### Initialize Hydra Database
```bash
docker exec ory-hydra hydra migrate sql -e --yes
```

#### Initialize Keto Database
```bash
docker exec ory-keto keto migrate up --yes
```

## Production Considerations

### Security Hardening

1. **Change Default Secrets**
   ```bash
   # Update in docker-compose.yml
   SECRETS_SYSTEM: "your-production-secret-32-chars"
   OIDC_SUBJECT_IDENTIFIERS_PAIRWISE_SALT: "your-salt"
   ```

2. **Enable HTTPS**
   ```bash
   # Add TLS certificates to APISIX
   # Configure HTTPS endpoints for all services
   ```

3. **Database Security**
   ```bash
   # Use strong passwords
   # Enable SSL/TLS for database connections
   # Restrict database access
   ```

### Scaling

1. **Horizontal Scaling**
   ```bash
   # Scale Kratos instances
   docker-compose up -d --scale kratos=3
   
   # Scale Hydra instances  
   docker-compose up -d --scale hydra=3
   ```

2. **Load Balancing**
   ```bash
   # Configure APISIX for load balancing
   # Add health checks
   # Implement circuit breakers
   ```

### Monitoring

1. **Health Checks**
   ```bash
   # Automated health monitoring
   curl http://localhost:4433/health/ready # Kratos
   curl http://localhost:4444/health/ready # Hydra
   curl http://localhost:4466/health/ready # Keto
   ```

2. **Metrics and Logging**
   ```bash
   # Configure Prometheus metrics
   # Set up centralized logging
   # Monitor authentication success/failure rates
   ```

## Troubleshooting

### Common Issues

#### Services Not Starting
```bash
# Check logs
docker-compose logs kratos
docker-compose logs hydra
docker-compose logs keto

# Check database connectivity
docker-compose logs postgres
```

#### Authentication Failures
```bash
# Check Kratos logs for identity issues
docker-compose logs kratos

# Verify database migrations
docker exec ory-kratos kratos migrate status
```

#### Authorization Issues
```bash
# Check Keto relations
curl http://localhost:4467/admin/relation-tuples

# Verify Keto configuration
docker-compose logs keto
```

### Reset Everything
```bash
# Stop and remove all containers and volumes
docker-compose down -v --remove-orphans

# Remove all images
docker-compose down --rmi all

# Start fresh
docker-compose up -d
```

## Development Workflow

### Making Configuration Changes
```bash
# Edit configuration files in configs/
vim configs/kratos/kratos.yml
vim configs/hydra/hydra.yml
vim configs/keto/keto.yml

# Restart specific service
docker-compose restart kratos
```

### Testing Custom Applications
```bash
# Add your application to docker-compose.yml
# Configure authentication via APISIX
# Test OAuth integration
```

### Integration Testing
```bash
# Run automated tests
# Test user registration flow
# Test OAuth token exchange
# Test permission checks
```

This deployment guide provides a complete walkthrough for setting up and testing the Ory authentication stack without the complexity of Kerberos integration.
