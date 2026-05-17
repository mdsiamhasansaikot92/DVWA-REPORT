CSRF VULNERABILITY ASSESSMENT REPORT

Target Application: DVWA (Damn Vulnerable Web Application)
Vulnerability Type: Cross-Site Request Forgery (CSRF)
Security Level:
Severity: High
Testing Environment: Educational Lab

INTRODUCTION

This report documents the exploitation of a Cross-Site Request Forgery (CSRF) vulnerability in DVWA at the Low security level.

CSRF is a web security vulnerability that allows an attacker to force an authenticated user to perform unwanted actions on a web application without the user’s knowledge or consent.

The assessment demonstrates how the absence of request validation mechanisms allows attackers to manipulate authenticated user actions.

OBJECTIVE

The objective of this assessment was to:

Identify CSRF vulnerabilities
Analyze insecure password change functionality
Demonstrate unauthorized state-changing requests
Evaluate missing request validation protections
TOOLS USED
Web Browser
DVWA
HTML Payloads
HTTP GET Requests
VULNERABILITY DESCRIPTION

The DVWA CSRF module at the Low security level allows password changes through HTTP GET requests without verifying the legitimacy of the request source.

The application does not implement:

CSRF tokens
Origin validation
Referrer validation
Proper HTTP method restrictions

Because of these missing protections, attackers can craft malicious requests that execute automatically when visited by authenticated users.

EXPLOITATION PROCESS

Step 1: Access the CSRF Module

The DVWA CSRF vulnerability page was opened from the vulnerabilities menu.

A password change form was presented.

Step 2: Test Password Change Functionality

A new password was entered:

Password:
abc123

Confirm Password:
abc123

The "Change" button was clicked.

OBSERVED REQUEST

After submission, the following URL was generated:

http://127.0.0.1/DVWA/vulnerabilities/csrf/?password_new=abc123&password_conf=abc123&Change=Change#

The password was successfully changed through a GET request.

IDENTIFIED SECURITY ISSUES

The following weaknesses were identified:

Sensitive actions performed using GET requests

The password change request was sent through URL parameters instead of an HTTP POST request.

No CSRF Token Protection

The application did not generate or validate CSRF tokens to verify request authenticity.

No Origin or Referrer Validation

The server did not verify where the request originated from.

Predictable Request Structure

The request parameters were simple and easily reproducible.

CSRF ATTACK DEMONSTRATION

A malicious attacker can create a crafted HTML payload that silently changes the victim’s password.

Example Payload:

<img src="http://127.0.0.1/DVWA/vulnerabilities/csrf/?password_new=hacked&password_conf=hacked&Change=Change#" />
ATTACK SCENARIO
The victim logs into DVWA.
The victim visits a malicious webpage controlled by the attacker.
The browser automatically loads the malicious image URL.
The request is sent using the victim’s active session cookies.
The victim’s password is changed without their knowledge.
ROOT CAUSE

The vulnerability exists because the application fails to validate whether requests originate from legitimate user actions.

The application trusts authenticated requests without verifying:

Request origin
Session intent
User interaction authenticity
SECURITY IMPACT

An attacker exploiting this vulnerability may be able to:

Change user passwords
Modify account settings
Trigger unauthorized actions
Hijack accounts
Abuse authenticated sessions

The impact is considered High because authenticated user actions can be manipulated remotely.

RECOMMENDATIONS

The following mitigations are recommended:

Implement CSRF Tokens

Generate unique random tokens for all state-changing requests.

Use HTTP POST Requests

Sensitive operations such as password changes should never use GET requests.

Validate Origin and Referrer Headers

Verify that requests originate from trusted application pages.

Implement SameSite Cookies

Restrict cross-site request behavior using secure cookie settings.

Require Re-Authentication

Require users to re-enter passwords before critical account changes.

Use Secure Framework Protections

Enable built-in CSRF protections provided by modern web frameworks.

CONCLUSION

The assessment confirmed that the DVWA application at the Low security level is vulnerable to Cross-Site Request Forgery attacks.

Because the application performs sensitive actions through unauthenticated GET requests without CSRF validation mechanisms, attackers can manipulate authenticated users into performing unintended actions.
