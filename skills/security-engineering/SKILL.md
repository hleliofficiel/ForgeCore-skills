# Security Engineering

> Distilled engineering knowledge for building secure applications and protecting user data.

Security is not a feature you add at the end of a project. It is an architectural property that must be considered at every stage of the software lifecycle. A secure system assumes breach and minimizes the blast radius.

---

# Core Principles

Every secure system should satisfy:

**Defense in Depth:** Employ multiple layers of security controls (e.g., firewall + WAF + authentication + input validation + database encryption). If one fails, others remain.
**Zero Trust Architecture:** Never trust any user, device, or network by default, even if they are inside the corporate perimeter. Always verify explicitly.
**Least Privilege:** Grant users and services only the minimum permissions necessary to perform their function, for the minimum amount of time required.
**Fail Securely:** When a system fails, it should default to a secure state (e.g., denying access rather than granting it).
**Simplicity:** Complexity is the enemy of security. Keep architectures and access models as simple as possible.

---

# Threat Modeling

Think like an attacker to defend like an engineer.

**Identify Assets:** What data or systems are most valuable?
**Identify Threats (STRIDE):**

- **S**poofing: Can someone pretend to be another user?
- **T**ampering: Can someone modify data they shouldn't?
- **R**epudiation: Can someone perform an action and deny they did it? (Need audit logs)
- **I**nformation Disclosure: Can someone view data they shouldn't?
- **D**enial of Service: Can someone crash the system or make it unavailable?
- **E**levation of Privilege: Can a regular user become an admin?
**Mitigate:** Design architectural solutions for identified threats before writing code.

---

# Application Security (AppSec)

Secure the code you write.

**Input Validation:** Never trust client input. Validate everything at the server boundary against a strict allowlist of expected formats, lengths, and types.
**Output Encoding:** Protect against Cross-Site Scripting (XSS) by contextually encoding all user-supplied data before rendering it in the browser.
**Parameterized Queries:** Always use parameterized queries or an ORM to prevent SQL Injection. Never concatenate strings to build SQL queries.
**Secure Authentication:** Use modern protocols (OIDC, OAuth2). Implement multi-factor authentication (MFA). Hash passwords using strong algorithms (Argon2, bcrypt) with unique salts.
**Session Management:** Use secure, HttpOnly, SameSite cookies. Set short expiration times and rotate session identifiers on privilege escalation (login).

---

# Dependency Management

Secure the code you don't write.

**Software Bill of Materials (SBOM):** Know exactly what open-source libraries are in your application.
**Continuous Scanning:** Use tools (e.g., Dependabot, Snyk) to automatically scan for known vulnerabilities (CVEs) in dependencies.
**Pin Dependencies:** Pin exact versions of dependencies to prevent supply chain attacks via malicious updates. Update deliberately and test thoroughly.

---

# Infrastructure Security

Secure the environment where the code runs.

**Network Segmentation:** Use Virtual Private Clouds (VPCs) and subnets to isolate resources. Databases should never be exposed directly to the public internet.
**Secret Management:** Do not store API keys, passwords, or certificates in source code. Use a secure vault (e.g., HashiCorp Vault, AWS Secrets Manager) and inject them at runtime.
**Immutable Infrastructure:** Do not SSH into servers to make changes. Update the configuration as code and deploy new instances.

---

# Cryptography

Do not invent your own cryptography.

**Data in Transit:** Enforce TLS 1.2 or higher for all network communication. Use HSTS headers.
**Data at Rest:** Encrypt sensitive data (PII, credentials, financial data) in the database and on disk.
**Key Management:** Use managed Key Management Services (KMS) to handle the generation, rotation, and storage of cryptographic keys.

---

# Incident Readiness

Assume you will be breached. Prepare for it.

**Audit Logging:** Log all security-relevant events (logins, permission changes, data access). Ensure logs are immutable and centralized.
**Incident Response Plan:** Have a documented plan for how to handle a breach, including communication strategies, containment procedures, and legal obligations.
**Regular Audits:** Conduct regular penetration testing and security reviews.

---

# The ForgeCore Security Standard

A production-ready secure system is:
- **Resilient:** It withstands automated attacks and slows down targeted attacks.
- **Transparent:** It logs enough information to reconstruct exactly what happened during an incident.
- **Proactive:** It integrates security checks into the CI/CD pipeline, catching vulnerabilities before they reach production.

Security is not about perfection; it is about raising the cost of an attack until it is no longer profitable for the attacker.
