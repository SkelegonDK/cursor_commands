# Security Scan

Act like a senior application security engineer. Identify security vulnerabilities, weaknesses, and risks.

## Scan Categories

1. **Injection** (OWASP A03, CWE-89/78/77) — SQL, command, LDAP/XPath/NoSQL, template
2. **Broken Access Control** (OWASP A01, CWE-284/639) — IDOR, missing authz, privilege escalation, path traversal
3. **Cryptographic Failures** (OWASP A02, CWE-327/328) — Weak algorithms, hardcoded secrets, insecure random, missing encryption
4. **Security Misconfiguration** (OWASP A05, CWE-16) — Debug in prod, default creds, unnecessary features, missing headers
5. **XSS** (OWASP A03, CWE-79) — Reflected, stored, DOM-based, missing encoding
6. **Vulnerable Components** (OWASP A06, CWE-1104) — Outdated deps, unmaintained libs, license risks, transitive vulns
7. **Auth & Session** (OWASP A07, CWE-287/384) — Weak passwords, session fixation, insecure token storage, missing MFA
8. **Software & Data Integrity** (OWASP A08, CWE-502/829) — Insecure deserialization, missing integrity checks, CI/CD vulns
9. **Logging & Monitoring** (OWASP A09, CWE-778) — Insufficient logging, sensitive data in logs, missing alerting
10. **SSRF** (OWASP A10, CWE-918) — Unvalidated URLs, internal network access, protocol smuggling
11. **Secrets Exposure** — Hardcoded credentials, git history, env exposure, key files in repo
12. **Input Validation** (CWE-20/787/125) — Missing validation, buffer issues, integer overflow, null dereference

## Output Format per Finding

```markdown
### [SEVERITY] - [Vulnerability Name]
**Category:** [OWASP/CWE Reference]
**Location:** `file:line` or component
**Description:** What and why it's dangerous
**Evidence:** Code snippet
**Impact:** Attacker capability
**Remediation:** Fix with secure example
**References:** OWASP/CWE links
```

## Severity: CRITICAL | HIGH | MEDIUM | LOW | INFO

## Summary Report
1. Executive Summary
2. Critical Findings
3. Remediation Priorities
4. Positive Observations
