# API / Interface Contract Template

API contracts define the interface surface of the system. Use OpenAPI for REST APIs, GraphQL schema for GraphQL, or structured markdown for other interface types (WebSockets, webhooks, message queues).

## For REST APIs (OpenAPI)

Store as `docs/api/service-name.yaml`. Generate from code annotations where possible, enrich manually for context.

```yaml
# docs/api/service-name.yaml
openapi: 3.1.0
info:
  title: "[Service Name] API"
  version: "1.0.0"
  description: |
    Brief description of what this API serves.

    Related documents:
    - [Architecture](../architecture.md)
    - [Requirements](../requirements.md)

servers:
  - url: https://api.example.com/v1
    description: Production
  - url: https://staging-api.example.com/v1
    description: Staging

paths:
  /resource:
    get:
      summary: "List resources"
      description: "Detailed description of behavior, pagination, filtering."
      operationId: listResources
      tags: [resources]
      parameters:
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
            maximum: 100
        - name: cursor
          in: query
          schema:
            type: string
      responses:
        '200':
          description: "Success"
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ResourceList'
        '401':
          $ref: '#/components/responses/Unauthorized'

components:
  schemas:
    Resource:
      type: object
      required: [id, name, created_at]
      properties:
        id:
          type: string
          format: uuid
        name:
          type: string
        created_at:
          type: string
          format: date-time
  responses:
    Unauthorized:
      description: "Authentication required"
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - bearerAuth: []
```

## For WebSocket / Realtime Interfaces

Use structured markdown when OpenAPI doesn't fit:

```markdown
---
title: "[Service] — Realtime Interface Contract"
status: draft
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [name]
tags: [api, realtime, websocket]
---

# Realtime Interface Contract

## Connection

- **Endpoint**: `wss://api.example.com/realtime`
- **Authentication**: Bearer token in connection params.
- **Heartbeat**: Client sends ping every 30s. Server responds with pong.

## Channels

### [Channel Name]

- **Subscribe pattern**: `resource:id`
- **Events emitted**:

| Event | Payload | When |
|-------|---------|------|
| `created` | `{ id, ... }` | New resource created |
| `updated` | `{ id, changes }` | Resource modified |
| `deleted` | `{ id }` | Resource removed |

## Error Codes

| Code | Meaning | Recovery |
|------|---------|----------|
| 4001 | Invalid token | Re-authenticate |
| 4002 | Rate limited | Back off and retry |
```

## For Webhook Contracts

```markdown
## Webhooks

### [Event Name] Webhook

- **Trigger**: When [condition].
- **Method**: POST
- **Content-Type**: application/json
- **Retry policy**: 3 retries with exponential backoff (1s, 5s, 25s).
- **Signature**: HMAC-SHA256 in `X-Signature-256` header.

**Payload**:
```json
{
  "event": "event.name",
  "timestamp": "2026-01-15T10:30:00Z",
  "data": { }
}
```

**Expected response**: 2xx within 10 seconds.
```

## Versioning Strategy

Document the API versioning approach:
- URL-based (`/v1/`, `/v2/`), header-based, or content negotiation.
- Deprecation policy: how much notice before breaking changes.
- Sunset headers and migration guides.
