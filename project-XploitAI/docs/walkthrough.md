# XploitAI — Code Walkthrough

| Field | Details |
|-------|---------|
| **Document Type** | Architecture & Code Walkthrough |
| **Project** | XploitAI |
| **Author** | Donal Jojo |
| **Last Updated** | 2026-05-01 |
| **Phase** | Phase 1 — Control Plane & Simulation |

---

## Core Philosophy

> **AI decides. Policies validate. Executors act. Visualization explains.**

XploitAI deliberately separates every concern into its own layer.
The AI never executes commands directly — it only produces decisions.
The executor never decides anything — it only runs what it's told.
The policy engine sits between them and enforces the rules.

---

## System Architecture

```
User (Dashboard)
       │
       ▼
  Orchestration Core (services/)
       │
       ▼
  AI Agent  ──────────────────────────────────────►  LLM API
  (ai/decision_engine.py)                          (Gemini / NVIDIA / Groq)
       │
       ▼
  Policy Engine (policy/engine.py)
       │  Validates every action before execution
       ▼
  Action Registry (actions/predefined.py)
       │  Only whitelisted actions can proceed
       ▼
  Executor (executor/)
       │  Simulation: fake outputs
       │  SSH: real commands on Kali VM (Phase 2)
       ▼
  Attack State (state/state_manager.py)
       │  Tracks phase, findings, history
       ▼
  Dashboard (dashboard/)
       │  Visualizes everything
       ▼
  User sees results
```

---

## Module by Module

### `dashboard/` — The UI Layer

The entry point for all user interaction.

**Key files:**
- `views.py` — all main view functions (dashboard, attack detail, approve, stop, etc.)
- `auth.py` — registration, activation, login, logout, password reset
- `api.py` — JSON API endpoints (attack chat ask/reset)
- `urls.py` — all URL route definitions
- `templates/dashboard/` — HTML templates

**Auth flow:**
1. User registers → account created as inactive
2. Activation email sent (console in dev) → user clicks link → account activated
3. User logs in → Django session created → redirected to dashboard
4. Admin group checked on `/configuration/` access

---

### `core/` — Django Models

Defines the data structures the whole system revolves around.

**Key model: `AttackState`**
```
AttackState
├── name                  # Run label
├── current_phase         # reconnaissance / exploitation / etc.
├── autonomy_status       # RUNNING / STOPPED / WAITING
├── stop_reason           # Why it stopped
├── state_data (JSON)     # findings, target, level_history, report_artifacts
└── current_plan (JSON)   # AI-generated plan steps for current phase
```

Everything — AI decisions, executor results, phase reviews — flows in and out of `AttackState`.

---

### `ai/` — The AI Brain

The most complex module. Handles all LLM interaction.

**Key files:**

| File | Role |
|------|------|
| `decision_engine.py` | Core reasoning — decides what action to take next |
| `planner.py` | Generates multi-step attack plans per phase |
| `command_generator.py` | Turns AI decisions into concrete commands |
| `autonomy.py` | Controls the autonomous attack loop |
| `output_analysis.py` | Parses executor output back into findings |
| `context_manager.py` | Manages what context is sent to the LLM |
| `safety.py` | Safety checks before any LLM call |
| `memory.py` | Stores findings across phases |
| `schemas.py` | Pydantic schemas for AI inputs/outputs |

**LLM Adapters (`ai/llm/`):**
XploitAI supports multiple LLM providers via adapter classes:
- `anthropic.py` — Claude
- `gemini.py` — Google Gemini
- `groq_adapter.py` — Groq (Llama, Mixtral)
- `nvidia_adapter.py` — NVIDIA NIM
- `ollama_adapter.py` — Local Ollama models
- `openai_adapter.py` — OpenAI
- `lmstudio_adapter.py` — LM Studio (local)
- `fallback.py` — Tries providers in order if primary fails
- `task_router.py` — Routes tasks to best provider based on capability

---

### `policy/` — The Safety Gate

Every action the AI proposes must pass through the policy engine before execution.

**`engine.py`** — validates:
- Is this action in the allowed action registry?
- Is the current phase correct for this action?
- Does this violate any kill-chain rules?

**`approval.py`** — handles human-in-the-loop approval flow:
- Some phases require explicit user approval before proceeding
- The attack pauses at `WAITING_FOR_APPROVAL` until the user clicks Approve

---

### `actions/` — The Attack Grammar

Defines all atomic attack actions XploitAI knows about.

**`predefined.py`** — the full list of allowed actions (e.g. `HTTPHeaderFetch`, `PortScan`, etc.)  
**`action_graph.json`** — defines which actions can follow which  
**`command_map.json`** — maps action names to actual commands (used by executor)

The AI can only request actions that exist in this registry. It cannot invent new ones.

---

### `executor/` — Runs the Commands

Takes validated actions and executes them.

**Phase 1 — Simulation (`simulator.py`):**
- Generates realistic fake outputs for every action
- No real network calls, no real tools
- Deterministic or probabilistic outcomes

**Phase 2 — SSH (`ssh_executor.py`):**
- Connects to Kali Linux VM via SSH (Paramiko)
- Runs real tools (nmap, curl, etc.)
- Returns real output

**Other files:**
- `daemon.py` — background process that polls for tasks and runs them
- `api_views.py` — internal API the daemon uses to communicate
- `contract.py` — defines the interface between orchestration and executor

---

### `services/` — Orchestration Logic

Ties the AI, policy, executor and state together into a working loop.

**`execution_service.py`** — the main attack loop:
1. Ask AI for next action
2. Validate with policy engine
3. Send to executor
4. Parse output
5. Update attack state
6. Repeat until phase complete

**`quick_test_service.py`** — same but abbreviated for quick tests  
**`remote_execution_service.py`** — same for SSH (Phase 2)  
**`reporting_service.py`** — generates PDF/JSON reports from attack state

---

### `parser/` — Output Parsing

**`output_parser.py`** — takes raw executor output (text) and converts it to structured findings that the AI and state manager can understand.

---

### `state/` — Attack State Manager

**`state_manager.py`** — single source of truth for where an attack is.
Handles transitions between phases, records findings, manages history.

---

## Key Flows

### Starting an Attack
```
User fills /start/ form
    → execution_service creates AttackState
    → planner generates Phase 1 plan (reconnaissance)
    → attack pauses at WAITING_FOR_APPROVAL
    → User clicks Approve
    → executor runs actions one by one
    → output_parser processes each result
    → decision_engine decides next action
    → state_manager records everything
    → phase completes → moves to next phase
    → repeat until all phases done
```

### Human-in-the-Loop Approval
```
AI generates plan
    → autonomy_status = WAITING_FOR_APPROVAL
    → Dashboard shows "Approve Plan" button
    → User reviews steps
    → User clicks Approve → POST /attack/<pk>/approve/
    → autonomy_status = RUNNING
    → executor daemon picks up and starts executing
```

### AI Chat
```
User types question in chat widget
    → POST /attack-chat/ask/ with JSON body
    → DashboardChatService sends question + attack context to LLM
    → LLM responds
    → Response returned as JSON { "reply": "..." }
    → UI displays reply
```

---

## What Phase 1 Does NOT Include

- ❌ Real Kali Linux or Metasploit
- ❌ Real network scanning
- ❌ SSH connections to VMs
- ❌ Any actual exploitation
- ✅ All of the above is simulated safely