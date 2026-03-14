# Security Policy

## Scope

ZLAR-FL is a fleet governance tool that aggregates audit trails and policy status across multiple ZLAR-governed agents. It provides a single view of fleet health, drift detection, and compliance posture.

ZLAR-FL is an observability tool — it does not enforce policy or modify agent behavior. However, vulnerabilities that could cause it to report incorrect fleet status, miss drift conditions, or leak audit data across fleet boundaries are in scope.

---

## Reporting a Vulnerability

**Do not open a public issue for security vulnerabilities.**

Use GitHub's private vulnerability reporting:

1. Go to [ZLAR-AI/ZLAR-FL Security Advisories](https://github.com/ZLAR-AI/ZLAR-FL/security/advisories)
2. Click **"Report a vulnerability"**
3. Provide a clear description, reproduction steps, and affected components

We will acknowledge receipt within 48 hours and provide a timeline for remediation.

If you cannot use GitHub's reporting tool, email **hello@zlar.ai** with the subject line "Security: ZLAR-FL" and we will establish a private channel.

---

## Threat Model

**Registry manipulation.** The fleet registry is a plaintext artifact. An attacker with file access could add phantom agents or remove real ones to hide compromised fleet members. The registry is not encrypted or signed (documented known limitation).

**Cross-agent data leakage.** FL aggregates audit data from multiple agents. A vulnerability could allow one agent's data to be attributed to or visible from another agent's reports.

### Out of Scope

ZLAR-FL does not enforce policy on individual agents. For per-agent governance, see [ZLAR Gate](https://github.com/ZLAR-AI/ZLAR-Gate) or [ZLAR-OC](https://github.com/ZLAR-AI/ZLAR-OC).

---

## Supported Versions

| Version | Supported |
|---------|-----------|
| main (HEAD) | Yes |

---

## Disclosure Policy

We practice coordinated disclosure. Acknowledgment within 48 hours, severity assessment within 7 days, fix and advisory published, credit offered.

We ask that reporters refrain from public disclosure until a fix is available, or until 90 days have elapsed — whichever comes first.
