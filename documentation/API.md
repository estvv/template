# API Documentation

## Overview

{{API_OVERVIEW}}

**Base URL:** `{{BASE_URL}}`

**Version:** `{{API_VERSION}}`

**Protocol:** {{PROTOCOL}} (REST / GraphQL / gRPC)

## Authentication

### Authentication Method

{{AUTH_METHOD_DESCRIPTION}}

**Type:** {{AUTH_TYPE}} (Bearer Token / API Key / Session / OAuth2)

### Authentication Headers

```http
Authorization: Bearer {{TOKEN}}
X-API-Key: {{API_KEY}}
```

### Authentication Endpoints

#### Login

```http
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:**

```json
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "refresh_token_here",
    "expiresIn": 3600
  }
}
```

#### Refresh Token

```http
POST /auth/refresh
Content-Type: application/json

{
  "refreshToken": "refresh_token_here"
}
```

### Error Handling

#### Invalid Credentials (401)

```json
{
  "success": false,
  "error": {
    "code": "INVALID_CREDENTIALS",
    "message": "Invalid email or password"
  }
}
```

#### Expired Token (401)

```json
{
  "success": false,
  "error": {
    "code": "TOKEN_EXPIRED",
    "message": "Token has expired"
  }
}
```

#### Insufficient Permissions (403)

```json
{
  "success": false,
  "error": {
    "code": "FORBIDDEN",
    "message": "You do not have permission to access this resource"
  }
}
```

## Request/Response Format

### Standard Response Format

```json
{
  "success": true,
  "data": {
    // Response data
  },
  "error": null,
  "meta": {
    "page": 1,
    "total": 100,
    "perPage": 20
  }
}
```

### Error Response Format

```json
{
  "success": false,
  "data": null,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable error message",
    "details": {
      "field": "email",
      "reason": "invalid format"
    },
    "requestId": "uuid-for-tracking"
  },
  "meta": null
}
```

## Pagination

### Query Parameters

```
GET /resource?page=1&perPage=20&sort=createdAt&order=desc
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `page` | integer | 1 | Page number |
| `perPage` | integer | 20 | Items per page (max: 100) |
| `sort` | string | createdAt | Sort field |
| `order` | string | desc | Sort order (asc/desc) |

### Response with Pagination

```json
{
  "success": true,
  "data": {
    "items": [],
    "pagination": {
      "page": 1,
      "perPage": 20,
      "total": 100,
      "totalPages": 5
    }
  }
}
```

## Filtering

### Filter Parameters

```
GET /users?status=active&role=admin&createdAt>2024-01-01
```

| Operator | Description |
|----------|-------------|
| `=` | Equals |
| `!=` | Not equals |
| `>` | Greater than |
| `>=` | Greater than or equal |
| `<` | Less than |
| `<=` | Less than or equal |
| `~` | Contains (substring) |
| `^` | Starts with |
| `$` | Ends with |

## Rate Limiting

### Rate Limit Headers

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 99
X-RateLimit-Reset: 1704067200
```

### Rate Limit Response (429)

```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded. Try again in 60 seconds.",
    "details": {
      "limit": 100,
      "remaining": 0,
      "resetAt": "2024-01-01T00:00:00Z"
    }
  }
}
```

## Resources

### {{RESOURCE_NAME_1}}

#### List {{RESOURCE_NAME_1}}

```http
GET /{{RESOURCE_ENDPOINT_1}}
Authorization: Bearer {{TOKEN}}
```

**Query Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `{{PARAM_1}}` | {{TYPE_1}} | {{REQUIRED_1}} | {{DESCRIPTION_1}} |
| `{{PARAM_2}}` | {{TYPE_2}} | {{REQUIRED_2}} | {{DESCRIPTION_2}} |

**Response:**

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "uuid",
        "field1": "value1",
        "field2": "value2",
        "createdAt": "2024-01-01T00:00:00Z",
        "updatedAt": "2024-01-01T00:00:00Z"
      }
    ]
  }
}
```

#### Get {{RESOURCE_NAME_1}}

```http
GET /{{RESOURCE_ENDPOINT_1}}/:id
Authorization: Bearer {{TOKEN}}
```

**Response:**

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "field1": "value1",
    "field2": "value2",
    "createdAt": "2024-01-01T00:00:00Z",
    "updatedAt": "2024-01-01T00:00:00Z"
  }
}
```

#### Create {{RESOURCE_NAME_1}}

```http
POST /{{RESOURCE_ENDPOINT_1}}
Authorization: Bearer {{TOKEN}}
Content-Type: application/json

{
  "field1": "value1",
  "field2": "value2"
}
```

**Request Body:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `field1` | string | yes | {{FIELD1_DESCRIPTION}} |
| `field2` | number | no | {{FIELD2_DESCRIPTION}} |

**Response (201 Created):**

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "field1": "value1",
    "field2": "value2",
    "createdAt": "2024-01-01T00:00:00Z"
  }
}
```

