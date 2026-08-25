# Threat 3: No Rate Limiting (Brute Force Attack)

## Description
- **Type:** Code-level vulnerability (Authentication / Brute Force)
- **Location:** Login endpoint (`/rest/user/login`)
- **Cause:** The application does not restrict repeated login attempts, allowing unlimited password guesses without any delay or blocking mechanism
- **Impact:** Attackers can perform brute force attacks by repeatedly trying different password combinations, potentially leading to unauthorized account access

## Proof of Concept
Multiple incorrect password attempts (10–15 rapid attempts) were made directly on the login page. Before the fix, the system allowed unlimited attempts with no blocking or delay.

## Fix Implementation
**Backend (Node.js/Express):**
- Implemented rate-limiting middleware using `express-rate-limit`
- Restricted the number of login attempts within a defined time window
- Blocked excessive requests once the limit was exceeded

**Configuration Applied:**
- Maximum attempts: 5 requests
- Time window: 1 minute
- Block response: `HTTP 429 (Too Many Requests)`

## Testing (Web + Burp Suite Verification)
1. **Web Application Testing** — Repeated failed login attempts made directly on the login page.
   - Before fix: unlimited attempts allowed, no blocking or delay
   - After fix: repeated failed attempts triggered a block, preventing further login attempts
2. **Burp Suite Testing** — Login requests intercepted and repeatedly resent with incorrect credentials.
   - After fix: server rejected excessive requests and returned `429 Too Many Requests`

## Result
The brute-force vulnerability was successfully mitigated by implementing rate limiting on the login endpoint, confirmed by both direct web testing and Burp Suite testing.

## Screenshots
| # | File | Description |
|---|---|---|
| 1 | `01-repeated-login-attempt-1.png` | First rapid failed login attempt |
| 2 | `02-repeated-login-attempt-2.png` | Second rapid failed login attempt |
| 3 | `03-repeated-login-attempt-3.png` | Third rapid failed login attempt |
| 4 | `04-repeated-login-attempt-4.png` | Fourth rapid failed login attempt |
| 5 | `05-login-code-reference.png` | Login handler code referenced during testing |
| 6 | `06-login-block-after-fix.png` | Login blocked after repeated attempts (after fix) |
| 7 | `07-burpsuite-429-response.png` | Burp Suite showing `429 Too Many Requests` response |
