# TC009 - Attack Chat API

| Field | Details |
|-------|---------|
| **Test Case ID** | TC009 |
| **Test Case Name** | Attack Chat API |
| **Module** | AI Chat |
| **Type** | Manual API / UI |
| **Priority** | Medium |
| **Severity if Failed** | Major |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-02 |
| **Last Updated** | 2026-05-02 |

---

## Objective

Verify that authenticated users can ask attack-context questions through the chat API, reset chat context, and receive controlled errors for invalid requests.

---

## Preconditions

- App running at `http://localhost:8000`
- User is logged in
- At least one attack run exists in the current session/context
- CSRF token is available from browser cookies or page source
- LLM provider is configured, or expected provider-unavailable behavior is documented

---

## Test Data

| Request | Value |
|---------|-------|
| Ask Endpoint | `POST /attack-chat/ask/` |
| Reset Endpoint | `POST /attack-chat/reset/` |
| Valid Message | `What did the reconnaissance phase find?` |
| Empty Message | blank string |
| Content-Type | `application/json` |

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open an attack detail page with chat widget, if available | Chat input and send/reset controls are visible. |
| 2 | Send valid chat message through UI or `POST /attack-chat/ask/` | Response returns JSON with `reply`. UI displays the reply. |
| 3 | Ask a follow-up question | Response uses attack context where available. |
| 4 | Send `POST /attack-chat/reset/` | Request returns success and context is reset. |
| 5 | Send empty message to ask endpoint | Validation error is returned or UI prevents submission. |
| 6 | Send request without `Content-Type: application/json` | Request is rejected or handled with controlled validation. |
| 7 | Send request without CSRF token | Request is rejected with CSRF/forbidden response. |
| 8 | Log out and retry ask endpoint | Request redirects to login or returns unauthorized/forbidden response. |

---

## API Checks

- [ ] Valid ask request returns JSON
- [ ] JSON response contains `reply`
- [ ] Empty or malformed body does not create server error
- [ ] Reset endpoint returns success
- [ ] Provider-unavailable errors are user-friendly and controlled

---

## Security Checks

- [ ] Chat API requires authentication
- [ ] CSRF protection is enforced for POST requests
- [ ] Chat response does not reveal API keys or environment secrets
- [ ] Malformed JSON does not expose traceback
- [ ] User cannot access another user's private attack context

---

## Screenshots / Evidence to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| Chat widget before message | `tc009_chat_widget.png` |
| Chat response in UI | `tc009_chat_response_ui.png` |
| Valid JSON response | `tc009_valid_json_response.png` |
| Empty message validation | `tc009_empty_message_validation.png` |
| CSRF failure evidence | `tc009_csrf_failure.png` |

---

## Pass Criteria

- Valid authenticated chat request returns a useful JSON reply
- Reset endpoint succeeds
- Empty, malformed, unauthenticated, and CSRF-missing requests are handled safely
- No secrets or stack traces are exposed

## Fail Criteria

- Valid chat request fails unexpectedly
- API returns malformed JSON
- Invalid requests trigger server errors
- Logged-out users can use chat endpoint
- Chat leaks sensitive configuration

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | |
| **Execution Date** | |
| **Status** | Pass / Fail / Blocked |
| **Result File** | `results/TC009-attack-chat-api-result.md` |
| **Defects Found** | |
| **Notes** | |
