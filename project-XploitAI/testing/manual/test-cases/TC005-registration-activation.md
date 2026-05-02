# TC005 - Registration and Account Activation

| Field | Details |
|-------|---------|
| **Test Case ID** | TC005 |
| **Test Case Name** | Registration and Account Activation |
| **Module** | Authentication |
| **Type** | Manual UI |
| **Priority** | High |
| **Severity if Failed** | Critical |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-02 |
| **Last Updated** | 2026-05-02 |

---

## Objective

Verify that a new user can register, remains inactive until activation, and can log in only after using the activation link.

---

## Preconditions

- App running at `http://localhost:8000`
- Django console email backend is enabled for local development
- Browser is in a fresh session
- Test username/email are not already registered

---

## Test Data

| Field | Value |
|-------|-------|
| Username | `qa_user_005` |
| Email | `qa_user_005@example.com` |
| Password | `[use a valid test password]` |
| Password Confirm | Same as password |

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Navigate to `/register/` | Registration page loads with username, email, password, and confirm password fields. |
| 2 | Submit valid registration data | User is created inactive and redirected to `/login/` or shown a success message. |
| 3 | Attempt login before activation | Login is rejected. User is not allowed into dashboard. |
| 4 | Check server console for activation email output | Activation link is printed and contains `/activate/<uidb64>/<token>/`. |
| 5 | Open the activation link in browser | Account is activated and user is redirected to `/login/` or shown activation success. |
| 6 | Log in with the new account | Login succeeds and dashboard is displayed. |
| 7 | Attempt to register with the same username/email again | Duplicate validation error is shown. |
| 8 | Attempt registration with mismatched passwords | Validation error is shown and account is not created. |

---

## UI Checks

- [ ] Registration form renders correctly
- [ ] Password fields mask typed values
- [ ] Success and validation messages are visible
- [ ] Activation success/failure messages are understandable
- [ ] Duplicate user errors are clear but do not expose sensitive account details

---

## Security Checks

- [ ] User cannot log in before activation
- [ ] Passwords are not displayed in plain text
- [ ] Activation token cannot be reused after successful activation, if applicable
- [ ] Invalid activation tokens do not activate accounts
- [ ] No stack trace appears for invalid token or duplicate registration

---

## Screenshots to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| Registration form | `tc005_registration_form.png` |
| Registration success/redirect | `tc005_registration_success.png` |
| Login blocked before activation | `tc005_login_before_activation_blocked.png` |
| Activation success page | `tc005_activation_success.png` |
| Duplicate user validation | `tc005_duplicate_validation.png` |

---

## Pass Criteria

- New account is created inactive
- Login is blocked until activation
- Activation link activates the account
- Activated account can log in
- Duplicate and mismatched-password validations work

## Fail Criteria

- New user can log in before activation
- Activation link fails for a valid newly registered user
- Duplicate registration creates another account
- Password validation fails to catch mismatch
- Error pages expose internal details

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | |
| **Execution Date** | |
| **Status** | Pass / Fail / Blocked |
| **Result File** | `results/TC005-registration-activation-result.md` |
| **Defects Found** | |
| **Notes** | |
