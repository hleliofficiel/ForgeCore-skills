# API Design & Integration Contracts

> Distilled engineering knowledge for designing robust, secure, and developer-friendly Application Programming Interfaces.

An API is a contract between your system and the outside world. Once published, it is extremely difficult to change. Good API design focuses on predictability, consistency, and resilience.

---

# Core Principles

Every API should satisfy:

**Predictability:** The API should behave exactly as a consumer expects. Follow established standards.
**Consistency:** Use consistent naming conventions, status codes, and error formats across all endpoints.
**Statelessness:** Each request must contain all the information necessary for the server to understand and process it.
**Idempotency:** Repeating a request should yield the same result as making it once.
**Documentation:** An API without good documentation is useless. Provide OpenAPI (Swagger) specs.

---

# RESTful Design Standards

When building REST APIs:

**Nouns, Not Verbs:** Use nouns to represent resources (e.g., `/users`, not `/getUsers`).
**Pluralize Resources:** Always use plural nouns for consistency (e.g., `/users/123`, not `/user/123`).
**HTTP Methods:**

- `GET`: Retrieve a resource. Must be idempotent and safe.
- `POST`: Create a new resource. Not idempotent.
- `PUT`: Replace an existing resource entirely. Idempotent.
- `PATCH`: Partially update an existing resource. Idempotent.
- `DELETE`: Remove a resource. Idempotent.

**Nesting:** Keep URLs shallow. Avoid nesting resources more than one level deep (e.g., prefer `/comments?post_id=1` over `/posts/1/comments/2/replies`).

---

# GraphQL Principles

When building GraphQL APIs:

**Graph Mentality:** Think in graphs, not endpoints. Model your data as a connected graph of nodes and edges.
**Avoid N+1 Problems:** Use Dataloaders to batch and cache database requests.
**Pagination:** Always implement cursor-based pagination (Connections pattern) for lists.
**Mutation Design:** Input types should be specific to the mutation. Return the modified object and any affected edges.
**Complexity Analysis:** Protect against deeply nested or complex queries by implementing query depth and complexity limits.

---

# Idempotency

Network failures happen. Clients will retry requests. Your API must handle this safely.

**Idempotency Keys:** Require an `Idempotency-Key` header for state-changing operations (like payments or creations).
**Implementation:** Store the key and the resulting response. If the same key is received, return the stored response without re-executing the operation.
**TTL:** Idempotency keys should expire (e.g., after 24 hours).

---

# Pagination, Filtering, and Sorting

Never return an unbounded list of results.

**Pagination:** Use cursor-based pagination (`cursor`, `limit`) for large datasets and infinite scrolls. Offset-based (`offset`, `limit`) is acceptable for simple UI tables but degrades in performance at scale.
**Filtering:** Use query parameters for filtering (e.g., `?status=active&role=admin`).
**Sorting:** Use explicit parameters for sorting (e.g., `?sort=-created_at` for descending).

---

# Error Handling

Errors should be helpful to the developer consuming the API.

**Standardized Format:** Use a consistent JSON structure for all errors. Example:

```json
{
  "error": {
    "code": "validation_failed",
    "message": "The provided email is invalid.",
    "details": [
      { "field": "email", "issue": "Invalid format" }
    ]
  }
}
```

**HTTP Status Codes:** Use the correct semantic HTTP status code:

- `200 OK`: Success
- `201 Created`: Resource created
- `400 Bad Request`: Client error (validation)
- `401 Unauthorized`: Missing or invalid authentication
- `403 Forbidden`: Authenticated, but lacks permission
- `404 Not Found`: Resource does not exist
- `429 Too Many Requests`: Rate limit exceeded
- `500 Internal Server Error`: Server failure

Never return a 200 OK with an error payload.

---

# Versioning

APIs evolve. Do not break existing clients.

**URL Versioning:** Include the version in the URL path (e.g., `/v1/users`). This is the most explicit and pragmatic approach.
**Header Versioning:** Use custom headers (e.g., `Accept: application/vnd.company.v1+json`). More theoretically pure, but harder to debug and cache.
**Deprecation:** Announce deprecations well in advance. Return `Deprecation` and `Sunset` HTTP headers.

---

# Rate Limiting & Throttling

Protect your infrastructure from abuse and noisy neighbors.

**Implementation:** Implement rate limiting based on IP address or API key.
**Headers:** Return rate limit status in response headers:

- `X-RateLimit-Limit`: Maximum requests allowed
- `X-RateLimit-Remaining`: Requests left in the current window
- `X-RateLimit-Reset`: Timestamp when the limit resets
**Throttling:** Softly degrade performance before hitting hard limits, if possible.

---

# API Security

Secure your perimeter.

**Authentication:** Use industry standards like OAuth2 or JWT. Avoid custom authentication schemes.
**Authorization:** Implement RBAC (Role-Based Access Control) or ABAC (Attribute-Based Access Control) at the business logic layer, not just the API gateway.
**CORS:** Configure Cross-Origin Resource Sharing correctly. Only allow trusted domains.
**Input Validation:** Sanitize and validate all incoming data. Protect against SQL Injection and XSS.
**Rate Limiting:** (See above) Essential for preventing brute-force and DDoS attacks.

---

# The ForgeCore API Standard

A production-ready API is:
- **Intuitive:** Developers can guess how it works without constantly reading the docs.
- **Robust:** It handles bad input gracefully and never crashes.
- **Secure:** It protects user data and system integrity by default.
- **Stable:** It guarantees backward compatibility.

Design your API for the developer who will be integrating it at 2 AM on a Friday.
