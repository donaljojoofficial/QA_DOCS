# XploitAI — Setup Guide

| Field | Details |
|-------|---------|
| **Document Type** | Setup & Installation Guide |
| **Project** | XploitAI |
| **Author** | Donal Jojo |
| **Last Updated** | 2026-05-01 |
| **Phase** | Phase 1 — Control Plane & Simulation |

---

## Overview

XploitAI is an AI-orchestrated penetration testing simulation framework built on Django.
Phase 1 runs entirely in simulation — no real VMs, no real exploits, no network scanning.
This guide covers setting up the project locally on Windows for development and testing.

---

## System Requirements

| Requirement | Minimum |
|-------------|---------|
| OS | Windows 10 / 11 |
| Python | 3.12+ |
| RAM | 8 GB (16 GB recommended) |
| Disk | 2 GB free |
| Browser | Chrome / Firefox (for dashboard testing) |

---

## 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/XploitAI.git
cd XploitAI
```

Or if you downloaded the zip, extract to:
```
E:\QA\XploitAI-main\
```

---

## 2. Create a Virtual Environment

```bash
# Inside the XploitAI-main folder
python -m venv venv

# Activate (Windows)
venv\Scripts\activate
```

You should see `(venv)` in your terminal prompt.

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

Key packages installed:

| Package | Purpose |
|---------|---------|
| `Django >= 5.0` | Web framework |
| `pydantic >= 2.5` | Data validation |
| `structlog` | Structured logging |
| `python-dotenv` | Environment variable loading |
| `google-generativeai` | Gemini LLM integration |
| `groq` | Groq LLM integration |
| `paramiko` | SSH executor (Phase 2 only) |

---

## 4. Configure Environment Variables

Copy the example file and fill in your values:

```bash
copy .env.example .env
```

Open `.env` and configure:

```env
# Required — Django core
DJANGO_SECRET_KEY=any-long-random-string-here
DJANGO_DEBUG=true

# Required — at least one LLM provider
GEMINI_API_KEY=your_gemini_api_key_here

# Optional — NVIDIA output analysis
NVIDIA_API_KEY=your_nvidia_api_key_here
NVIDIA_MODEL=mistralai/mistral-small-4-119b-2603
OUTPUT_ANALYSIS_PROVIDER=nvidia
OUTPUT_ANALYSIS_MODEL=nvidia/nemotron-3-super-120b-a12b
```

> ⚠️ Never commit `.env` to Git. It is already listed in `.gitignore`.

**Where to get API keys:**
- Gemini → https://aistudio.google.com/app/apikey
- NVIDIA → https://build.nvidia.com

---

## 5. Run Database Migrations

```bash
python manage.py migrate
```

Creates `db.sqlite3` with all required tables.

---

## 6. Create a Superuser (Admin Account)

```bash
python manage.py createsuperuser
```

Then grant yourself Admin group access:
1. Go to `http://localhost:8000/admin/`
2. Login with your superuser credentials
3. Go to **Auth → Users → your user → Groups**
4. Add to `Admin` group → Save

---

## 7. Start the Server

```bash
python manage.py runserver
```

App runs at: **http://localhost:8000**

---

## 8. Key URLs

| URL | Description | Access |
|-----|-------------|--------|
| `/` | Main dashboard | Login required |
| `/register/` | Create new account | Public |
| `/login/` | Login | Public |
| `/profile/` | User profile | Login required |
| `/configuration/` | LLM API config | Admin only |
| `/targets/` | Target management | Login required |
| `/executors/` | Executor management | Login required |
| `/history/` | Past attack runs | Login required |
| `/start/` | Start new attack | Login required |
| `/quick-test/start/` | Quick test mode | Login required |
| `/admin/` | Django admin panel | Superuser only |

---

## 9. Email Activation (Local Dev)

Django prints activation emails to the terminal console by default.

After registering, check your terminal for output like:
```
Subject: Activate your XploitAI account

http://localhost:8000/activate/Mg/abc123token/
```

Copy that URL into your browser to activate the account.

---

## 10. Simulation Mode (Phase 1)

When starting an attack, select **Simulation** as the executor.
Use any target URL, e.g. `http://127.0.0.1:4280/`.

The simulator generates realistic fake command outputs so the AI can make decisions without real VMs. Safe to run entirely on your local machine.

---

## 11. Project Folder Structure

```
XploitAI-main/
├── agent/           # AI decision logic
├── ai/              # LLM adapters (Gemini, NVIDIA, Groq, Ollama etc.)
├── actions/         # Atomic attack action definitions
├── core/            # Django models (AttackState, Action, Result)
├── dashboard/       # UI views, templates, auth
├── executor/        # Simulation + SSH executors
├── parser/          # Output parser (raw → structured)
├── policy/          # Policy engine (kill-chain enforcement)
├── services/        # Orchestration services
├── state/           # Attack state manager
├── xploitai/        # Django project settings + URLs
├── manage.py
├── requirements.txt
└── .env.example
```

---

## 12. Common Issues & Fixes

| Issue | Fix |
|-------|-----|
| `ModuleNotFoundError` | Run `pip install -r requirements.txt` with venv active |
| `OperationalError` on startup | Run `python manage.py migrate` |
| Login fails for valid user | User may not be activated — check terminal for activation link |
| `/configuration/` redirects away | User not in `Admin` group — add via `/admin/` |
| AI plan never generates | Check your LLM API key is set correctly in `.env` |
| `DJANGO_SECRET_KEY` error | Set any value for this key in `.env` |