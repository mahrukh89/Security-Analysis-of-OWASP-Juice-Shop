# Threat 6: Weak Password Policy

## Description
- **Type:** Code-level vulnerability (Weak Authentication / Weak Password Policy)
- **Location:** `server/models/User.ts` (Password field in the User model)
- **Issue:** The application allowed users to register with weak passwords without enforcing any validation rules such as minimum length or complexity requirements

## Vulnerability Explanation
The password field directly hashed and stored user input without checking its strength, allowing passwords such as `12345`, `password`, and `abc123` — all highly vulnerable to dictionary and brute-force attacks.

## Impact
- Attackers can easily guess or brute-force weak passwords
- Leads to unauthorized account access
- Increases risk of account takeover and data breaches
- Weak credentials reduce overall system security

## Fix Implementation
- Added password validation rules in the User model
- Implemented regex-based validation before hashing the password
- Enforced:
  - Minimum length (8 characters)
  - At least one uppercase letter
  - At least one lowercase letter
  - At least one number
  - At least one special character

**Code Fix Summary:**
- Used Sequelize validation (`validate` property)
- Applied a regex pattern to enforce strong password rules
- Ensured only strong passwords are accepted before storing in the database

## Testing
| Test Case | Input | Result |
|---|---|---|
| Weak Password | `12345` | ❌ Rejected |
| Weak Password | `password` | ❌ Rejected |
| Strong Password | `Mahrukh@123` | ✅ Accepted |

## Result
- Weak passwords are no longer accepted
- Application security improved against brute-force and credential attacks
- User authentication is now more secure

## Screenshots
| # | File | Description |
|---|---|---|
| 1 | `01-weak-password-registration.png` | Registration flow used to test password rules |
| 2 | `02-burpsuite-registration-request.png` | Burp Suite capturing the registration request |
| 3 | `03-password-validation-code.png` | Regex-based password validation code (User model) |
| 4 | `04-strong-password-accepted.png` | Password strength checklist — strong password accepted |
