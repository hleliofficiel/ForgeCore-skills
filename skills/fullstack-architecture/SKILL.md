# Fullstack Architecture & Application Design

> Distilled engineering knowledge for designing and building complete, end-to-end web applications.

Fullstack engineering is not about knowing every framework. It is about understanding the boundaries between systems—how the database, backend, and frontend communicate securely, efficiently, and reliably to deliver a cohesive product.

---

# Core Principles

Every fullstack application should satisfy:

**Clear Boundaries:** The frontend and backend should be decoupled. They communicate strictly through well-defined API contracts.
**Single Source of Truth:** State should live in the database. The frontend is a projection of that state, and the backend is the gatekeeper.
**Security at the Edge and Core:** Validate data on the frontend for user experience, but validate *everything* on the backend for security. Never trust the client.
**Progressive Enhancement:** Core functionality should work under constrained conditions (e.g., slow networks).

---

# Rendering Strategies

Choose the right rendering pattern based on the product's needs.

**Client-Side Rendering (CSR):**
- Entire application loads in the browser (e.g., standard React/Vue apps).
- Best for highly interactive dashboards where SEO is irrelevant.
- Suffer from slow initial load times (Time to Interactive).

**Server-Side Rendering (SSR):**
- HTML is generated on the server per request (e.g., Next.js, Remix, traditional MVCs).
- Best for dynamic content that requires strong SEO and fast First Contentful Paint.
- Increases server load and requires node/runtime servers.

**Static Site Generation (SSG):**
- HTML is generated at build time.
- Best for marketing sites, blogs, and documentation.
- Unbeatable performance and security, but requires rebuilding for content updates.

---

# The BFF Pattern (Backend for Frontend)

Do not force the frontend to orchestrate complex domain logic.

**Aggregation:** Instead of the client making 5 requests to 5 microservices, introduce a BFF layer (e.g., a GraphQL server or a dedicated REST gateway). The client makes 1 request to the BFF; the BFF aggregates the data.
**Formatting:** The BFF should format data exactly as the UI needs it, minimizing frontend data transformation logic.
**Security Context:** The BFF can hold secure HttpOnly cookies and manage sessions, exchanging them for short-lived tokens to communicate with downstream services.

---

# State Management Across Boundaries

State is the hardest part of fullstack development.

**Server State:** Data that lives in the database. Use tools like React Query, SWR, or Apollo to fetch, cache, synchronize, and update server state in the frontend. Do not put server state into Redux/Zustand.
**Client State:** Ephemeral UI state (e.g., "is this modal open?"). Use lightweight local state (React `useState`, Vue `ref`) or global client stores (Zustand, Pinia).
**URL State:** The most underrated state manager. Store filters, pagination, and active tabs in the URL query parameters so users can share links and refresh without losing context.

---

# Integration & Type Safety

End-to-end type safety eliminates entire categories of bugs.

**Shared Contracts:** Use tools like OpenAPI (Swagger) to generate frontend clients from backend code, or use GraphQL introspection.
**Monorepos:** If using TypeScript across the stack (e.g., tRPC, Next.js), share type definitions directly between frontend and backend in a monorepo workspace (Turborepo, Nx).

---

# The ForgeCore Fullstack Standard

A production-ready fullstack application is:
- **Cohesive:** It feels like one product, even if it is powered by a dozen services.
- **Type-Safe:** Changing a database column fails the frontend build if it breaks the contract.
- **Resilient:** Network failures are caught, retried, and surfaced to the user gracefully.
