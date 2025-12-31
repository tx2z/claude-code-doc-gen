---
description: "[Internal] API documentation template - use /doc-gen instead"
disable-model-invocation: true
---

# API Documentation Template

Template for generating API documentation. Variables use mustache-style syntax: `{{VARIABLE_NAME}}`.

---

## Template

```markdown
# {{API_NAME}} API Documentation

{{API_DESCRIPTION}}

## Base URL

| Environment | URL |
|-------------|-----|
{{#ENVIRONMENTS}}
| {{ENV_NAME}} | `{{ENV_URL}}` |
{{/ENVIRONMENTS}}

## Authentication

{{#AUTH_BEARER}}
### Bearer Token Authentication

This API uses Bearer token authentication. Include the token in the `Authorization` header:

```
Authorization: Bearer YOUR_ACCESS_TOKEN
```

#### Obtaining a Token

```bash
curl -X POST '{{BASE_URL}}/auth/login' \
  -H 'Content-Type: application/json' \
  -d '{
    "email": "user@example.com",
    "password": "your-password"
  }'
```

**Response:**

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIs...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
  "expiresIn": 900
}
```

#### Token Refresh

```bash
curl -X POST '{{BASE_URL}}/auth/refresh' \
  -H 'Content-Type: application/json' \
  -d '{
    "refreshToken": "YOUR_REFRESH_TOKEN"
  }'
```
{{/AUTH_BEARER}}

