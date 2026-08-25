# 6. Weak Password Policy

**STRIDE category:** Spoofing
**Severity:** Critical
**Location:** `server/models/User.ts` (Password field in the User model)

## Description
The application allowed users to register with weak passwords, with no validation rules enforcing minimum length or complexity.

## Cause
The password field directly hashed and stored user input without checking its strength.

## Impact
- Attackers can easily guess or brute-force weak passwords
- Leads to unauthorized account access
- Increases risk of account takeover and data breaches
- Weak credentials reduce overall system security

## Proof of Concept
Registration was accepted for trivially weak passwords, including:
- `12345`
- `password`
- `abc123`

Such passwords are highly vulnerable to dictionary and brute-force attacks.

📷 `screenshots/before-weak-password-accepted.png`

## Fix Implementation

**Backend (Node.js/Express — Sequelize model)**
- Added password validation rules directly in the `User` model.
- Implemented regex-based validation before hashing the password.

**Enforced rules:**
- Minimum length: 8 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one number
- At least one special character

## Code Fix Summary
- Used Sequelize's `validate` property.
- Applied a regex pattern to enforce strong password rules.
- Ensured only strong passwords are accepted before storing in the database.

## Testing

| Test Case | Input | Result |
|---|---|---|
| Weak password | `12345` | ❌ Rejected |
| Weak password | `password` | ❌ Rejected |
| Strong password | `Mahrukh@123` | ✅ Accepted |

📷 `screenshots/after-fix-weak-password-rejected.png`
📷 `screenshots/after-fix-strong-password-accepted.png`

## Result
✅ Weak passwords are no longer accepted. Application security improved against brute-force and credential attacks — user authentication is now significantly more secure.
