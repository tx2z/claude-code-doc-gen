---
description: "[Internal] API documentation agent - use /doc-gen instead"
disable-model-invocation: true
allowed-tools: Read, Glob, Grep, WebSearch
---

# API Documentation Agent (DOC02)

Generate comprehensive API documentation for REST APIs, GraphQL APIs, and library/package APIs.

---

## 1. API Discovery

### 1.1 Identify API Type

**Search for API indicators:**

**REST API:**
```
Grep: @Get|@Post|@Put|@Patch|@Delete|app\.get|app\.post|router\.get|@api_view|@app\.route
Glob: **/*.controller.ts, **/routes/*.ts, **/routes/*.js, **/views.py, **/routes.py
```

**GraphQL API:**
```
Grep: @Query|@Mutation|@Resolver|typeDefs|gql`|GraphQLSchema
Glob: **/*.resolver.ts, **/schema.graphql, **/schema.gql
```

**Library API:**
```
Grep: export (function|class|const|interface|type)
Glob: src/index.ts, lib/index.ts, src/main.ts
```

### 1.2 Extract Endpoint Information

**For REST APIs, gather:**
- [ ] HTTP method (GET, POST, PUT, PATCH, DELETE)
- [ ] Route path (with parameters)
- [ ] Controller/handler name
- [ ] Description (from decorators or comments)
- [ ] Request body schema
- [ ] Response schema
- [ ] Query parameters
- [ ] Path parameters
- [ ] Headers required
- [ ] Authentication requirements

**NestJS Pattern:**
```
Grep: @Controller|@Get|@Post|@Put|@Patch|@Delete|@Body|@Param|@Query
```

**Express Pattern:**
```
Grep: router\.(get|post|put|patch|delete)|app\.(get|post|put|patch|delete)
```

**FastAPI Pattern:**
```
Grep: @app\.(get|post|put|patch|delete)|@router\.(get|post|put|patch|delete)
```

**Django REST Framework:**
```
Grep: @api_view|class.*APIView|ViewSet
```

### 1.3 Extract Schemas/DTOs

**Search for request/response schemas:**
```
Glob: **/dto/*.ts, **/schemas/*.py, **/models/*.ts, **/types/*.ts
Grep: class.*Dto|interface.*Request|interface.*Response|BaseModel|@dataclass
```

**Document for each schema:**
- [ ] Field name
- [ ] Field type
- [ ] Required/optional
- [ ] Validation rules
- [ ] Description
- [ ] Example value

---

## 2. REST API Documentation

### 2.1 Endpoint Listing Format

**Standard format per endpoint:**
```markdown
### {{METHOD}} {{PATH}}

{{DESCRIPTION}}

**Authentication:** {{AUTH_REQUIREMENT}}

**Parameters:**

| Name | Location | Type | Required | Description |
|------|----------|------|----------|-------------|
| `id` | path | string | Yes | Resource ID |
| `limit` | query | integer | No | Number of items (default: 10) |

**Request Body:**

```json
{
  "field1": "string",
  "field2": 123,
  "nested": {
    "subField": true
  }
}
```

**Response:**

| Status | Description |
|--------|-------------|
| 200 | Success |
| 400 | Bad Request |
| 401 | Unauthorized |
| 404 | Not Found |

**Success Response (200):**

```json
{
  "id": "abc123",
  "field1": "value",
  "createdAt": "2025-01-01T00:00:00Z"
}
```

**Error Response (400):**

```json
{
  "statusCode": 400,
  "message": "Validation failed",
  "errors": [
    {
      "field": "field1",
      "message": "field1 must be a string"
    }
  ]
}
```
```

### 2.2 Example Requests

**Generate examples for multiple clients:**

**cURL:**
```bash
curl -X POST 'https://api.example.com/v1/users' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_TOKEN' \
  -d '{
    "email": "user@example.com",
    "name": "John Doe"
  }'
```

**JavaScript (fetch):**
```javascript
const response = await fetch('https://api.example.com/v1/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_TOKEN',
  },
  body: JSON.stringify({
    email: 'user@example.com',
    name: 'John Doe',
  }),
});

const data = await response.json();
```

**JavaScript (axios):**
```javascript
const { data } = await axios.post('https://api.example.com/v1/users', {
  email: 'user@example.com',
  name: 'John Doe',
}, {
  headers: {
    'Authorization': 'Bearer YOUR_TOKEN',
  },
});
```

**Python (requests):**
```python
import requests

response = requests.post(
    'https://api.example.com/v1/users',
    headers={'Authorization': 'Bearer YOUR_TOKEN'},
    json={
        'email': 'user@example.com',
        'name': 'John Doe',
    }
)
data = response.json()
```

### 2.3 Authentication Documentation

**Identify authentication method:**
```
Grep: @UseGuards|passport|jwt|Bearer|API-Key|OAuth|session
Glob: **/auth/*.ts, **/middleware/*.ts
```

**Document authentication:**
```markdown
## Authentication

This API uses Bearer token authentication.

### Obtaining a Token

```bash
curl -X POST 'https://api.example.com/v1/auth/login' \
  -H 'Content-Type: application/json' \
  -d '{
    "email": "user@example.com",
    "password": "your-password"
  }'
```

### Using the Token

Include the token in the Authorization header:

```
Authorization: Bearer YOUR_ACCESS_TOKEN
```

### Token Refresh

Access tokens expire after 1 hour. Use the refresh token to get a new access token:

```bash
curl -X POST 'https://api.example.com/v1/auth/refresh' \
  -H 'Content-Type: application/json' \
  -d '{
    "refreshToken": "YOUR_REFRESH_TOKEN"
  }'
```
```

### 2.4 Rate Limiting Documentation

**Identify rate limiting:**
```
Grep: @Throttle|RateLimit|rate-limit|throttle
Glob: **/*.guard.ts, **/middleware/*.ts
```

**Document rate limits:**
```markdown
## Rate Limiting

This API implements rate limiting to ensure fair usage.

| Tier | Requests per Minute | Daily Limit |
|------|---------------------|-------------|
| Free | 60 | 1,000 |
| Pro | 600 | 50,000 |
| Enterprise | Unlimited | Unlimited |

### Rate Limit Headers

Responses include rate limit information:

| Header | Description |
|--------|-------------|
| `X-RateLimit-Limit` | Maximum requests per window |
| `X-RateLimit-Remaining` | Remaining requests in window |
| `X-RateLimit-Reset` | Unix timestamp when window resets |

### Exceeding Rate Limits

When rate limited, you'll receive a 429 response:

```json
{
  "statusCode": 429,
  "message": "Too many requests",
  "retryAfter": 60
}
```
```

### 2.5 Error Code Documentation

**Standard error code table:**
```markdown
## Error Codes

### HTTP Status Codes

| Status | Name | Description |
|--------|------|-------------|
| 200 | OK | Request successful |
| 201 | Created | Resource created |
| 204 | No Content | Request successful, no response body |
| 400 | Bad Request | Invalid request body or parameters |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource not found |
| 409 | Conflict | Resource already exists |
| 422 | Unprocessable Entity | Validation error |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server error |

### Application Error Codes

| Code | Name | Description | Resolution |
|------|------|-------------|------------|
| `USER_NOT_FOUND` | User Not Found | The specified user does not exist | Verify user ID |
| `INVALID_TOKEN` | Invalid Token | The auth token is invalid or expired | Refresh token |
| `VALIDATION_ERROR` | Validation Error | Request body validation failed | Check error details |
```

---

## 3. GraphQL API Documentation

### 3.1 Schema Discovery

**Search for GraphQL types:**
```
Grep: type Query|type Mutation|type Subscription|input |enum |interface
Glob: **/*.graphql, **/*.gql, **/schema.ts
```

### 3.2 Query Documentation

```markdown
## Queries

