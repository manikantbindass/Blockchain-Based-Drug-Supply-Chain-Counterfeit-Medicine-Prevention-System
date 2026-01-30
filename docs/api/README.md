# API Documentation

This folder contains comprehensive REST API documentation for the drug supply chain platform.

## Purpose

Provides detailed specifications for all backend REST APIs, enabling frontend developers, third-party integrators, and testing teams to understand and consume the services effectively.

## Contents

### API Specification Document
File: `api-specification.md`

Comprehensive API documentation including:
- Base URLs and versioning strategy
- Authentication and authorization mechanisms
- Request/response formats and schemas
- Error handling and status codes
- Rate limiting policies
- Pagination strategies
- Filtering and sorting capabilities

### Postman Collection
File: `postman-collection.json`

Importable Postman collection containing:
- Sample requests for all endpoints
- Environment variables for different stages (dev, staging, production)
- Pre-request scripts for authentication
- Test assertions for validating responses
- Example payloads for batch registration, transfers, and verification

## API Design Philosophy

### RESTful Principles
- Resource-oriented URL structure
- Standard HTTP methods (GET, POST, PUT, PATCH, DELETE)
- Stateless communication
- JSON request and response format
- Proper use of HTTP status codes
- HATEOAS links for resource discovery

### Versioning Strategy
- URL-based versioning: `/api/v1/`, `/api/v2/`
- Backward compatibility maintained for one major version
- Deprecation warnings in response headers
- Clear migration guides for version upgrades

### Security
- JWT-based authentication for user sessions
- OAuth 2.0 for third-party integrations
- API keys for service-to-service communication
- Role-based access control (RBAC)
- Request signing for sensitive operations
- IP whitelisting for regulatory endpoints

## Core API Modules

### Batch Management API
Manage medicine batch lifecycle from manufacturing to consumption.

**Endpoints**:
- `POST /api/v1/batches` - Register new batch
- `GET /api/v1/batches/{batchId}` - Get batch details
- `GET /api/v1/batches` - List all batches with filters
- `PUT /api/v1/batches/{batchId}` - Update batch information
- `GET /api/v1/batches/{batchId}/history` - Complete audit trail

### Transfer API
Manage custody chain transfers between supply chain participants.

**Endpoints**:
- `POST /api/v1/transfers` - Initiate transfer
- `GET /api/v1/transfers/{transferId}` - Get transfer details
- `PUT /api/v1/transfers/{transferId}/accept` - Accept custody
- `PUT /api/v1/transfers/{transferId}/reject` - Reject transfer
- `GET /api/v1/batches/{batchId}/transfers` - Transfer history

### Verification API
Authenticity verification for consumers and stakeholders.

**Endpoints**:
- `POST /api/v1/verification/qr` - Verify by QR code scan
- `POST /api/v1/verification/batch` - Verify by batch ID
- `GET /api/v1/verification/{verificationId}` - Get verification record
- `POST /api/v1/verification/{verificationId}/certificate` - Generate certificate

### Regulator API
Regulatory oversight, audit, and compliance tools.

**Endpoints**:
- `GET /api/v1/regulator/batches` - Search all batches
- `GET /api/v1/regulator/recalls` - List recalls
- `POST /api/v1/regulator/recalls` - Initiate recall
- `GET /api/v1/regulator/audit/{batchId}` - Full audit trail
- `GET /api/v1/regulator/analytics` - Supply chain metrics

## Standards

### Request Headers
```
Content-Type: application/json
Authorization: Bearer {jwt_token}
X-API-Version: v1
X-Request-ID: {unique_request_id}
```

### Response Format
```json
{
  "success": true,
  "data": {},
  "metadata": {
    "timestamp": "2026-01-30T17:30:00Z",
    "requestId": "req-12345"
  }
}
```

### Error Response
```json
{
  "success": false,
  "error": {
    "code": "BATCH_NOT_FOUND",
    "message": "Batch with specified ID does not exist",
    "field": "batchId"
  },
  "metadata": {
    "timestamp": "2026-01-30T17:30:00Z",
    "requestId": "req-12345"
  }
}
```

## How This Fits Into The System

API documentation serves as:
- Contract between frontend and backend teams
- Integration guide for third-party systems
- Testing reference for QA teams
- Onboarding resource for new developers
- Compliance artifact demonstrating system capabilities

## Current Status

**Status**: Planned

API specification will evolve alongside backend implementation. Initial endpoints defined based on core use cases:
- Manufacturer batch registration
- Supply chain transfer workflows
- Consumer verification
- Regulatory audits

Postman collection will be continuously updated as new endpoints are added.