{{#AUTH_API_KEY}}
### API Key Authentication

Include your API key in the `X-API-Key` header:

```
X-API-Key: YOUR_API_KEY
```

API keys can be generated in the [Developer Settings]({{API_KEY_URL}}).
{{/AUTH_API_KEY}}

{{#HAS_RATE_LIMITING}}
## Rate Limiting

| Tier | Requests/Minute | Daily Limit |
|------|-----------------|-------------|
{{#RATE_LIMITS}}
| {{TIER_NAME}} | {{RPM}} | {{DAILY}} |
{{/RATE_LIMITS}}

### Rate Limit Headers

| Header | Description |
|--------|-------------|
| `X-RateLimit-Limit` | Maximum requests per window |
| `X-RateLimit-Remaining` | Remaining requests |
| `X-RateLimit-Reset` | Window reset timestamp (Unix) |

### Rate Limit Exceeded

When you exceed the rate limit, you'll receive a `429 Too Many Requests` response:

```json
{
  "statusCode": 429,
  "message": "Rate limit exceeded",
  "retryAfter": 60
}
```
{{/HAS_RATE_LIMITING}}

## Endpoints

{{#ENDPOINT_GROUPS}}
### {{GROUP_NAME}}

{{GROUP_DESCRIPTION}}

{{#ENDPOINTS}}
---

#### {{METHOD}} {{PATH}}

{{ENDPOINT_DESCRIPTION}}

{{#REQUIRES_AUTH}}
**Authentication:** Required
{{/REQUIRES_AUTH}}

{{#HAS_PARAMETERS}}
**Parameters:**

| Name | Location | Type | Required | Description |
|------|----------|------|----------|-------------|
{{#PARAMETERS}}
| `{{PARAM_NAME}}` | {{PARAM_LOCATION}} | `{{PARAM_TYPE}}` | {{PARAM_REQUIRED}} | {{PARAM_DESCRIPTION}} |
{{/PARAMETERS}}
{{/HAS_PARAMETERS}}

{{#HAS_REQUEST_BODY}}
**Request Body:**

```json
{{REQUEST_BODY_EXAMPLE}}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
{{#REQUEST_FIELDS}}
| `{{FIELD_NAME}}` | `{{FIELD_TYPE}}` | {{FIELD_REQUIRED}} | {{FIELD_DESCRIPTION}} |
{{/REQUEST_FIELDS}}
{{/HAS_REQUEST_BODY}}

**Responses:**

| Status | Description |
|--------|-------------|
{{#RESPONSES}}
| {{STATUS_CODE}} | {{STATUS_DESCRIPTION}} |
{{/RESPONSES}}

**Success Response ({{SUCCESS_STATUS}}):**

```json
{{SUCCESS_RESPONSE_EXAMPLE}}
```

{{#HAS_ERROR_EXAMPLE}}
**Error Response ({{ERROR_STATUS}}):**

```json
{{ERROR_RESPONSE_EXAMPLE}}
```
{{/HAS_ERROR_EXAMPLE}}

**Example Requests:**

<details>
<summary>cURL</summary>

```bash
{{CURL_EXAMPLE}}
```
</details>

<details>
<summary>JavaScript (fetch)</summary>

```javascript
{{FETCH_EXAMPLE}}
```
</details>

<details>
<summary>Python (requests)</summary>

```python
{{PYTHON_EXAMPLE}}
```
</details>

{{/ENDPOINTS}}
{{/ENDPOINT_GROUPS}}

## Error Codes

### HTTP Status Codes

| Status | Name | Description |
|--------|------|-------------|
| 200 | OK | Request successful |
| 201 | Created | Resource created successfully |
| 204 | No Content | Request successful, no response body |
| 400 | Bad Request | Invalid request body or parameters |
| 401 | Unauthorized | Authentication required or invalid |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource not found |
| 409 | Conflict | Resource already exists |
| 422 | Unprocessable Entity | Validation error |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server error |

{{#HAS_APP_ERROR_CODES}}
### Application Error Codes

| Code | Name | Description |
|------|------|-------------|
{{#APP_ERROR_CODES}}
| `{{ERROR_CODE}}` | {{ERROR_NAME}} | {{ERROR_DESCRIPTION}} |
{{/APP_ERROR_CODES}}
{{/HAS_APP_ERROR_CODES}}

## Types

{{#TYPES}}
### {{TYPE_NAME}}

{{TYPE_DESCRIPTION}}

```{{CODE_LANGUAGE}}
{{TYPE_DEFINITION}}
```

| Field | Type | Description |
|-------|------|-------------|
{{#TYPE_FIELDS}}
| `{{FIELD_NAME}}` | `{{FIELD_TYPE}}` | {{FIELD_DESCRIPTION}} |
{{/TYPE_FIELDS}}

{{/TYPES}}

## Pagination

{{#HAS_PAGINATION}}
Endpoints that return lists support pagination using the following query parameters:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `page` | integer | 1 | Page number (1-indexed) |
| `limit` | integer | 10 | Items per page (max: 100) |

**Paginated Response Format:**

```json
{
  "data": [...],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 100,
    "totalPages": 10
  }
}
```
{{/HAS_PAGINATION}}

## Changelog

{{#CHANGELOG_ENTRIES}}
### {{VERSION}} - {{DATE}}

{{#CHANGES}}
- {{CHANGE_TYPE}}: {{CHANGE_DESCRIPTION}}
{{/CHANGES}}
{{/CHANGELOG_ENTRIES}}
```

---

## Variable Reference

### Required Variables

| Variable | Type | Description |
|----------|------|-------------|
| `API_NAME` | string | Name of the API |
| `API_DESCRIPTION` | string | Brief description |
| `BASE_URL` | string | Base URL for API calls |

### Authentication Variables

| Variable | Type | Description |
|----------|------|-------------|
| `AUTH_BEARER` | boolean | Uses Bearer token auth |
| `AUTH_API_KEY` | boolean | Uses API key auth |
| `API_KEY_URL` | string | URL to get API keys |

### Endpoint Variables

| Variable | Type | Description |
|----------|------|-------------|
| `METHOD` | string | HTTP method (GET, POST, etc.) |
| `PATH` | string | Endpoint path |
| `ENDPOINT_DESCRIPTION` | string | What the endpoint does |
| `REQUIRES_AUTH` | boolean | Needs authentication |

### Request/Response Variables

| Variable | Type | Description |
|----------|------|-------------|
| `REQUEST_BODY_EXAMPLE` | string | JSON request body example |
| `SUCCESS_RESPONSE_EXAMPLE` | string | JSON success response |
| `ERROR_RESPONSE_EXAMPLE` | string | JSON error response |

---

## Example Request Templates

### cURL Template
```bash
curl -X {{METHOD}} '{{BASE_URL}}{{PATH}}' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_TOKEN' \
{{#HAS_BODY}}
  -d '{{REQUEST_BODY}}'
{{/HAS_BODY}}
```

### JavaScript (fetch) Template
```javascript
const response = await fetch('{{BASE_URL}}{{PATH}}', {
  method: '{{METHOD}}',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_TOKEN',
  },
{{#HAS_BODY}}
  body: JSON.stringify({{REQUEST_BODY_JS}}),
{{/HAS_BODY}}
});

const data = await response.json();
```

### Python (requests) Template
```python
import requests

response = requests.{{METHOD_LOWER}}(
    '{{BASE_URL}}{{PATH}}',
    headers={'Authorization': 'Bearer YOUR_TOKEN'},
{{#HAS_BODY}}
    json={{REQUEST_BODY_PYTHON}},
{{/HAS_BODY}}
)
data = response.json()
```

---

## OpenAPI Spec Template

```yaml
openapi: 3.0.3
info:
  title: {{API_NAME}}
  description: {{API_DESCRIPTION}}
  version: {{API_VERSION}}
  contact:
    name: {{CONTACT_NAME}}
    email: {{CONTACT_EMAIL}}
  license:
    name: {{LICENSE_NAME}}
    url: {{LICENSE_URL}}

servers:
{{#ENVIRONMENTS}}
  - url: {{ENV_URL}}
    description: {{ENV_NAME}}
{{/ENVIRONMENTS}}

paths:
{{#ENDPOINT_GROUPS}}
{{#ENDPOINTS}}
  {{PATH}}:
    {{METHOD_LOWER}}:
      summary: {{ENDPOINT_SUMMARY}}
      description: {{ENDPOINT_DESCRIPTION}}
      operationId: {{OPERATION_ID}}
      tags:
        - {{GROUP_NAME}}
{{#HAS_PARAMETERS}}
      parameters:
{{#PARAMETERS}}
        - name: {{PARAM_NAME}}
          in: {{PARAM_LOCATION}}
          required: {{PARAM_REQUIRED}}
          schema:
            type: {{PARAM_TYPE}}
          description: {{PARAM_DESCRIPTION}}
{{/PARAMETERS}}
{{/HAS_PARAMETERS}}
{{#HAS_REQUEST_BODY}}
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/{{REQUEST_SCHEMA}}'
{{/HAS_REQUEST_BODY}}
      responses:
{{#RESPONSES}}
        '{{STATUS_CODE}}':
          description: {{STATUS_DESCRIPTION}}
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/{{RESPONSE_SCHEMA}}'
{{/RESPONSES}}
{{/ENDPOINTS}}
{{/ENDPOINT_GROUPS}}

components:
  schemas:
{{#TYPES}}
    {{TYPE_NAME}}:
      type: object
      properties:
{{#TYPE_FIELDS}}
        {{FIELD_NAME}}:
          type: {{OPENAPI_TYPE}}
          description: {{FIELD_DESCRIPTION}}
{{/TYPE_FIELDS}}
      required:
{{#REQUIRED_FIELDS}}
        - {{FIELD_NAME}}
{{/REQUIRED_FIELDS}}
{{/TYPES}}

  securitySchemes:
{{#AUTH_BEARER}}
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
{{/AUTH_BEARER}}
{{#AUTH_API_KEY}}
    apiKeyAuth:
      type: apiKey
      in: header
      name: X-API-Key
{{/AUTH_API_KEY}}

security:
{{#AUTH_BEARER}}
  - bearerAuth: []
{{/AUTH_BEARER}}
{{#AUTH_API_KEY}}
  - apiKeyAuth: []
{{/AUTH_API_KEY}}
```
