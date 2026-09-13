# TripMate API Response Standard

Status: **APPROVED - 2026-09-12**

Scope: Backend, Frontend, and Mobile API integration

# Decision

TripMate APIs return typed DTOs directly for successful responses. They do not wrap successful or
failed responses in a synthetic `{ success, statusCode, message, data, errors }` envelope.

Failures follow RFC 7807:

- request-validation failures return `ValidationProblemDetails` with field-level `errors`;
- business failures return `ProblemDetails` with the stable `errorCode` extension;
- conflict responses may add a documented typed extension such as `existingPoiId`;
- unexpected failures return generic non-sensitive `ProblemDetails` without exception, stack,
  SQL, credential, or infrastructure details;
- authentication challenges may return no body when that is the actual JwtBearer runtime behavior,
  and OpenAPI must not advertise a body the runtime does not return.

# Success Examples

A single-resource endpoint returns its typed DTO directly:

```json
{
  "id": 42,
  "name": "My Khe Beach"
}
```

A paginated endpoint returns its typed page DTO directly:

```json
{
  "page": 1,
  "pageSize": 20,
  "totalCount": 1,
  "totalPages": 1,
  "items": []
}
```

# Error Examples

Validation failure:

```json
{
  "type": "https://tools.ietf.org/html/rfc9110#section-15.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "errors": {
    "page": [
      "Page must be greater than zero."
    ]
  }
}
```

Business failure:

```json
{
  "status": 404,
  "title": "The requested resource was not found.",
  "errorCode": "Poi.NotFound"
}
```

# Contract Rules

- JSON property names use `camelCase`.
- HTTP status codes, request fields, response fields, nullability, defaults, enums, and error
  extensions must match the approved API contract and generated OpenAPI document.
- Stable `errorCode` values are integration contracts; FE and Mobile must not depend on mutable
  English message text.
- Nullable properties that belong to a DTO remain present and may contain `null`, unless a specific
  endpoint contract explicitly states otherwise.
- BE, FE, and Mobile must update the canonical API contract before introducing an incompatible
  response shape.

# Reconciliation Note

The Engineering Quality Checklist and Team Engineering Rules previously included a unified-envelope
example. D4 for TM-58 explicitly resolves that conflict in favor of the established Backend
`Result<T>` plus RFC 7807 pipeline, which is also used by TM-98. This file is the canonical TripMate
API response decision for subsequent integration work.
