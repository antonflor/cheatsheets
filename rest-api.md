# REST API Cheat Sheet

> **Applies to:** HTTP-based JSON APIs and general REST conventions
> **Last reviewed:** 2026-07-14

REST is an architectural style, not a wire protocol. APIs vary, so treat these as interoperable defaults rather than universal rules.

## HTTP methods

| Method | Typical use | Safe | Idempotent |
|---|---|---:|---:|
| `GET` | Retrieve a representation | Yes | Yes |
| `HEAD` | Retrieve headers only | Yes | Yes |
| `POST` | Create or trigger processing | No | No |
| `PUT` | Replace a resource at a known URI | No | Yes |
| `PATCH` | Partially update a resource | No | Depends on patch semantics |
| `DELETE` | Remove a resource | No | Yes |
| `OPTIONS` | Discover communication options | Yes | Yes |

**Safe** means the method is intended not to change server state. **Idempotent** means repeating the same request has the same intended effect as sending it once.

## Common status codes

| Code | Meaning | Typical use |
|---:|---|---|
| `200` | OK | Successful read or update |
| `201` | Created | Resource created; return a `Location` header when practical |
| `202` | Accepted | Asynchronous work accepted but not finished |
| `204` | No Content | Successful request with no response body |
| `304` | Not Modified | Conditional request cache hit |
| `400` | Bad Request | Malformed syntax or invalid request structure |
| `401` | Unauthorized | Authentication is missing or invalid |
| `403` | Forbidden | Identity is known but lacks permission |
| `404` | Not Found | Resource does not exist or is intentionally concealed |
| `409` | Conflict | State conflict, duplicate, or failed concurrency condition |
| `412` | Precondition Failed | `If-Match` or another precondition failed |
| `415` | Unsupported Media Type | Unsupported request content type |
| `422` | Unprocessable Content | Semantically invalid request |
| `429` | Too Many Requests | Rate limit exceeded |
| `500` | Internal Server Error | Unexpected server failure |
| `502` | Bad Gateway | Invalid upstream response |
| `503` | Service Unavailable | Temporarily unavailable or overloaded |
| `504` | Gateway Timeout | Upstream timed out |

## Request example

```http
POST /v1/users HTTP/1.1
Host: api.example.com
Authorization: Bearer <token>
Content-Type: application/json
Accept: application/json
Idempotency-Key: <unique-request-id>

{
  "name": "Jane Doe",
  "email": "jane@example.com"
}
```

## Response example

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /v1/users/123
ETag: "7b9f6a"

{
  "id": "123",
  "name": "Jane Doe",
  "email": "jane@example.com"
}
```

## curl patterns

```bash
curl --fail-with-body --silent --show-error \
  --request GET \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer <token>' \
  'https://api.example.com/v1/users?limit=50'
```

```bash
curl --fail-with-body --silent --show-error \
  --request POST \
  --header 'Authorization: Bearer <token>' \
  --header 'Content-Type: application/json' \
  --data @request.json \
  https://api.example.com/v1/users
```

Capture headers and body separately when troubleshooting:

```bash
curl --dump-header response.headers \
  --output response.json \
  --write-out '%{http_code}\n' \
  https://api.example.com/health
```

## Resource and URL design

- Use stable nouns: `/users/123`, not `/getUser?id=123`.
- Keep identifiers opaque to clients.
- Use query parameters for filtering, sorting, pagination, and optional projections.
- Avoid deeply nested resource paths.
- Do not encode secrets or sensitive data in URLs; URLs are commonly logged.
- Return absolute or well-defined relative links when clients need navigation.

Example:

```text
GET /v1/incidents?status=open&severity=critical&sort=-created_at&limit=50
```

## Pagination

Cursor pagination is generally more stable than offsets for changing datasets:

```json
{
  "items": [],
  "next_cursor": "opaque-value"
}
```

Do not promise a meaningful total count unless the service can provide it efficiently and consistently.

## Concurrency and retries

Use ETags and conditional requests to prevent lost updates:

```http
GET /v1/config/123
ETag: "version-7"
```

```http
PUT /v1/config/123
If-Match: "version-7"
```

Retry only when the operation is safe or idempotent, or when the API supports an idempotency key. Use bounded exponential backoff with jitter. Honor `Retry-After` on `429` and `503` responses.

## Authentication and authorization

- Require HTTPS.
- Keep authentication separate from authorization.
- Validate token issuer, audience, signature, expiration, and intended scopes.
- Prefer short-lived credentials.
- Never log bearer tokens, session cookies, API keys, passwords, or full sensitive payloads.
- Apply authorization at the resource and action level, not only at the route level.

## Error format

Use one consistent machine-readable structure. RFC 9457 Problem Details is a strong default:

```json
{
  "type": "https://api.example.com/problems/invalid-parameter",
  "title": "Invalid parameter",
  "status": 422,
  "detail": "limit must be between 1 and 100",
  "instance": "/v1/users?limit=1000"
}
```

## Observability

Include or propagate:

- A request or correlation ID.
- Trace context.
- Structured logs with secret redaction.
- Latency, throughput, saturation, and error metrics.
- Upstream dependency timing.

## Versioning and compatibility

Prefer additive changes where possible. Removing fields, changing field meaning, tightening validation, or altering enum behavior can break clients even when the URL version is unchanged. Publish a deprecation timeline and instrument usage before removal.

## References

- [HTTP Semantics — RFC 9110](https://www.rfc-editor.org/rfc/rfc9110)
- [HTTP Caching — RFC 9111](https://www.rfc-editor.org/rfc/rfc9111)
- [Problem Details for HTTP APIs — RFC 9457](https://www.rfc-editor.org/rfc/rfc9457)
- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
