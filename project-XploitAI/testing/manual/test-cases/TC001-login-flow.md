# TC001 — Login Flow

**Type:** Manual UI  
**Priority:** High  
**Precondition:** App running at localhost:8000, valid user exists

## Steps
| # | Action | Expected Result |
|---|--------|-----------------|
| 1 | Go to /login | Login page loads |
| 2 | Enter valid email + password | Fields accept input |
| 3 | Click "Sign In" | Redirects to /dashboard |
| 4 | Enter wrong password | Error message appears |

## Pass/Fail
- [✅] Pass  
- [ ] Fail  

## Notes / Screenshots
<!-- Add screenshot path or observations here -->
Valid credentials:
QA_DOCS\project-XploitAI\testing\manual\screenshots\tc001_valid_cred.png
QA_DOCS\project-XploitAI\testing\manual\screenshots\tc001_sucess_login.png
Wrong credentials:
QA_DOCS\project-XploitAI\testing\manual\screenshots\tc001_wrong_cred.png
QA_DOCS\project-XploitAI\testing\manual\screenshots\tc001_error_login.png