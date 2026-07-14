# Microservices Architecture & Distributed Systems

> Distilled engineering knowledge for designing, scaling, and operating distributed systems.

Microservices solve organizational scaling problems, not technical scaling problems. If you cannot build a well-structured monolith, you will build a distributed monolith. Introduce microservices only when independent deployability and organizational boundaries demand it.

---

# Core Principles

Every distributed system should satisfy:

**High Cohesion, Loose Coupling:** Services that change together should be deployed together. Services should not share databases.
**Independent Deployability:** You must be able to deploy a single service to production without deploying any other service.
**Resilience over Reliability:** Expect individual components to fail. The system as a whole must gracefully degrade rather than collapse.
**Observability by Default:** Distributed systems are impossible to debug without centralized logging, distributed tracing, and metrics.

---

# Domain-Driven Design (DDD)

Microservices must align with business capabilities.

**Bounded Contexts:** Define strict boundaries around domain models. A `User` in the Billing context has different attributes than a `User` in the Shipping context. Do not force a single enterprise data model.
**Ubiquitous Language:** Use the same terminology in code as the business uses in meetings.
**Context Mapping:** Explicitly define how different bounded contexts interact (e.g., Customer/Supplier, Anti-Corruption Layer, Conformist).

---

# Service Communication

How services talk to each other determines the system's coupling and performance.

**Synchronous (HTTP/gRPC):**
- **Pros:** Simple, immediate feedback.
- **Cons:** Creates tight temporal coupling. If Service B is down, Service A fails. Cascading failures are highly likely.
- **When to use:** For querying data (Reads) or operations that require immediate consistency.

**Asynchronous (Event-Driven / Messaging):**
- **Pros:** Loose temporal coupling, highly scalable, resilient to downstream outages.
- **Cons:** Eventual consistency, harder to trace, requires message brokers (Kafka, RabbitMQ).
- **When to use:** For state changes (Writes) and cross-domain business processes.

**API Gateways:** Use a gateway to handle cross-cutting concerns (auth, rate limiting, routing) at the edge, so internal services do not have to.

---

# Distributed Data Management

The hardest part of microservices is the data.

**Database-per-Service:** The golden rule. Services must encapsulate their data and only expose it via APIs or events. Never integrate via a shared database.
**Event Sourcing:** Store state changes as a sequence of immutable events. This provides a perfect audit log and allows reconstructing state at any point in time.
**CQRS (Command Query Responsibility Segregation):** Separate the read model from the write model. Optimize writes for consistency and reads for performance.

---

# Distributed Transactions

ACID transactions do not span microservice boundaries.

**The Saga Pattern:** A sequence of local transactions. Each local transaction updates the database and publishes an event or message to trigger the next local transaction in the saga.
**Choreography vs. Orchestration:**

- *Choreography:* Services react to events published by other services. Good for simple workflows, but can become a tangled mess (event spaghetti).

- *Orchestration:* A central coordinator service tells other services what to do. Good for complex workflows, but introduces a single point of failure and tighter coupling.
**Compensating Transactions:** Because you cannot roll back a distributed transaction, you must write explicit compensating logic to undo a previous step if a subsequent step fails.

---

# Resilience Patterns

Prevent local failures from becoming global outages.

**Timeouts:** Fail fast. Never wait indefinitely for a response.
**Retries & Jitter:** Retry transient failures, but add random jitter to avoid thundering herd problems.
**Circuit Breakers:** If a downstream service is failing, open the circuit and return an error immediately. Periodically test the service to see if it has recovered.
**Bulkheads:** Isolate resources (e.g., thread pools, connection pools) so a failure in one area does not consume all resources and bring down the entire service.

---

# The ForgeCore Microservices Standard

A production-ready distributed system is:
- **Autonomous:** Teams can develop, test, and deploy their services independently.
- **Eventual:** It embraces eventual consistency for cross-domain operations.
- **Traceable:** Every request has a unique correlation ID that flows through all logs and traces.
- **Monolith-First (Usually):** It started as a well-structured monolith and was extracted into services only when organizational boundaries required it.
