# Threat 5: Broken Access Control / RBAC (Unauthorized Endpoint Access)

## Description
- **Type:** Code-level vulnerability (Broken Access Control / RBAC)
- **Location:** Protected API endpoint (`/api/users`)
- **Impact:** The application did not properly enforce authentication checks on protected routes. Unauthorized users could attempt to access sensitive endpoints, potentially exposing user data or triggering unintended system behavior. Inconsistent error handling also risked leaking internal application details.
- **Severity:** High (OWASP A01: Broken Access Control)

## Proof of Concept
Requesting a protected route without authentication returned an unhandled server error revealing internal stack traces and file paths, instead of a clean, generic access-denied response.

## Fix Implementation
**Backend (Node.js/Express):**
- Implemented strict authentication validation before processing requests
- Ensured only authenticated users can access protected endpoints
- Standardized error handling to prevent leakage of internal details

**Security Improvements Applied:**
- Unauthorized requests now return `401 Unauthorized` with a generic error message (no sensitive details)
- Removed exposure of internal stack traces and system paths
- Ensured no sensitive data is returned without valid authentication

## Testing (Web Verification)
1. **Unauthorized Access Test** — `GET /api/users` without authentication
   - Before fix: endpoint returned unintended responses / verbose errors
   - After fix: server returns `401 Unauthorized – No Authorization header was found`, no data exposed
2. **Data Exposure Verification** — checked response in browser DevTools (Inspect → Network tab)
   - Before fix: potential exposure of internal details or user data
   - After fix: no sensitive data returned, only a secure generic error message

## Result
The broken access control vulnerability was successfully mitigated by enforcing strict authentication checks on protected endpoints. Unauthorized users are now prevented from accessing sensitive resources, and the system no longer exposes internal details.

## Screenshots
| # | File | Description |
|---|---|---|
| 1 | `01-unauthenticated-500-error.png` | Verbose 500 error with stack trace before the fix |
| 2 | `02-access-control-fix-code.png` | Backend code enforcing strict auth checks and standardized errors |
| 3 | `03-401-unauthorized-after-fix.png` | Clean `401 Unauthorized` response after the fix |
