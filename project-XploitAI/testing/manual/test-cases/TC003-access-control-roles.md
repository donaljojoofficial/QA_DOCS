# TC003 - Access Control and Roles

| Field | Details |
|-------|---------|
| **Test Case ID** | TC003 |
| **Test Case Name** | Access Control and Roles |
| **Module** | Authentication / Authorization |
| **Type** | Manual UI |
| **Priority** | High |
| **Severity if Failed** | Critical |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-02 |
| **Last Updated** | 2026-05-02 |

---

## Objective

Verify that protected pages require login and that admin-only configuration access is restricted to users in the Admin group.

---

## Preconditions

- App running at `http://localhost:8000`
- One activated regular user exists
- One activated Admin-group user exists
- Browser can be tested in a fresh/incognito session

---

## Test Data

| User Type | Expected Access |
|-----------|-----------------|
| Logged-out visitor | Public auth pages only |
| Regular user | Dashboard, profile, targets, executors, history, start attack |
| Admin user | All regular user pages plus `/configuration/` |

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | In a logged-out session, navigate to `/` | Redirected to `/login?next=/` or login page. Dashboard is not visible. |
| 2 | Logged out, navigate to `/profile/` | Redirected to login page. |
| 3 | Logged out, navigate to `/start/` | Redirected to login page. |
| 4 | Log in as regular user | Redirected to dashboard. |
| 5 | As regular user, navigate to `/configuration/` | Access is denied or redirected. Configuration secrets page is not visible. |
| 6 | As regular user, navigate to `/targets/`, `/executors/`, and `/history/` | Pages load successfully. |
| 7 | Log out regular user | Session is cleared and login page is shown. |
| 8 | Log in as Admin-group user | Redirected to dashboard. |
| 9 | As Admin user, navigate to `/configuration/` | Configuration page loads successfully. |
| 10 | Navigate to `/settings/` as Admin user | Permanently redirects to `/configuration/`. |

---

## UI Checks

- [ ] Login redirects include a sensible `next` parameter where applicable
- [ ] Access denied or redirect behavior is clear to the user
- [ ] Regular pages render after login without broken navigation
- [ ] Configuration page is not visible to regular users

---

## Security Checks

- [ ] Unauthenticated users cannot view protected content
- [ ] Regular users cannot access admin-only configuration
- [ ] Admin-only pages do not leak API keys to unauthorized users
- [ ] Logout invalidates access to protected pages
- [ ] Direct URL access follows the same authorization rules as navigation links

---

## Screenshots to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| Logged-out redirect from dashboard | `tc003_logged_out_dashboard_redirect.png` |
| Regular user denied configuration | `tc003_regular_user_config_denied.png` |
| Regular user targets page | `tc003_regular_user_targets.png` |
| Admin configuration page | `tc003_admin_configuration.png` |

---

## Pass Criteria

- Protected pages require login
- Regular users cannot access `/configuration/`
- Admin users can access `/configuration/`
- Logout clears protected page access

## Fail Criteria

- Logged-out users can view protected pages
- Regular users can view or modify configuration
- Admin users cannot access valid admin-only pages
- Authorization errors expose stack traces or secrets

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | |
| **Execution Date** | |
| **Status** | Pass / Fail / Blocked |
| **Result File** | `results/TC003-access-control-roles-result.md` |
| **Defects Found** | |
| **Notes** | |
