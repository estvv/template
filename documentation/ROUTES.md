# API Routes

## Overview

{{ROUTES_OVERVIEW}}

## Base URL

```
{{BASE_URL}}
```

## Authentication

{{AUTHENTICATION_METHOD}}

### Authentication Headers

```http
{{AUTH_HEADER_EXAMPLE}}
Authorization: Bearer <token>
```

## Route Groups

### {{ROUTE_GROUP_1_NAME}}

#### {{ENDPOINT_1}}

**Method:** `{{HTTP_METHOD}}`
**Path:** `{{PATH}}`
**Description:** {{DESCRIPTION}}

**Request:**

```http
{{REQUEST_EXAMPLE}}
```

**Parameters:**

| Parameter | Type | Location | Required | Description |
|-----------|------|----------|----------|-------------|
| {{PARAM_1}} | {{TYPE_1}} | {{LOCATION_1}} | {{REQUIRED_1}} | {{DESC_1}} |
| {{PARAM_2}} | {{TYPE_2}} | {{LOCATION_2}} | {{REQUIRED_2}} | {{DESC_2}} |

**Request Body:**

```json
{
  "{{FIELD_1}}": "{{VALUE_1}}",
  "{{FIELD_2}}": "{{VALUE_2}}"
}
```

**Response:**

```http
{{STATUS_CODE}} {{STATUS_MESSAGE}}
```

```json
{
  "success": true,
  "data": {
    "{{RESPONSE_FIELD_1}}": "{{RESPONSE_VALUE_1}}",
    "{{RESPONSE_FIELD_2}}": "{{RESPONSE_VALUE_2}}"
  }
}
```

**Errors:**

| Status Code | Error Code | Description |
|-------------|------------|-------------|
| {{ERROR_STATUS_1}} | {{ERROR_CODE_1}} | {{ERROR_DESC_1}} |
| {{ERROR_STATUS_2}} | {{ERROR_CODE_2}} | {{ERROR_DESC_2}} |

#### {{ENDPOINT_2}}

**Method:** `{{HTTP_METHOD}}`
**Path:** `{{PATH}}`
**Description:** {{DESCRIPTION}}

**Request:**

```http
{{REQUEST_EXAMPLE}}
```

**Response:**

```json
{
  "success": true,
  "data": {}
}
```

### {{ROUTE_GROUP_2_NAME}}

#### {{ENDPOINT_3}}

**Method:** `{{HTTP_METHOD}}`
**Path:** `{{PATH}}`
**Description:** {{DESCRIPTION}}

## Request/Response Formats

### Success Response

```json
{
  "success": true,
  "data": {},
  "meta": {
    "page": 1,
    "total": 100,
    "per_page": 20
  }
}
```

### Error Response

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": {}
  }
}
```

## Pagination

### Query Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `page` | number | 1 | Page number |
| `per_page` | number | 20 | Items per page |
| `sort` | string | `created_at` | Sort field |
| `order` | string | `desc` | Sort order (asc/desc) |

### Response Format

```json
{
  "success": true,
  "data": [],
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 100,
    "total_pages": 5
  },
  "links": {
    "first": "{{BASE_URL}}/resource?page=1",
    "prev": "{{BASE_URL}}/resource?page=1",
    "next": "{{BASE_URL}}/resource?page=2",
    "last": "{{BASE_URL}}/resource?page=5"
  }
}
```

## Filtering

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `{{FILTER_1}}` | {{TYPE}} | {{DESCRIPTION}} |
| `{{FILTER_2}}` | {{TYPE}} | {{DESCRIPTION}} |

### Example

```http
GET {{BASE_URL}}/resource?{{FILTER_1}}={{VALUE_1}}&{{FILTER_2}}={{VALUE_2}}
```

## Sorting

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `sort` | string | Field to sort by |
| `order` | string | Sort direction (asc/desc) |

### Example

```http
GET {{BASE_URL}}/resource?sort=created_at&order=desc
```

## Rate Limiting

### Headers

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1633027200
```

### Rate Limit Response

When rate limit is exceeded:

```http
HTTP/1.1 429 Too Many Requests
```

```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded. Try again in 60 seconds.",
    "details": {
      "retry_after": 60
    }
  }
}
```

## Versioning

### URL Versioning

```http
GET {{BASE_URL}}/v1/resource
GET {{BASE_URL}}/v2/resource
```

### Header Versioning

```http
Accept: application/vnd.{{API_NAME}}.v1+json
```

## WebSocket Routes

### Connection

```
{{WS_URL}}?token={{AUTH_TOKEN}}
```

### Events

#### {{EVENT_1}}

**Event:** `{{EVENT_NAME}}`
**Direction:** {{DIRECTION}} (client/server)
**Description:** {{DESCRIPTION}}

**Payload:**

```json
{
  "{{FIELD_1}}": "{{VALUE_1}}",
  "{{FIELD_2}}": "{{VALUE_2}}"
}
```

## SDK Examples

### JavaScript/TypeScript

```javascript
{{SDK_EXAMPLE_JS}}
```

### Python

```python
{{SDK_EXAMPLE_PYTHON}}
```

### cURL

```bash
{{SDK_EXAMPLE_CURL}}
```

## Testing Routes

### Health Check

```http
GET {{BASE_URL}}/health
```

**Response:**

```json
{
  "status": "healthy",
  "version": "{{API_VERSION}}",
  "uptime": 12345
}
```

### API Information

```http
GET {{BASE_URL}}/info
```

**Response:**

```json
{
  "name": "{{API_NAME}}",
  "version": "{{API_VERSION}}",
  "documentation": "{{DOCS_URL}}"
}
```

## Request Examples

### Create Resource

```bash
curl -X POST {{BASE_URL}}/resource \
  -H "Authorization: Bearer {{TOKEN}}" \
  -H "Content-Type: application/json" \
  -d '{"name": "example"}'
```

### List Resources

```bash
curl -X GET "{{BASE_URL}}/resource?page=1&per_page=20" \
  -H "Authorization: Bearer {{TOKEN}}"
```

### Update Resource

```bash
curl -X PUT {{BASE_URL}}/resource/123 \
  -H "Authorization: Bearer {{TOKEN}}" \
  -H "Content-Type: application/json" \
  -d '{"name": "updated"}'
```

### Delete Resource

```bash
curl -X DELETE {{BASE_URL}}/resource/123 \
  -H "Authorization: Bearer {{TOKEN}}"
```

## Common Patterns

### Search

```http
GET {{BASE_URL}}/resource/search?q={{QUERY}}
```

### Bulk Operations

```http
POST {{BASE_URL}}/resource/bulk
```

```json
{
  "operations": [
    {"action": "create", "data": {}},
    {"action": "update", "id": 123, "data": {}},
    {"action": "delete", "id": 456}
  ]
}
```

### Export/Import

```http
GET {{BASE_URL}}/resource/export?format=csv
POST {{BASE_URL}}/resource/import
```

## Notes

{{ADDITIONAL_NOTES}}

## Related Documentation

- [API.md](./API.md) - Complete API documentation
- [ARCHITECTURE.md](./ARCHITECTURE.md) - System architecture
- [DATABASE.md](./DATABASE.md) - Database schema