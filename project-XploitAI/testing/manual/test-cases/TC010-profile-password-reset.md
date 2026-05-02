# TC010 - Profile Password Change and Password Reset

| Field | Details |
|-------|---------|
| **Test Case ID** | TC010 |
| **Test Case Name** | Profile Password Change and Password Reset |
| **Module** | Authentication |
| **Type** | Manual UI |
| **Priority** | High |
| **Severity if Failed** | Critical |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-02 |
| **Last Updated** | 2026-05-02 |

---

## Objective

Verify that logged-in users can change their password from profile and that the forgot-password reset flow works using the local console reset code.

---

## Preconditions

- App running at `http://localhost:8000`
- Test user is active and can log in
- Django console email backend is enabled for local development
- Tester has access to terminal output for reset code

---

## Test Data

| Field | Value |
|-------|-------|
| Test User Email | `qa_user_010@example.com` or existing test email |
| Current Password | `[current test password]` |
| New Password | `[new valid test password]` |
| Invalid Confirm Password | Different from new password |

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Log in as active test user | Dashboard is displayed. |
| 2 | Navigate to `/profile/` | Profile page loads. |
| 3 | Submit password change with current password and matching new passwords | Success message is shown or user is redirected as designed. |
| 4 | Log out | Session ends. |
| 5 | Log in with old password | Login fails. |
| 6 | Log in with new password | Login succeeds. |
| 7 | Navigate to `/password-reset/` while logged out | Password reset request page loads. |
| 8 | Submit the test user's email | Reset code/link is printed to console and UI shows generic success. |
| 9 | Open `/password-reset/verify/<uidb64>/` from the reset flow | Verification page loads. |
| 10 | Submit valid reset code with matching password fields | Password is reset successfully. |
| 11 | Attempt reset with mismatched password confirmation | Validation error is shown and password is not changed. |
| 12 | Attempt reset with invalid code | Error is shown and password is not changed. |

---

## UI Checks

- [ ] Profile page renders correctly
- [ ] Password fields are masked
- [ ] Success and error messages are clear
- [ ] Reset request response does not reveal whether the email belongs to a valid account, if designed for privacy
- [ ] Verification form validates required fields

---

## Security Checks

- [ ] Password change requires login
- [ ] Old password stops working after successful change
- [ ] Reset code is required before password reset
- [ ] Invalid reset code is rejected
- [ ] Passwords are never displayed in plain text
- [ ] No stack trace appears for invalid reset attempts

---

## Screenshots to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| Profile page | `tc010_profile_page.png` |
| Password change success | `tc010_password_change_success.png` |
| Old password login rejected | `tc010_old_password_rejected.png` |
| Password reset request page | `tc010_reset_request_page.png` |
| Reset verification page | `tc010_reset_verify_page.png` |
| Invalid reset code error | `tc010_invalid_code_error.png` |

---

## Pass Criteria

- Logged-in user can change password with valid input
- Old password fails after successful change
- New password works after change/reset
- Reset code flow accepts valid code and rejects invalid code
- Password validation errors are clear

## Fail Criteria

- Password change works without login
- Old password still works after successful change
- Reset accepts invalid code
- Mismatched password confirmation is accepted
- Error responses expose internal details

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | |
| **Execution Date** | |
| **Status** | Pass / Fail / Blocked |
| **Result File** | `results/TC010-profile-password-reset-result.md` |
| **Defects Found** | |
| **Notes** | |
