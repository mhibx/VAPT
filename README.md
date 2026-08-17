# Web Application VAPT

A collection of web application Vulnerability Assessment and Penetration Testing (VAPT) projects conducted as part of hands-on cybersecurity training and bootcamp assessments.

The projects document the process of identifying, validating, exploiting, and reporting web application vulnerabilities, including proof-of-concept evidence, impact analysis, risk classification, and remediation recommendations.

> ⚠️ **Disclaimer**
>
> These assessments were conducted against intentionally vulnerable or authorized lab environments as part of cybersecurity training and educational activities.
>
> The targets documented in this repository are not intended to represent unauthorized testing of real-world systems.

---

## 📂 Projects

### 01 — Merdeka Bank Web Application

Penetration testing assessment of the Merdeka Bank web application conducted as part of the Merdeka Siber bootcamp.

The assessment followed a penetration testing methodology based on the Penetration Testing Execution Standard (PTES), covering intelligence gathering, threat modeling, vulnerability analysis, exploitation, and reporting. 

#### Key Findings

- 🔴 Authentication Bypass
- 🔴 Insecure Direct Object Reference (IDOR)
- 🔴 Weak Authentication Scheme / OTP Brute Force
- 🟠 Business Logic Flaw
- 🟠 Unrestricted File Upload
- 🟡 Reflected XSS
- 🟡 Stored XSS
- 🟡 Information Disclosure
- 🟡 User Enumeration

#### Selected Techniques

- Web application reconnaissance
- Authentication testing
- Access control testing
- Parameter manipulation
- Burp Suite interception and request modification
- OTP brute-force testing
- Cross-Site Scripting testing
- File upload validation testing
- Business logic testing

**[→ View Merdeka Bank Case Study](./merdeka-bank/)**

---

### 02 — JerseySekaT Web Application

Web application penetration testing project conducted as part of the Codelamp Indonesia cybersecurity bootcamp.

The assessment focused on identifying common web application vulnerabilities through reconnaissance, directory and endpoint discovery, manual testing, exploitation, and proof-of-concept validation.

#### Key Findings

- 🔴 SQL Injection
- 🟠 Stored XSS
- 🟡 Unrestricted File Upload leading to Stored XSS
- 🟠 Directory Listing
- 🟡 Security Misconfiguration

#### Selected Techniques

- Directory and endpoint enumeration
- Web application reconnaissance
- SQL injection testing
- Cross-Site Scripting testing
- File upload validation testing
- Burp Suite request interception and manipulation
- Security header analysis

**[→ View JerseySekaT Case Study](./jersey-sekat/)**

---

## 🛠️ Tools & Technologies

| Category | Tools |
|---|---|
| Proxy / Web Testing | Burp Suite |
| Reconnaissance | Dirsearch |
| Vulnerability Testing | Manual testing & custom payloads |
| Web Analysis | Browser Developer Tools |
| Documentation | Markdown, Microsoft Word / PowerPoint |
| Operating System | Linux / Kali Linux |

---

## 🔍 Methodology

The assessments generally followed a structured penetration testing workflow:

```text
Reconnaissance
      ↓
Enumeration
      ↓
Vulnerability Analysis
      ↓
Manual Validation
      ↓
Exploitation / PoC
      ↓
Impact Analysis
      ↓
Risk Classification
      ↓
Remediation Recommendations
      ↓
Reporting