### users

Get a list of users with optional filtering.

```graphql
query GetUsers($limit: Int, $offset: Int, $filter: UserFilter) {
  users(limit: $limit, offset: $offset, filter: $filter) {
    id
    email
    name
    createdAt
  }
}
```

**Arguments:**

| Argument | Type | Required | Description |
|----------|------|----------|-------------|
| `limit` | Int | No | Number of items (default: 10) |
| `offset` | Int | No | Pagination offset |
| `filter` | UserFilter | No | Filter criteria |

**Returns:** `[User!]!`
```

### 3.3 Mutation Documentation

```markdown
## Mutations

### createUser

Create a new user account.

```graphql
mutation CreateUser($input: CreateUserInput!) {
  createUser(input: $input) {
    id
    email
    name
  }
}
```

**Input:**

```graphql
input CreateUserInput {
  email: String!
  name: String!
  password: String!
}
```

**Returns:** `User!`
```

---

## 4. Library API Documentation

### 4.1 Function Documentation

**Search for exported functions:**
```
Grep: export (async )?function|export const .* = (\(|async)|module\.exports
Glob: src/index.ts, lib/*.ts, src/**/*.ts
```

**Document each function:**
```markdown
### functionName

{{DESCRIPTION}}

**Signature:**

```typescript
function functionName(param1: string, param2?: Options): Promise<Result>
```

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `param1` | `string` | Yes | Description of param1 |
| `param2` | `Options` | No | Configuration options |

**Options:**

```typescript
interface Options {
  timeout?: number;    // Request timeout in ms (default: 5000)
  retries?: number;    // Number of retries (default: 3)
  debug?: boolean;     // Enable debug logging (default: false)
}
```

**Returns:**

```typescript
interface Result {
  success: boolean;
  data: OutputData;
  metadata: Metadata;
}
```

**Example:**

```typescript
import { functionName } from 'package-name';

const result = await functionName('input', {
  timeout: 10000,
  retries: 5,
});

console.log(result.data);
```

**Throws:**

| Error | Description |
|-------|-------------|
| `ValidationError` | When param1 is invalid |
| `TimeoutError` | When request times out |
```

### 4.2 Class Documentation

```markdown
### ClassName

{{CLASS_DESCRIPTION}}

**Constructor:**

```typescript
new ClassName(config: Config)
```

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `isConnected` | `boolean` | Connection status |
| `version` | `string` | Client version |

**Methods:**

#### connect()

Establish connection to the service.

```typescript
async connect(): Promise<void>
```

#### disconnect()

Close the connection.

```typescript
disconnect(): void
```

#### send(message)

Send a message.

```typescript
async send(message: Message): Promise<Response>
```

**Example:**

```typescript
import { ClassName } from 'package-name';

const client = new ClassName({
  host: 'localhost',
  port: 8080,
});

await client.connect();
const response = await client.send({ type: 'ping' });
client.disconnect();
```
```

### 4.3 Interface/Type Documentation

```markdown
### Types

#### User

```typescript
interface User {
  id: string;
  email: string;
  name: string;
  role: UserRole;
  createdAt: Date;
  updatedAt: Date;
}
```

#### UserRole

```typescript
type UserRole = 'admin' | 'user' | 'guest';
```

#### CreateUserInput

```typescript
interface CreateUserInput {
  email: string;      // Valid email address
  name: string;       // Full name (2-100 characters)
  password: string;   // Min 8 characters
  role?: UserRole;    // Default: 'user'
}
```
```

---

## 5. OpenAPI/Swagger Generation

### 5.1 OpenAPI 3.0 Spec Structure

```yaml
openapi: 3.0.3
info:
  title: {{API_NAME}}
  description: {{API_DESCRIPTION}}
  version: {{VERSION}}
  contact:
    name: {{CONTACT_NAME}}
    email: {{CONTACT_EMAIL}}
  license:
    name: MIT
    url: https://opensource.org/licenses/MIT

servers:
  - url: https://api.example.com/v1
    description: Production
  - url: https://staging-api.example.com/v1
    description: Staging
  - url: http://localhost:3000/v1
    description: Local development

paths:
  /users:
    get:
      summary: List users
      operationId: listUsers
      tags:
        - Users
      parameters:
        - name: limit
          in: query
          schema:
            type: integer
            default: 10
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
    post:
      summary: Create user
      operationId: createUser
      tags:
        - Users
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserInput'
      responses:
        '201':
          description: Created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
          format: uuid
        email:
          type: string
          format: email
        name:
          type: string
      required:
        - id
        - email
        - name
    CreateUserInput:
      type: object
      properties:
        email:
          type: string
          format: email
        name:
          type: string
        password:
          type: string
          minLength: 8
      required:
        - email
        - name
        - password
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - bearerAuth: []
```

### 5.2 Postman Collection Generation

```json
{
  "info": {
    "name": "{{API_NAME}}",
    "description": "{{API_DESCRIPTION}}",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "auth": {
    "type": "bearer",
    "bearer": [
      {
        "key": "token",
        "value": "{{access_token}}",
        "type": "string"
      }
    ]
  },
  "variable": [
    {
      "key": "base_url",
      "value": "https://api.example.com/v1"
    },
    {
      "key": "access_token",
      "value": ""
    }
  ],
  "item": [
    {
      "name": "Users",
      "item": [
        {
          "name": "List Users",
          "request": {
            "method": "GET",
            "url": {
              "raw": "{{base_url}}/users?limit=10",
              "host": ["{{base_url}}"],
              "path": ["users"],
              "query": [
                {
                  "key": "limit",
                  "value": "10"
                }
              ]
            }
          }
        },
        {
          "name": "Create User",
          "request": {
            "method": "POST",
            "url": {
              "raw": "{{base_url}}/users",
              "host": ["{{base_url}}"],
              "path": ["users"]
            },
            "body": {
              "mode": "raw",
              "raw": "{\n  \"email\": \"user@example.com\",\n  \"name\": \"John Doe\",\n  \"password\": \"securepassword123\"\n}",
              "options": {
                "raw": {
                  "language": "json"
                }
              }
            }
          }
        }
      ]
    }
  ]
}
```

---

## 6. Language-Specific Patterns

### 6.1 NestJS

**Controller pattern:**
```
Grep: @Controller\(['"]([^'"]+)['"]\)|@Get\(|@Post\(|@Put\(|@Patch\(|@Delete\(
```

**DTO pattern:**
```
Grep: class.*Dto|@IsString|@IsNumber|@IsEmail|@IsOptional
Glob: **/dto/*.ts
```

### 6.2 Express.js

**Route pattern:**
```
Grep: router\.(get|post|put|patch|delete)\(['"]
```

### 6.3 FastAPI (Python)

**Endpoint pattern:**
```
Grep: @(app|router)\.(get|post|put|patch|delete)\(
```

**Pydantic model pattern:**
```
Grep: class.*\(BaseModel\)
```

### 6.4 Django REST Framework

**ViewSet pattern:**
```
Grep: class.*ViewSet|class.*APIView|@api_view
```

**Serializer pattern:**
```
Grep: class.*Serializer
```

### 6.5 Go (Gin/Echo)

**Handler pattern:**
```
Grep: \.(GET|POST|PUT|PATCH|DELETE)\(|func.*Handler
Glob: **/*.go
```

### 6.6 ASP.NET Core

**Controller pattern:**
```
Grep: \[HttpGet\]|\[HttpPost\]|\[Route\(|class.*Controller
Glob: **/*.cs
```

---

## 7. Output Format

Generate API documentation following this structure:

```markdown
# {{API_NAME}} API Documentation

{{DESCRIPTION}}

## Base URL

```
Production: https://api.example.com/v1
Staging: https://staging-api.example.com/v1
```

## Authentication

{{AUTHENTICATION_DOCS}}

## Rate Limiting

{{RATE_LIMITING_DOCS}}

## Endpoints

### {{RESOURCE_NAME}}

{{ENDPOINT_DOCS}}

## Error Codes

{{ERROR_CODES}}

## Types/Schemas

{{TYPE_DOCS}}

## Examples

{{EXAMPLE_REQUESTS}}
```

---

## 8. Quality Checks

Before finalizing, verify:

- [ ] All endpoints are documented
- [ ] Request/response examples are valid JSON
- [ ] Authentication is clearly explained
- [ ] Error codes are comprehensive
- [ ] Examples are copy-paste ready
- [ ] OpenAPI spec validates successfully
- [ ] Postman collection imports correctly
- [ ] No sensitive data in examples (real API keys, passwords)
