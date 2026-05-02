# XploitAI — API & URL Reference

| Field | Details |
|-------|---------|
| **Document Type** | API & Route Reference |
| **Project** | XploitAI |
| **Author** | Donal Jojo |
| **Last Updated** | 2026-05-01 |
| **Base URL** | `http://localhost:8000` |

---

## Overview

XploitAI is a Django application. Most routes return HTML pages (UI views).
A small number of routes return JSON and act as API endpoints — these are marked **[JSON API]**.

---

## Authentication Routes

### `GET /register/`
Registration page — displays the sign-up form.

### `POST /register/`
Submit registration form.

**Form fields:**
| Field | Type | Required |
|-------|------|----------|
| `username` | string | ✅ |
| `email` | string (email) | ✅ |
| `password` | string | ✅ |
| `password_confirm` | string | ✅ |

**On success:** Redirects to `/login/`. User is created but inactive (needs email activation).  
**On failure:** Returns form with validation errors (mismatched passwords, duplicate username, etc.)

---

### `GET /activate/<uidb64>/<token>/`
Email activation link. Activates the user account.

**On success:** Redirects to `/login/`  
**On failure:** Shows invalid/expired token message

---

### `GET /login/`
Login page — displays the login form.

### `POST /login/`
Submit login credentials.

**Form fields:**
| Field | Type |
|-------|------|
| `username` | string |
| `password` | string |

**On success:** Redirects to `/` (dashboard)  
**On failure:** Returns form with error message. No account detail leaked.

---

### `POST /logout/`
Logs out the current user. Redirects to `/login/`.

---

### `GET /profile/`
User profile page. Login required.

### `POST /profile/change-password/`
Change password form submission. Login required.

---

### `GET /password-reset/`
Password reset request page.

### `POST /password-reset/`
Submit email for password reset. Sends reset code via email (or console in dev).

---

### `GET /password-reset/verify/<uidb64>/`
Password reset verification page.

### `POST /password-reset/verify/<uidb64>/`
Submit reset code + new password.

**Form fields:**
| Field | Type |
|-------|------|
| `code` | string |
| `new_password` | string |
| `confirm_password` | string |

---

## Dashboard Routes

### `GET /`
Main dashboard. Shows active attack state, recent activity, autonomy status.  
**Access:** Login required.

---

### `GET /history/`
List of all past attack runs.  
**Access:** Login required.

### `POST /history/delete-all/`
Delete all attack history.  
**Access:** Login required.

---

### `GET /start/`
New attack start form.

### `POST /start/`
Submit and launch a new attack simulation.

**Form fields:**
| Field | Description |
|-------|-------------|
| `target_url` | Target to simulate attack against |
| `executor_type` | `simulation` or `ssh` |
| `autonomy_mode` | Level of AI autonomy |

**On success:** Redirects to `/attack/<pk>/`

---

### `GET /quick-test/start/`
Quick test start form (abbreviated lifecycle).

### `POST /quick-test/start/`
Launch a quick test run.

---

### `GET /attack/<int:pk>/`
Attack detail page for a specific run. Shows phase, plan steps, execution status.

### `GET /attack/<int:pk>/phases/<str:phase_key>/`
Detail view for a specific phase of an attack run.

### `POST /attack/<int:pk>/phases/<str:phase_key>/regenerate/`
Regenerate the AI plan for a specific phase.

### `GET /attack/<int:pk>/replay/`
Replay view — step through attack history.

### `GET /attack/<int:pk>/plan/`
View the current AI-generated plan.

### `GET /attack/<int:pk>/phase-reviews/`
View phase review summaries.

### `GET /attack/<int:pk>/command-logs/`
View raw command execution logs.

### `POST /attack/<int:pk>/delete/`
Delete a specific attack run.

---

## Attack Control Routes

### `POST /attack/<int:pk>/approve/`
Approve the AI-generated plan and allow execution to proceed.  
**Access:** Login required.

### `POST /attack/<int:pk>/resume/`
Resume a paused or stopped attack.  
**Access:** Login required.

### `POST /attack/<int:pk>/stop/`
Stop a currently running attack.  
**Access:** Login required.

### `POST /attack/<int:pk>/retry-phase/`
Retry a failed phase.  
**Access:** Login required.

---

## Report Routes

### `POST /attack/<int:pk>/report/generate`
Generate a report for an attack run.

### `GET /attack/<int:pk>/report/latest`
Get the latest generated report.  
**Returns:** JSON

**Response example:**
```json
{
  "status": "generated",
  "generated_at": 1234567890,
  "payload": {
    "executive_summary": "..."
  }
}
```

---

## JSON API Routes

### `POST /attack-chat/ask/` **[JSON API]**
Ask the AI assistant a question about the current attack state.  
**Access:** Login required.

**Request:**
```json
{ "message": "What did the recon phase find?" }
```

**Response:**
```json
{ "reply": "During reconnaissance, the AI detected open ports on..." }
```

**Headers required:**
```
Content-Type: application/json
X-CSRFToken: <csrf_token_from_cookie>
```

---

### `POST /attack-chat/reset/` **[JSON API]**
Reset the AI chat context for the current session.  
**Access:** Login required.

**Response:** `200 OK`

---

### `GET /check_status/` **[JSON API]**
Check connectivity status of configured LLM provider.  
**Access:** Login required.

**Response example:**
```json
{ "status": "ok", "provider": "gemini" }
```

---

## Configuration Route

### `GET /configuration/`
LLM configuration page — set API keys, model selection, provider settings.  
**Access:** Admin group only. Regular users are redirected.

---

## Target & Executor Management

### `GET /targets/`
Target management page — list, add, edit, delete attack targets.

### `GET /executors/`
Executor management page — manage simulation and SSH executors.

---

## Executor Internal API

### `/api/executor/...`
Internal API used by the executor daemon process for heartbeats and task polling.
Not intended for direct user interaction.

See `executor/urls.py` for full route definitions.

---

## Settings Redirect

### `GET /settings/`
Permanently redirects to `/configuration/`.

---

## Admin Panel

### `/admin/`
Django admin interface. Full model management.  
**Access:** Superuser only.

---

## HTTP Status Codes Used

| Code | Meaning in XploitAI |
|------|---------------------|
| `200` | Page or API response OK |
| `302` | Redirect (after form submit, login, logout) |
| `403` / redirect | Access denied (wrong group or not logged in) |
| `404` | Attack run or resource not found |
| `422` | Validation error (form or API) |
| `500` | Server error — should never appear in normal use |