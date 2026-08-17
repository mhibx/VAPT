# JerseySekaT — Web Application VAPT

A web application penetration testing project completed as part of the Codelamp Indonesia cybersecurity bootcamp.

The goal of this assessment was to identify and validate security weaknesses in the JerseySekaT web application from an external attacker's perspective.

The testing combined reconnaissance and automated discovery with manual validation, request manipulation, exploitation, and proof-of-concept testing.

> ⚠️ **Disclaimer**
>
> This assessment was performed against an authorized training/laboratory environment as part of a cybersecurity bootcamp.
>
> The original laboratory environment is no longer available. This repository preserves the original assessment report and selected proof-of-concept evidence for educational and portfolio purposes.

---

## Project Overview

| | |
|---|---|
| **Project** | JerseySekaT Web Application |
| **Assessment Type** | Web Application VAPT |
| **Testing Approach** | Black-box |
| **Testing Area** | External |
| **Methodology** | Penetration Testing Execution Standard (PTES) |
| **Risk Rating** | CVSS |
| **Assessment Date** | 16–18 November 2025 |
| **Role** | Penetration Testing / Security Assessment |

The assessment was performed from an external perspective without relying on internal application knowledge or privileged access.

---

## What I Tested

The testing covered several areas of the application, including:

- Application and endpoint discovery
- Authentication functionality
- Input validation
- SQL Injection
- Cross-Site Scripting
- File upload handling
- Directory exposure
- Security headers
- Information disclosure
- Web server configuration

Automated discovery was used where appropriate, followed by manual validation to determine whether the identified issues were actually exploitable.

---

## Findings

The assessment identified **7 findings**:

| # | Finding | Severity | Status |
|---|---|---|---|
| 1 | SQL Injection | 🔴 Critical | Open |
| 2 | Cross-Site Scripting (XSS) - Stored | 🟠 High | Open |
| 3 | Directory Listing | 🟠 High | Open |
| 4 | Security Misconfiguration | 🟡 Medium | Open |
| 5 | Security Misconfiguration | 🟡 Medium | Open |
| 6 | Security Misconfiguration | 🟡 Medium | Open |
| 7 | Unrestricted File Upload to Stored XSS | 🟡 Medium | Open |

---

## Selected Findings

### 🔴 SQL Injection

SQL Injection was identified in the login functionality.

I tested the login parameters with SQL injection payloads and was able to bypass the normal authentication process and access an administrative account.

This demonstrated that user-controlled input was being incorporated into a database query without sufficient protection.

**Potential impact**

- Authentication bypass
- Unauthorized administrative access
- Potential exposure of database contents
- Potential modification or deletion of application data

**Recommendation**

Use parameterized queries or prepared statements for database operations and avoid constructing SQL queries directly from user-controlled input.

---

### 🟠 Stored XSS

Stored Cross-Site Scripting was identified in the product review functionality.

A JavaScript payload submitted through the review field was stored by the application and executed when the affected content was displayed.

Unlike reflected XSS, the payload did not need to be supplied again by the victim because it had already been stored by the application.

**Potential impact**

Depending on the user's privileges and application context, successful exploitation could allow:

- Session compromise
- Sensitive data theft
- Unauthorized actions
- Malicious redirects

**Recommendation**

Validate user input and apply context-aware output encoding before displaying user-controlled content.

---

### 🟠 Directory Listing

The `/uploads/` directory was publicly accessible and allowed directory indexing.

This exposed the contents of the directory and made uploaded files easier to discover.

**Potential impact**

An attacker could use the exposed directory to:

- Discover uploaded files
- Gather information about the application
- Identify potentially sensitive resources
- Improve reconnaissance for further attacks

**Recommendation**

Disable directory indexing and restrict direct access to uploaded resources where appropriate.

---

### 🟡 Security Misconfiguration — `robots.txt`

The public `robots.txt` file disclosed the `/admin/` path.

While `robots.txt` is not an access control mechanism, exposing administrative paths can make reconnaissance easier.

**Recommendation**

