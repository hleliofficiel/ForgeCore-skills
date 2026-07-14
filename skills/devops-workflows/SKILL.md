# DevOps Workflows & Operations

> Distilled engineering knowledge for continuous integration, deployment, infrastructure, and operational reliability.

DevOps is not a team or a title. It is a culture and a set of practices designed to break down silos between development and operations. The goal is to deliver high-quality software safely, quickly, and sustainably.

---

# Core Principles

Every operational workflow should satisfy:

**Automation:** If a task must be done more than once, script it. Human intervention introduces errors.
**Immutability:** Infrastructure and artifacts should not be modified after creation. Replace, do not patch.
**Version Control Everything:** Code, configuration, infrastructure definitions, and deployment scripts all belong in Git.
**Shift Left:** Integrate security, testing, and compliance as early in the development lifecycle as possible.
**Continuous Feedback:** Failures must be visible immediately to the engineer who caused them.

---

# Continuous Integration (CI)

CI is the practice of merging code to a central repository frequently and validating it automatically.

**The Golden Rule:** The main branch must always be deployable.
**Fast Feedback:** The CI pipeline must be fast (ideally under 10 minutes). Slow pipelines destroy productivity.
**Automated Testing:** CI must run unit tests, integration tests, and static analysis (linting, type checking).
**Artifact Generation:** CI should build the final deployment artifact (e.g., a Docker image) once. Do not rebuild in later stages.

---

# Continuous Deployment / Delivery (CD)

CD is the practice of automating the release of software to environments.

**Deployment is Not Release:** Separating deployment (putting code on servers) from release (enabling it for users via feature flags) reduces risk.
**Environment Parity:** Staging and production environments must be as identical as possible. Differences cause deployment surprises.
**Automated Rollbacks:** If a deployment fails health checks, it should automatically roll back to the previous stable version.

---

# Deployment Strategies

Choose the right strategy for the risk profile of the application.

**Blue/Green Deployment:** Run two identical environments. Route traffic to Blue. Deploy new code to Green. Switch the router to Green. Zero downtime, easy rollback.
**Canary Release:** Deploy new code to a small percentage of servers or users. Monitor metrics. If stable, roll out entirely.
**Rolling Update:** Update instances one by one. Slower, but requires less infrastructure overhead than Blue/Green.

---

# Infrastructure as Code (IaC)

Infrastructure should be defined in code, reviewed via PR, and applied automatically.

**Declarative over Imperative:** Define *what* the infrastructure should look like (e.g., Terraform), not *how* to build it (e.g., bash scripts).
**Idempotent Provisioning:** Running the IaC script multiple times should result in the same state.
**State Management:** Securely manage and backup IaC state files. Never store secrets in state.

---

# Observability and Monitoring

You cannot operate what you cannot see.

**The Three Pillars:**
1. **Metrics:** Time-series data (e.g., CPU, HTTP requests, error rates). Used for alerting and dashboards.
2. **Logs:** Immutable records of discrete events. Used for debugging after an alert fires.
3. **Traces:** Representation of a single request across multiple services. Essential for microservices.

**Alerting Philosophy:**

- Alert on symptoms (e.g., "Checkout failure rate > 5%"), not causes (e.g., "CPU at 90%").
- Every alert must be actionable. If an alert requires no action, delete it.
- Route alerts appropriately (PagerDuty for critical, Slack for informational).

---

# Incident Response

Failures are inevitable. How you handle them defines your engineering culture.

**Blameless Post-Mortems:** Focus on *why* the system allowed the failure, not *who* caused it. Human error is a symptom of a systemic problem.
**Runbooks:** Maintain documented procedures for common alerts and operational tasks.
**Incident Command:** During a major outage, designate an Incident Commander to coordinate communication and a technical lead to focus on the fix.

---

# Security in Operations (DevSecOps)

Security is everyone's responsibility.

**Secret Management:** Never commit secrets to version control. Use a dedicated secret manager (e.g., AWS Secrets Manager, HashiCorp Vault).
**Least Privilege Infrastructure:** IAM roles and service accounts should have the minimum permissions necessary.
**Vulnerability Scanning:** Automatically scan container images and dependencies for known vulnerabilities in the CI pipeline.

---

# The ForgeCore Operational Standard

Production-ready operations mean:
- **Confidence:** Engineers deploy at 4 PM on a Friday without fear, because the systems prevent catastrophic failure.
- **Reproducibility:** You can rebuild the entire infrastructure from scratch using code.
- **Visibility:** When something breaks, the system tells you exactly where to look.

Operations is an engineering discipline. Treat infrastructure like software.
