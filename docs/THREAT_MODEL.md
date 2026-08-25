# Threat Modeling & Security Analysis of OWASP Juice Shop

**Author:** Mahrukh

## 1. Introduction

### 1.1 Project Overview
Web applications are widely used in modern systems but often contain vulnerabilities that can be exploited by attackers. This project analyzes security weaknesses in a web application using structured threat modeling techniques.

### 1.2 Objectives
- Understand the application architecture
- Create a Data Flow Diagram (DFD)
- Identify threats using STRIDE methodology
- Validate vulnerabilities through testing
- Perform risk analysis
- Recommend mitigation strategies

### 1.3 Selected Application
OWASP Juice Shop is an intentionally vulnerable web application designed for security training. It simulates an e-commerce platform and contains vulnerabilities such as SQL Injection, Cross-Site Scripting (XSS), and Broken Authentication.

## 2. System Architecture
See [`architecture/OVERVIEW.md`](../architecture/OVERVIEW.md) for the full component breakdown and Data Flow Diagram.

## 3. Threat Modeling (STRIDE)

### 3.1 STRIDE Overview
STRIDE is a threat modeling technique used to identify security risks: **S**poofing, **T**ampering, **R**epudiation, **I**nformation Disclosure, **D**enial of Service, **E**levation of Privilege.

### 3.2 Identified Threats

| # | Threat | STRIDE Category | Details |
|---|---|---|---|
| 1 | Weak Authentication | Spoofing | Users could register with trivial passwords like `123` |
| 2 | Cross-Site Scripting (XSS) | Tampering | Input fields allowed injection of malicious scripts |
| 3 | SQL Injection | Tampering | Login query vulnerable to logic manipulation |
| 4 | No Rate Limiting | Denial of Service | Login endpoint accepted unlimited attempts |
| 5 | Sensitive Data Exposure | Information Disclosure | Auth tokens exposed in API response body |
| 6 | Broken Access Control | Elevation of Privilege | Protected endpoints/pages reachable without authorization |

## 4. Threat Validation, Fixes & Testing

Each threat below was exploited, fixed at the code level, and re-verified. Full write-ups (with before/after screenshots) live in their own folders under [`fixes/`](../fixes/):

1. **Cross-Site Scripting (XSS)** → [`fixes/01-cross-site-scripting-xss/`](../fixes/01-cross-site-scripting-xss/)
2. **SQL Injection (Authentication Bypass)** → [`fixes/02-sql-injection/`](../fixes/02-sql-injection/)
3. **No Rate Limiting (Brute Force)** → [`fixes/03-no-rate-limiting/`](../fixes/03-no-rate-limiting/)
4. **Sensitive Data Exposure (Token Leakage)** → [`fixes/04-sensitive-data-exposure/`](../fixes/04-sensitive-data-exposure/)
5. **Broken Access Control / RBAC** → [`fixes/05-broken-access-control/`](../fixes/05-broken-access-control/)
6. **Weak Password Policy** → [`fixes/06-weak-password-policy/`](../fixes/06-weak-password-policy/)

## 5. Risk Assessment
See [`docs/RISK_ASSESSMENT.md`](RISK_ASSESSMENT.md) for the full risk matrix and OWASP Top 10 mapping.

## 6. Mitigation and Recommendations

**Fixes**
- Use input validation and sanitization
- Implement parameterized queries
- Use secure authentication mechanisms
- Apply proper access control

**Secure Practices**
- Use HTTPS encryption
- Apply the least privilege principle
- Perform regular security testing

## 7. Conclusion
This project demonstrated how vulnerabilities exist in web applications and how they can be exploited. Using threat modeling and testing, multiple security flaws were identified and analyzed. Implementing secure coding practices significantly reduces risk and improves overall system security.
