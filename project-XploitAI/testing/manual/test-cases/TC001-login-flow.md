# TC001 — Login Flow

| Field | Details |
|-------|---------|
| **Test Case ID** | TC001 |
| **Test Case Name** | Login Flow Validation |
| **Module** | Authentication |
| **Type** | Manual UI |
| **Priority** | High |
| **Severity if Failed** | Critical |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-01 |
| **Last Updated** | 2026-05-01 |

---

## Objective

Verify that the login page accepts valid credentials, rejects invalid ones,
and correctly redirects authenticated users to the dashboard.

---

## Preconditions

- App running at `http://localhost:8000`
- Database is active and seeded
- A valid test user exists in the database
- Browser cache cleared — fresh session (no existing login)
- No active logged-in session before test begins

---

## Test Data

| Field | Valid Value | Invalid Value |
|-------|-------------|---------------|
| Email / Username | `test@example.com` | `fake@example.com` |
| Password | `[use your test password]` | `wrongpassword123` |

> ⚠️ Never write real passwords in test case files. Use masked values or reference a secure vault.

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Navigate to `http://localhost:8000/login` | Login page loads. Email and password fields visible. Sign In button present. |
| 2 | Enter **valid** email and password | Fields accept input. Password is masked (dots/asterisks). |
| 3 | Click **Sign In** | User is redirected to `/dashboard` or `/`. Dashboard content is visible. |
| 4 | Log out and return to `/login` | Login page is shown again. No session persists. |
| 5 | Enter **valid email** but **wrong password** | Error message shown. User stays on login page. No redirect. |
| 6 | Enter **non-existent email** and any password | Error message shown. No account details leaked in error. |
| 7 | Leave both fields blank and click Sign In | Validation error shown for both fields. Form does not submit. |
| 8 | Attempt to access `/` directly while logged out | Redirected to `/login?next=/`. Not shown dashboard. |

---

## UI Checks (observe during steps above)

- [ ] Login page layout renders without broken elements
- [ ] Email field is functional and accepts text
- [ ] Password field masks characters while typing
- [ ] Sign In button is clickable and triggers submission
- [ ] Error messages are clearly visible and user-friendly
- [ ] No sensitive data (stack trace, DB errors) shown on failure
- [ ] Redirect after login goes to the correct page

---

## Security Checks (observe during steps above)

- [ ] Password is never shown in plain text
- [ ] Error message does not reveal whether email exists or not
- [ ] No redirect to unintended pages after login
- [ ] Session is created only after successful login

---

## Screenshots to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| Login page before any input | `tc001_login_page.png` |
| Valid credentials entered (password masked) | `tc001_valid_cred.png` |
| Dashboard after successful login | `tc001_success_login.png` |
| Wrong password entered | `tc001_wrong_cred.png` |
| Error message displayed | `tc001_error_msg.png` |
| Blank form validation error | `tc001_blank_validation.png` |

---

## Pass Criteria

All of the following must be true for this test to PASS:

- Valid credentials → redirect to dashboard ✓
- Wrong password → error shown, no redirect ✓
- Blank form → validation error shown ✓
- Logged-out user → cannot access dashboard directly ✓
- Password always masked ✓

## Fail Criteria

Test FAILS if any of the following occur:

- Valid credentials do not redirect to dashboard
- Wrong password still logs the user in
- Any page shows a Django stack trace or database error
- Password is visible in plain text at any point
- Blank form submits without validation

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | |
| **Execution Date** | |
| **Status** | ⬜ Pass / ⬜ Fail / ⬜ Blocked |
| **Result File** | `results/TC001-login-flow-result.md` |
| **Defects Found** | |
| **Notes** | |