Do not rely on `robots.txt` to protect sensitive resources. Administrative functionality should be protected through proper authentication and authorization controls.

---

### 🟡 Security Misconfiguration — Security Headers

Security header testing showed that only **1 of 11 tested security headers** was present.

The missing headers included controls intended to reduce risks such as XSS, clickjacking, MIME sniffing, and other browser-based attacks.

**Recommendation**

Review and implement appropriate security headers based on the application's requirements, including controls such as:

- Content-Security-Policy
- Strict-Transport-Security
- X-Frame-Options
- X-Content-Type-Options
- Referrer-Policy

---

### 🟡 Security Misconfiguration — `.DS_Store`

A publicly accessible `.DS_Store` file was discovered during reconnaissance.

Although `.DS_Store` files are commonly created by macOS, exposing them on a web server can disclose information about directory structure and application resources.

**Recommendation**

Remove development artifacts such as `.DS_Store` files before deployment and prevent access to hidden or unintended files through web server configuration.

---

### 🟡 Unrestricted File Upload → Stored XSS

The application did not adequately validate uploaded file types.

During testing, I uploaded a file through the available upload functionality and manipulated the request using Burp Suite. The uploaded content could subsequently be served as HTML, allowing an XSS payload to execute in the browser.

This finding was particularly interesting because the issue involved more than a simple file extension bypass. The upload weakness could be chained with client-side execution.

**Potential impact**

- Stored XSS
- Session compromise
- Sensitive data theft
- Malicious redirects
- Potential delivery of malicious content

**Recommendation**

Validate uploaded files based on their actual content rather than relying only on their extension. Uploaded files should also be stored outside executable web directories, with restrictive permissions and controlled access.

---

## Proof of Concept

The repository includes selected screenshots extracted from the original assessment report.

The screenshots were selected to demonstrate the most important parts of the testing process rather than reproducing the complete report.

### 01 — Directory Enumeration

![Directory Enumeration](./screenshots/01-directory-enumeration.png)

Directory enumeration was used to identify application paths and publicly accessible resources.

---

### 02 — Directory Listing

![Directory Listing](./screenshots/02-directory-listing.png)

The `/uploads/` directory exposed its contents through directory indexing.

---

### 03 — Security Headers

![Security Headers](./screenshots/03-security-headers.png)

Security header analysis showed that only a small portion of the expected browser security controls were implemented.

---

### 04 — SQL Injection

![SQL Injection](./screenshots/04-sql-injection.png)

SQL Injection testing against the login functionality resulted in authentication bypass and access to the administrative interface.

---

### 05 — Stored XSS / File Upload

![Stored XSS](./screenshots/05-file-upload-stored-xss.png)

The file upload functionality was tested and chained with stored XSS to demonstrate the impact of insufficient upload validation.

---

## Tools & Techniques

### Tools

- Burp Suite
- Dirsearch
- Web browser
- HTTP request/response analysis

### Techniques

- External reconnaissance
- Directory and endpoint enumeration
- Authentication testing
- SQL Injection testing
- Cross-Site Scripting testing
- File upload testing
- Security header analysis
- Security misconfiguration analysis
- Manual request manipulation
- Proof-of-concept validation

---

## Key Takeaways

This project gave me practical experience in testing a web application from an external attacker's perspective.

The most valuable parts of the assessment were:

- Combining automated discovery with manual validation
- Understanding how application inputs were processed
- Using Burp Suite to intercept and modify HTTP requests
- Validating SQL Injection through an actual authentication bypass
- Testing how uploaded files were handled by the application
- Chaining an upload weakness with Stored XSS
- Assessing the impact of configuration and information disclosure issues
- Documenting findings with reproducible proof of concept and remediation recommendations

---

## Report & Evidence

The complete penetration testing report is available here:

`report.pdf`

Selected proof-of-concept evidence:

`screenshots/`

---

## Disclaimer

This project is presented for educational and portfolio purposes.

All testing was performed against an authorized training/laboratory environment. No unauthorized systems were targeted.
