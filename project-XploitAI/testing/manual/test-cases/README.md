# XploitAI Manual Test Cases

| TC ID | Test Case Name | Module | Priority |
|-------|----------------|--------|----------|
| TC001 | Login Flow Validation | Authentication | High |
| TC002 | Start Attack Simulation | Attack Orchestration | High |
| TC003 | Access Control and Roles | Authentication / Authorization | High |
| TC004 | Target Management | Target Management | Medium |
| TC005 | Registration and Account Activation | Authentication | High |
| TC006 | Attack Approval and Control Actions | Attack Control | High |
| TC007 | Attack History and Delete | Attack History | Medium |
| TC008 | Report Generation and Latest Report | Reporting | Medium |
| TC009 | Attack Chat API | AI Chat | Medium |
| TC010 | Profile Password Change and Password Reset | Authentication | High |

## Suggested Execution Order

1. TC005 - Registration and Account Activation
2. TC001 - Login Flow Validation
3. TC003 - Access Control and Roles
4. TC002 - Start Attack Simulation
5. TC006 - Attack Approval and Control Actions
6. TC008 - Report Generation and Latest Report
7. TC009 - Attack Chat API
8. TC007 - Attack History and Delete
9. TC004 - Target Management
10. TC010 - Profile Password Change and Password Reset

## Notes

- Keep real passwords and API keys out of test case and result files.
- Use Phase 1 simulation mode only unless a later test cycle explicitly covers SSH execution.
- Capture screenshots under `testing/manual/screenshots/` using each test case's suggested filenames.
