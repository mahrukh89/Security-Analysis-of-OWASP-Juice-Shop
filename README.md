# 1. Cross-Site Scripting (XSS)

**STRIDE category:** Tampering
**Severity:** High
**Location:** User comment / search fields

## Description
Input fields (including the search bar and product comment fields) rendered user-supplied content without proper context-aware output encoding, allowing injection of executable JavaScript.

## Cause
Inadequate escaping of user input before rendering it back into the page (unsafe HTML rendering instead of Angular's built-in escaping).

## Impact
Attackers could execute arbitrary scripts in the victim's browser, leading to session hijacking, credential theft, or page defacement.

## Proof of Concept
Payloads tested:
- `<script>alert('XSS')</script>`
- `<img src=x onerror=alert('XSS')>`

**Before fix:** Both payloads executed — an alert box popped up in the browser, confirming the script ran.

📷 `screenshots/before-script-payload.png`
📷 `screenshots/before-img-onerror-payload.png`

## Fix Implementation

**Frontend (Angular)**
- Replaced unsafe rendering with Angular's automatic escaping — `{{ comment.text }}` instead of `innerHTML`.

**Backend (Node.js/Express)**
- Added `xss-clean` middleware to sanitize incoming comment data.
- Implemented server-side validation to reject payloads containing `<script>` tags or inline event handlers.

## Testing (Burp Suite Verification)

| Test | Payload | Before Fix | After Fix |
|---|---|---|---|
| Script Injection | `<script>alert('XSS')</script>` | Alert executed in browser | Rendered as escaped text: `&lt;script&gt;alert('XSS')&lt;/script&gt;` |
| Event Handler Injection | `<img src=x onerror=alert('XSS')>` | Alert triggered via image error | Payload sanitized, displayed harmlessly as text |

📷 `screenshots/after-fix-escaped-output.png`
📷 `screenshots/burp-suite-intercept.png`

## Result
✅ XSS mitigated via context-aware output escaping on the frontend, combined with backend sanitization and validation. Burp Suite confirmed no script execution after the fix.
