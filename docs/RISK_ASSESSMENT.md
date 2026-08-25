# Risk Assessment

Risk was scored using a simple **Impact × Likelihood → Severity** model, consistent with the STRIDE findings validated in each [`fixes/`](../fixes/) folder.

| Threat | Impact | Likelihood | Severity | OWASP Top 10 Mapping |
|---|:---:|:---:|:---:|---|
| Weak Authentication (password policy + rate limiting) | High | High | **Critical** | A07:2021 – Identification & Authentication Failures |
| Cross-Site Scripting (XSS) | Medium | High | **High** | A03:2021 – Injection |
| Sensitive Data Exposure (Token Leakage) | High | Medium | **High** | A02:2021 – Cryptographic Failures |
| Broken Access Control / Privilege Escalation | Critical | Medium | **Critical** | A01:2021 – Broken Access Control |
| SQL Injection | Critical | Medium | **Critical** | A03:2021 – Injection |

## Severity Definitions
- **Critical** — Directly leads to full account/system compromise; must be fixed before release.
- **High** — Significant data or session exposure; high priority fix.
- **Medium** — Meaningful risk but requires specific conditions to exploit.

## Overall Assessment
The application's most severe risks stemmed from **authentication weaknesses** (password policy, brute-force exposure) and **broken access control**, both of which allow full compromise of user accounts or unauthorized data access. All identified issues were reproduced, fixed at the code level, and re-verified using both manual browser testing and Burp Suite request interception. See [`fixes/`](../fixes/) for the full before/after evidence per vulnerability.
