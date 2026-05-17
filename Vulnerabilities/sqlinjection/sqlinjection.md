SQL INJECTION VULNERABILITY ASSESSMENT REPORT

Target Application: DVWA (Damn Vulnerable Web Application)
Vulnerability Type: SQL Injection
Security Level:
Severity: Critical
Testing Environment: Educational Lab

INTRODUCTION

This report documents the exploitation of a SQL Injection vulnerability in DVWA at the Low security level.

SQL Injection occurs when user-supplied input is improperly handled within SQL queries, allowing attackers to manipulate database operations. Successful exploitation may allow unauthorized access to sensitive database information, authentication bypass, and full database compromise.

The assessment demonstrates how unsanitized user input can provide attackers with direct interaction with the backend database.

OBJECTIVE

The objective of this assessment was to:

Identify SQL Injection vulnerabilities
Bypass intended query logic
Enumerate database structure
Extract sensitive user credentials
Demonstrate database compromise risks
TOOLS USED
Web Browser
DVWA
SQL Injection Payloads
MySQL Information Schema Queries
VULNERABILITY DESCRIPTION

The DVWA SQL Injection module at the Low security level directly inserts user-supplied input into SQL queries without sanitization or parameterized statements.

Because of this, attackers can inject malicious SQL syntax into application queries and manipulate database behavior.

The vulnerability allows attackers to:

Bypass query conditions
Retrieve unauthorized data
Enumerate database tables
Extract sensitive information
EXPLOITATION PROCESS

Step 1: Access the SQL Injection Module

The DVWA SQL Injection page was opened from the vulnerabilities menu.

The application provided a user ID lookup field.

Step 2: Test Normal Functionality

A valid numeric ID was entered:

1

The application returned user information associated with ID 1.

This confirmed that database queries were processed based on user input.

BASIC SQL INJECTION TEST

The following payload was entered:

' OR 1=1#

OBSERVED RESULT

The application returned multiple user records instead of a single result.

The injected condition:

OR 1=1

always evaluates as TRUE, causing the database query to return all available records.

This confirmed that the application was vulnerable to SQL Injection.

DATABASE ENUMERATION

After confirming the vulnerability, database structure enumeration was performed.

EXTRACT TABLE NAMES

Payload Used:

' UNION SELECT table_name, null FROM information_schema.tables#

OBSERVED RESULT

The application displayed available database table names.

Relevant tables such as:

users

were identified successfully.

EXTRACT COLUMN NAMES

Payload Used:

' UNION SELECT column_name, null FROM information_schema.columns WHERE table_name='users'#

OBSERVED RESULT

The application revealed column names from the users table.

Example columns identified:

id
user
password
first_name
last_name
DUMP USER CREDENTIALS

Payload Used:

' UNION SELECT user, password FROM users#

OBSERVED RESULT

The application returned stored usernames and password hashes from the database.

This demonstrated unauthorized access to sensitive authentication data.

ROOT CAUSE

The vulnerability exists because user input is directly concatenated into SQL queries without proper security controls.

Missing protections include:

Input validation
Parameterized queries
Prepared statements
Output restrictions
Database query sanitization

The application trusts user-controlled input and executes it as part of SQL statements.

SECURITY IMPACT

An attacker exploiting this vulnerability may be able to:

Bypass authentication
Retrieve sensitive information
Dump database contents
Modify or delete records
Escalate privileges
Gain administrative access
Fully compromise the application database

Because attackers gain direct influence over database queries, the impact is considered Critical.

RECOMMENDATIONS

The following mitigations are recommended:

Use Parameterized Queries

Implement prepared statements instead of directly concatenating user input into SQL queries.

Validate User Input

Restrict input to expected formats and data types.

Implement Stored Procedures

Use controlled database procedures to minimize query manipulation risks.

Apply Least Privilege

Restrict database account permissions used by the application.

Hide Database Errors

Do not expose SQL error messages to users.

Use Web Application Firewalls (WAF)

Deploy filtering mechanisms to detect malicious SQL patterns.

Perform Security Testing

Regularly conduct penetration testing and code reviews to identify injection vulnerabilities.

CONCLUSION

The assessment confirmed that the DVWA application at the Low security level is critically vulnerable to SQL Injection attacks.

Because the application directly processes unsanitized user input within SQL queries, attackers can manipulate database behavior, enumerate database structures, and retrieve sensitive information.
