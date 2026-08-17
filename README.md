# Web Application VAPT

This directory contains web application vulnerability assessment and penetration testing projects that I completed as part of my cybersecurity training.

These projects gave me hands-on experience in approaching web applications from an attacker's perspective — from reconnaissance and vulnerability discovery to manual validation, exploitation, proof-of-concept development, and reporting.

The two assessments were performed in different training environments and used different targets, which gave me the opportunity to work through a range of authentication, authorization, input validation, business logic, and web security issues.

---

## Projects

### 🔴 Merdeka Bank

A web application penetration testing project completed as part of the **Merdeka Siber cybersecurity bootcamp**.

The assessment focused on authentication, authorization, transaction functionality, business logic, file uploads, XSS, and information disclosure.

I identified and documented **11 findings**, including:

- Weak Authentication Scheme
- Insecure Direct Object Reference (IDOR)
- Authentication Bypass
- Business Logic Flaw
- Unrestricted File Upload
- Reflected XSS
- Stored XSS
- Information Disclosure

Several of the findings involved manually manipulating HTTP requests with Burp Suite to determine whether authorization and business rules were properly enforced on the server side.

👉 **[View Merdeka Bank Assessment](./merdeka-bank/)**

---

### 🔴 JerseySekaT

A black-box web application penetration testing project completed as part of the **Codelamp Indonesia cybersecurity bootcamp**.

The assessment was performed from an external attacker's perspective and covered reconnaissance, endpoint discovery, authentication testing, input validation, file uploads, and security configuration.

The assessment resulted in **7 findings**, including:

- SQL Injection
- Stored XSS
- Directory Listing
- Security Misconfiguration
- Unrestricted File Upload → Stored XSS

One of the more interesting findings was the file upload issue, which could be chained with Stored XSS to demonstrate a more realistic attack path.

👉 **[View JerseySekaT Assessment](./jersey-sekat/)**

---

## Skills Practiced

Across these assessments, I practiced:

### Reconnaissance

- Application and endpoint discovery
- Directory enumeration
- Information disclosure analysis

### Web Application Security

- Authentication testing
- Authorization and access control testing
- IDOR testing
- SQL Injection
- Cross-Site Scripting
- File upload testing
- Business logic testing
- Security misconfiguration analysis

### Manual Testing

- HTTP request/response analysis
- Parameter manipulation
- Burp Suite interception and modification
- Proof-of-concept validation

### Reporting

- Vulnerability description
- Impact assessment
- Severity classification
- Proof-of-concept documentation
- Remediation recommendations

---

## My Approach

I try not to treat a vulnerability scanner's output as the final answer.

For these projects, the more important part was understanding **why** a finding existed and whether it could actually be exploited.

My general workflow was:

1. Map the application and its attack surface.
2. Identify potentially interesting inputs and functionality.
3. Test the application's security controls.
4. Intercept and modify requests where necessary.
5. Validate whether the behavior was actually exploitable.
6. Document reproducible evidence.
7. Assess the potential impact.
8. Provide remediation recommendations.

This helped me practice the difference between simply finding a potential vulnerability and being able to explain and demonstrate its security impact.

---

## Methodologies

The assessments used a structured penetration testing approach based on the **Penetration Testing Execution Standard (PTES)**.

The findings were also assessed using **CVSS** where applicable.

Each individual project contains its own report and selected proof-of-concept evidence.

---

## Repository Structure

```text
vapt/
├── README.md
│
├── merdeka-bank/
│   ├── README.md
│   ├── report.pdf
│   └── screenshots/
│
└── jersey-sekat/
    ├── README.md
    ├── report.pdf
    └── screenshots/
