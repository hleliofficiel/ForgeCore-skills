# Site Reliability Engineering (SRE)

> Distilled engineering knowledge for operating massive scale systems, measuring reliability, and managing incidents.

SRE is what happens when you ask a software engineer to design an operations team. It replaces manual sysadmin tasks with automation, treats operations as a software problem, and accepts that 100% reliability is both impossible and financially irresponsible.

---

# Core Principles

Every SRE practice should satisfy:

**Embrace Risk:** Understand that pushing new features always introduces risk. The goal is to manage that risk, not eliminate it.
**Error Budgets:** A mathematically defined allowance for failure. Once the budget is spent, feature releases freeze, and the team focuses strictly on reliability.
**Toil Reduction:** Toil is manual, repetitive, automatable, tactical work. SREs should cap toil at 50% of their time, spending the rest on engineering long-term solutions.
**Measure Everything:** You cannot improve what you do not measure from the perspective of the end-user.

---

# SLIs, SLOs, and SLAs

The vocabulary of reliability.

**Service Level Indicator (SLI):**
- A quantitative measure of some aspect of the level of service that is provided.

- *Example:* The proportion of HTTP requests that complete successfully in under 200ms.

- *Formula:* `Good Events / Total Events * 100`

**Service Level Objective (SLO):**
- A target value or range of values for a service level that is measured by an SLI.

- *Example:* 99.9% of HTTP requests will complete in under 200ms over a rolling 30-day window.
- This is the internal target that drives engineering decisions.

**Service Level Agreement (SLA):**
- An explicit or implicit contract with your users that includes consequences of meeting (or missing) the SLOs.

- *Example:* If uptime drops below 99.9%, we refund 10% of the customer's monthly bill.
- SLAs are business contracts; SLOs are engineering targets. SLOs must always be stricter than SLAs.

---

# Incident Management

Chaos requires structure. When P1 incidents occur, follow a rigid protocol.

**The Roles:**

- *Incident Commander (IC):* Holds all authority. Coordinates the response, makes decisions, and delegates. Does *not* look at code or logs.

- *Communications Lead:* Translates technical jargon into business updates for stakeholders and customers.

- *Operations Lead:* The engineers actually digging through logs, deploying fixes, and resolving the issue.

**The Process:**
1. **Declare:** Someone notices an anomaly and declares an incident.
2. **Contain:** The immediate goal is to stop the bleeding (e.g., rollback a deploy, scale up resources, isolate a bad node). Do not focus on root cause analysis yet.
3. **Resolve:** Once contained, find and fix the root cause.
4. **Post-Mortem:** Document what happened.

---

# Blameless Post-Mortems

The most critical artifact of an incident.

**Focus on Systems, Not People:** Human error is a symptom, not a cause. If an engineer deleted the production database, the post-mortem must ask *why* the system allowed them to do that without safeguards.
**Actionable Preventatives:** Every post-mortem must result in Jira tickets (or equivalent) that prevent the specific failure from happening again.
**Transparency:** Share post-mortems widely across the engineering organization so other teams can learn from the failure.

---

# Capacity Planning & Load Testing

Do not wait for Black Friday to see if your system scales.

**Load Testing:** Simulating expected peak traffic to verify the system handles it gracefully.
**Stress Testing:** Pushing the system beyond expected limits until it breaks, to understand failure modes and bottlenecks.
**Chaos Engineering (Game Days):** Intentionally injecting failures (killing pods, adding network latency, dropping database connections) into production or staging environments to verify the system's resilience mechanisms work in practice.

---

# The ForgeCore SRE Standard

A production-ready SRE culture means:
- **Data-Driven:** Arguments about reliability are settled by looking at SLOs and error budgets.
- **Blameless:** Engineers are not afraid to admit mistakes because the culture focuses on fixing systems.
- **Proactive:** Teams hunt for vulnerabilities using chaos engineering before those vulnerabilities cause an outage.
