# Threat 4: Sensitive Data Exposure (Token Leakage)

## Description
- **Type:** Code-level vulnerability (Sensitive Data Exposure)
- **Location:** Login response (`/rest/user/login`)
- **Cause:** The application returned the authentication token inside the API response body after a successful login, making it accessible to client-side scripts and browser storage
- **Impact:** Attackers could steal authentication tokens via XSS attacks or browser inspection tools, leading to session hijacking and unauthorized account access

## Proof of Concept
After logging in, the authentication token was visible directly in browser storage / the Application panel of DevTools — meaning any client-side script (e.g. an XSS payload) could read and exfiltrate it.

## Fix Implementation
**Backend (Node.js/Express):**
- Removed the token from the API response body
- Implemented secure cookie-based storage for the authentication token

**Configuration Applied:**
- `httpOnly: true` → prevents JavaScript access (protects against XSS)
- `secure: true` → ensures transmission over HTTPS only
- `sameSite: 'strict'` → prevents CSRF attacks

## Testing (Web + Browser Verification)
1. **Web Application Testing**
   - Before fix: token visible in the API response (Inspect → Network tab)
   - After fix: token no longer visible in the response body
2. **Browser Storage Testing**
   - Before fix: token accessible via Local/Session Storage or JavaScript
   - After fix: token stored only in an HTTP-only cookie, inaccessible to JavaScript

## Result
The sensitive data exposure vulnerability was successfully mitigated by removing the token from the API response and storing it securely in an HTTP-only cookie, significantly improving session security.

## Screenshots
| # | File | Description |
|---|---|---|
| 1 | `01-token-visible-in-storage.png` | Token visible in browser storage before the fix |
| 2 | `02-httponly-cookie-fix-code.png` | Backend code implementing the HTTP-only cookie fix |
| 3 | `03-token-not-in-storage-after-fix.jpg` | Browser storage showing no exposed token after the fix |
