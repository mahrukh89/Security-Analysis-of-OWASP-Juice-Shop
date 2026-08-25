# Threat 2: SQL Injection (Authentication Bypass)

## Description
- **Type:** Code-level vulnerability (Injection)
- **Location:** Login authentication endpoint (`/rest/user/login`)
- **Cause:** User input was directly used in the database query without proper parameterization or sanitization, allowing SQL logic manipulation
- **Impact:** Attackers could bypass authentication using malicious input (e.g. `' OR 1=1--`), leading to unauthorized access and account takeover

## Proof of Concept
The classic authentication-bypass payload `' OR 1=1--` was submitted in the login form's email field. Before the fix, this logged the attacker in without valid credentials. Burp Suite was used to intercept and inspect the raw login request/response confirming the payload reached the backend.

## Fix Implementation
**Backend (Node.js/Express):**
- Replaced unsafe string-based query handling with secure parameterized / ORM-based query logic (Sequelize)
- Ensured all user input is treated strictly as data, not executable SQL
- Strengthened authentication logic to validate credentials properly before granting access

**Frontend (Angular):**
- Added basic input validation on the login form to prevent malformed input submission

## Testing (Web + Burp Suite Verification)
1. **Web Application Testing** — Payload `' OR 1=1--` tested directly on the login page.
   - Before fix: allowed login bypass and unauthorized access
   - After fix: login correctly rejected ("Invalid email or password")
2. **Burp Suite Verification** — Login request intercepted and modified with malicious payloads in the request body.
   - Server response confirmed injected SQL payloads were treated as normal input and did not affect authentication

## Result
The SQL injection vulnerability was successfully mitigated by implementing secure parameterized authentication logic on the backend and adding input validation on the frontend.

## Screenshots
| # | File | Description |
|---|---|---|
| 1 | `01-login-page-payload.png` | `' OR 1=1--` payload entered on the login page |
| 2 | `02-after-fix-redirect.jpg` | Products page reached (before-fix behavior context) |
| 3 | `03-burpsuite-request-response.png` | Burp Suite request/response for the login attempt |
| 4 | `04-vulnerable-code-snippet.png` | Backend query code referenced during validation |
| 5 | `05-login-rejected-after-fix.png` | Burp Suite confirming 401 Unauthorized after the fix |