#### Update {{RESOURCE_NAME_1}}

```http
PUT /{{RESOURCE_ENDPOINT_1}}/:id
Authorization: Bearer {{TOKEN}}
Content-Type: application/json

{
  "field1": "newValue1"
}
```

**Response (200 OK):**

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "field1": "newValue1",
    "field2": "value2",
    "updatedAt": "2024-01-01T01:00:00Z"
  }
}
```

#### Delete {{RESOURCE_NAME_1}}

```http
DELETE /{{RESOURCE_ENDPOINT_1}}/:id
Authorization: Bearer {{TOKEN}}
```

**Response (204 No Content):**

```
(empty body)
```

### {{RESOURCE_NAME_2}}

{{REPEAT_PATTERN_FOR_OTHER_RESOURCES}}

## Error Codes

### Common Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `VALIDATION_ERROR` | 400 | Invalid request data |
| `UNAUTHORIZED` | 401 | Authentication required |
| `FORBIDDEN` | 403 | Insufficient permissions |
| `NOT_FOUND` | 404 | Resource not found |
| `CONFLICT` | 409 | Resource conflict |
| `RATE_LIMIT_EXCEEDED` | 429 | Too many requests |
| `INTERNAL_ERROR` | 500 | Server error |

### Validation Errors (400)

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      },
      {
        "field": "password",
        "message": "Password must be at least 8 characters"
      }
    ]
  }
}
```

## Webhooks (if applicable)

### Webhook Events

| Event | Description |
|-------|-------------|
| `{{EVENT_1}}` | {{EVENT_1_DESCRIPTION}} |
| `{{EVENT_2}}` | {{EVENT_2_DESCRIPTION}} |

### Webhook Payload

```json
{
  "event": "user.created",
  "timestamp": "2024-01-01T00:00:00Z",
  "data": {
    "id": "uuid",
    "email": "user@example.com"
  },
  "signature": "webhook_signature"
}
```

### Webhook Security

Verify webhook signature:

```{{LANGUAGE}}
{{WEBHOOK_VERIFICATION_EXAMPLE}}
```

## SDKs & Client Libraries

### Official SDKs

| Language | Package | Documentation |
|-----------|---------|----------------|
| {{LANG_1}} | `{{PACKAGE_1}}` | {{DOCS_1}} |
| {{LANG_2}} | `{{PACKAGE_2}}` | {{DOCS_2}} |

### Installation

```bash
# {{LANG_1}}
{{INSTALL_COMMAND_1}}

# {{LANG_2}}
{{INSTALL_COMMAND_2}}
```

### Example Usage

```{{LANGUAGE}}
{{SDK_USAGE_EXAMPLE}}
```

## Testing

### Test Environment

**Base URL:** `{{TEST_BASE_URL}}`

**Test Credentials:**
- Email: `test@example.com`
- API Key: `test_api_key_12345`

### Postman Collection

Import our Postman collection: [Link to collection]({{POSTMAN_COLLECTION_LINK}})

### OpenAPI/Swagger

Interactive API documentation: [Swagger UI]({{SWAGGER_URL}})

OpenAPI specification: [OpenAPI JSON]({{OPENAPI_URL}})

## Versioning

### API Version Strategy

We use URL versioning: `/api/v1/resource`

**Current Version:** `v{{API_VERSION}}`

**Deprecation Policy:**
- Old versions supported for 6 months after new version release
- Deprecation notices in response headers
- Breaking changes require major version bump

### Version Header

```http
Accept: application/vnd.api.v1+json
```

## Best Practices

### Client Implementation

1. **Retry Logic**
   - Implement exponential backoff
   - Retry on 5xx errors
   - Don't retry on 4xx errors (except 429)

2. **Error Handling**
   - Always check `success` field
   - Handle all error codes
   - Log `requestId` for debugging

3. **Performance**
   - Use compression (gzip)
   - Implement caching
   - Batch requests when possible

4. **Security**
   - Never expose API keys in client-side code
   - Use HTTPS for all requests
   - Validate server certificates

## Changelog

### v{{API_VERSION}} ({{RELEASE_DATE}})

**Added:**
- {{NEW_FEATURE_1}}
- {{NEW_FEATURE_2}}

**Changed:**
- {{CHANGED_FEATURE_1}}

**Deprecated:**
- {{DEPRECATED_FEATURE_1}}

**Removed:**
- {{REMOVED_FEATURE_1}}

**Fixed:**
- {{BUG_FIX_1}}

## Support

### Documentation Issues

Report documentation issues via [GitHub Issues]({{ISSUES_LINK}})

### API Support

- Email: {{SUPPORT_EMAIL}}
- Discord: {{DISCORD_LINK}}
- Office Hours: {{OFFICE_HOURS}}

## Related Documentation

- [Architecture](./ARCHITECTURE.md) - System architecture overview
- [Database](./DATABASE.md) - Database schema and migrations
- [Deployment](./DEPLOYMENT.md) - Deployment guide