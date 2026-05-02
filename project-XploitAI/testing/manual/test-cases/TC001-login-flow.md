Test Execution Report — TC001 Login Flow

Test Case ID: TC001
Test Case Name: Login Flow Validation
Module: Authentication
Type: Manual UI Testing
Priority: High
Severity if Failed: Critical
Environment: Local Development
Application URL: http://localhost:8000/login
Browser:  Chrome 
OS:  Windows  11 
Tester: Donal Jojo
Execution Date: 1-may-2026
Status: ✅ Pass

Objective

Verify that the login functionality accepts valid credentials, rejects invalid credentials, and properly redirects authenticated users to the dashboard.

Preconditions
Application server is running locally on port 8000.
Database connection active.
Login module enabled.
Test user exists in database.
Browser cache cleared / fresh session.
No active logged-in session.
Test Data
Field	Value
Valid Email	test@example.com
Valid Password	********
Invalid Password	wrongpassword123

(Use masked passwords in reports—never expose real passwords.)

Execution Steps & Results
Step	Action	Expected Result	Actual Result	Status
1	Navigate to /login	Login page loads successfully	Login page loaded correctly with email/password fields visible	✅ Pass
2	Enter valid email + password	Fields accept input	Inputs accepted normally	✅ Pass
3	Click Sign In	Redirect to /dashboard	Successfully redirected to dashboard page	✅ Pass
4	Enter wrong password	Error message shown	Validation error displayed correctly; login blocked	✅ Pass
UI Validation Checks

Verified:

✅ Login page layout loads properly
✅ Email textbox functional
✅ Password textbox masks characters
✅ Sign In button clickable
✅ Proper redirect after authentication
✅ Error message visible on invalid login
✅ Session created after successful login
✅ Unauthorized access prevented with wrong password

Security Validation

Checked:

✅ Password hidden during typing
✅ Invalid credentials rejected
✅ No sensitive data shown in error message
✅ Authentication required for dashboard access
✅ Session established securely after login


Evidence / Screenshots
Valid Credentials Entry

QA_DOCS/project-XploitAI/testing/manual/screenshots/tc001_valid_cred.png

Successful Login

QA_DOCS/project-XploitAI/testing/manual/screenshots/tc001_success_login.png

Wrong Credentials Entry

QA_DOCS/project-XploitAI/testing/manual/screenshots/tc001_wrong_cred.png

Error Message

QA_DOCS/project-XploitAI/testing/manual/screenshots/tc001_error_login.png

Observations
Login flow works as expected.
No UI alignment issues observed.
Redirect behavior correct.
Error handling clear and understandable.
No crash / blank page / unexpected response.
Defects Found

None

Final Result

PASS ✅

Authentication module behaves according to expected functional requirements.