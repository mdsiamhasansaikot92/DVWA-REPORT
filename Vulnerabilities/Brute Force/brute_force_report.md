BRUTE FORCE VULNERABILITY ASSESSMENT REPORT

Target Application: DVWA (Damn Vulnerable Web Application)
Vulnerability Type: Brute Force Authentication Attack
Security Level:
Testing Tool: Burp Suite
Severity: High

INTRODUCTION

This report describes the exploitation of a brute force vulnerability in DVWA at the Low security level. The purpose of the assessment was to demonstrate how weak authentication controls allow attackers to automate credential guessing attacks and gain unauthorized access to the application.

A brute force attack is a method where multiple username and password combinations are repeatedly tested until valid credentials are discovered.

OBJECTIVE

The objective of this assessment was to:

Test the login page against brute force attacks
Identify weak or default credentials
Analyze the application's response behavior
Demonstrate the risks of missing authentication protections
TOOLS USED
Burp Suite
DVWA
Metasploit Wordlists

Wordlists Used:

Username Wordlist:
/usr/share/wordlists/metasploit/http_default_users.txt

Password Wordlist:
/usr/share/wordlists/metasploit/http_default_passwords.txt

VULNERABILITY DESCRIPTION

The DVWA application at the Low security level does not implement proper authentication security controls such as:

Rate limiting
Account lockout
CAPTCHA verification
Brute-force detection
Multi-factor authentication

Because of these missing protections, attackers can automate login attempts using common username and password combinations.

EXPLOITATION PROCESS

Step 1: Launch Burp Suite

Burp Suite was opened and configured as an intercepting proxy.

Procedure:

Open Burp Suite
Go to Proxy tab
Turn Intercept ON
Open Burp browser

Step 2: Capture Login Request

The DVWA brute force login page was accessed.

Random credentials were entered:

Username: test
Password: test

The login request was intercepted by Burp Suite.

Captured Request Example:

POST /DVWA/vulnerabilities/brute/ HTTP/1.1
Host: 192.168.x.x
Content-Type: application/x-www-form-urlencoded

username=test&password=test&Login=Login

Step 3: Send Request to Intruder

The intercepted request was sent to Burp Intruder.

Procedure:

Right click intercepted request
Select "Send to Intruder"

Step 4: Configure Payload Positions

In the Intruder Positions tab, the username and password values were selected as payload positions.

Example:

username=§test§&password=§test§

The § symbols indicate areas where Burp will insert payload values during the attack.

Step 5: Select Attack Type

Attack Type Selected:
Cluster Bomb

The Cluster Bomb attack type tests all possible combinations between multiple payload lists.

Step 6: Configure Username Payload

Payload Set 1 Configuration:

Payload Type:
Runtime File

Wordlist:
/usr/share/wordlists/metasploit/http_default_users.txt

Example Usernames:

admin
root
guest
administrator
test

Step 7: Configure Password Payload

Payload Set 2 Configuration:

Payload Type:
Runtime File

Wordlist:
/usr/share/wordlists/metasploit/http_default_passwords.txt

Example Passwords:

password
123456
admin
guest
qwerty

Step 8: Start the Attack

The Intruder attack was started.

Burp Suite automatically submitted multiple login requests using different combinations of usernames and passwords from the supplied wordlists.

RESULT ANALYSIS

The Intruder results table was analyzed using the following indicators:

Response Length
HTTP Status Code
Response Time

Most failed login attempts returned responses with identical lengths.

Example Failed Responses:

Username: admin
Password: test
Length: 5321

Username: guest
Password: guest
Length: 5321

However, one request returned a significantly larger response length.

Successful Response:

Username: admin
Password: password
Length: 6178

The larger response indicated successful authentication and access to additional application content.

VALID CREDENTIALS IDENTIFIED

Username: admin
Password: password

ROOT CAUSE

The vulnerability exists because the application lacks basic authentication security controls.

Missing protections include:

No rate limiting
No account lockout
No CAPTCHA
Weak default credentials
No brute-force detection mechanisms
SECURITY IMPACT

An attacker exploiting this vulnerability could:

Gain unauthorized access
Access sensitive information
Perform administrative actions
Compromise user accounts
Conduct further attacks against the application
RECOMMENDATIONS

The following security controls should be implemented to mitigate brute force attacks:

Implement Rate Limiting
Restrict repeated login attempts from the same IP address.
Enable Account Lockout
Temporarily lock accounts after multiple failed attempts.
Use CAPTCHA
Prevent automated login attempts.
Enforce Strong Password Policies
Require complex and unique passwords.
Implement Multi-Factor Authentication (MFA)
Add additional authentication verification.
Monitor Authentication Logs
Detect suspicious login behavior and repeated failures.
CONCLUSION

The assessment confirmed that the DVWA login functionality at the Low security level is vulnerable to brute force attacks.

Due to the absence of authentication protections, automated tools such as Burp Suite Intruder were able to successfully identify valid credentials using common username and password combinations.

This demonstrates the importance of implementing strong authentication security controls to prevent unauthorized access and automated credential attacks.
