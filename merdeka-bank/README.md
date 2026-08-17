# Merdeka Bank — Web Application VAPT

A web application penetration testing project completed as part of the Merdeka Siber cybersecurity bootcamp.

The goal of this assessment was to identify and validate security weaknesses in the Merdeka Bank web application. The testing covered authentication, authorization, input handling, business logic, file uploads, and client-side security.

The assessment was performed through reconnaissance, manual testing, request manipulation, exploitation, and proof-of-concept validation, followed by risk assessment and remediation recommendations.

> ⚠️ **Disclaimer**
>
> This assessment was performed against an authorized training/laboratory environment as part of a cybersecurity bootcamp.
>
> The original laboratory environment is no longer available. This repository preserves the original assessment report and selected proof-of-concept evidence for educational and portfolio purposes.
>
> Sensitive information from the original assessment has been or should be redacted before public distribution.

---

## Project Overview

| | |
|---|---|
| **Project** | Merdeka Bank Web Application |
| **Assessment Type** | Web Application VAPT |
| **Testing Approach** | Manual testing and exploitation |
| **Methodology** | Penetration Testing Execution Standard (PTES) |
| **Risk Rating** | CVSS v3.1 |
| **Assessment Date** | November 2025 |
| **Role** | Penetration Testing / Security Assessment |

The original report was prepared as *Penetration Testing Tahap 1 - Merdeka Bank*. The assessment followed a PTES-based workflow covering intelligence gathering, threat modeling, vulnerability analysis, exploitation, and reporting.

---

## What I Tested

The assessment covered several areas of the application, including:

- Authentication and OTP mechanisms
- Authorization and access control
- Transaction functionality
- User-controlled parameters
- Business logic
- File upload functionality
- Cross-Site Scripting
- Information disclosure
- User enumeration

The testing relied heavily on manual request analysis and manipulation to determine whether security controls were actually enforced by the server.

---

## Findings

The assessment resulted in **11 findings**:

| # | Finding | Severity | Status |
|---|---|---|---|
| 1 | Weak Authentication Scheme | 🔴 Critical | Open |
| 2 | Insecure Direct Object Reference | 🔴 Critical | Open |
| 3 | Authentication Bypass | 🔴 Critical | Open |
| 4 | Information Disclosure | 🟠 High | Open |
| 5 | Insecure Direct Object Reference | 🟠 High | Open |
| 6 | Insecure Direct Object Reference | 🟠 High | Open |
| 7 | Business Logic Flaw | 🟠 High | Open |
| 8 | Unrestricted File Upload | 🟡 Medium | Open |
| 9 | Cross-Site Scripting (XSS) - Reflected | 🟡 Medium | Open |
| 10 | Cross-Site Scripting (XSS) - Stored | 🟡 Medium | Open |
| 11 | Information Disclosure | 🟢 Low | Open |

---

## Selected Findings

### 🔴 Weak Authentication Scheme

The login process used a 4-digit OTP that could be brute-forced because there was no effective rate limiting.

During testing, the OTP values also showed a predictable pattern. This made the authentication mechanism significantly weaker than a properly generated and protected OTP.

**Potential impact**

- Authentication compromise
- Account takeover
- Unauthorized access to user data

**Recommendation**

Use cryptographically secure and unpredictable OTPs, enforce short expiration periods, and limit the number of verification attempts per account or session.

---

### 🔴 IDOR — Transfer Manipulation

This was one of the most significant findings from the assessment.

The transfer request contained user-controlled parameters for the source account, destination account, and transaction amount. I intercepted the request with Burp Suite and modified these values before forwarding it to the application.

The modified request was accepted, and the resulting transfer could be confirmed through the transaction history.

**Potential impact**

An attacker could potentially manipulate transactions outside their intended authorization scope, creating a serious risk of financial fraud and unauthorized fund transfers.

**Recommendation**

Authorization must be enforced server-side for every transaction. The application should verify that the authenticated user is authorized to operate on the source account and that the requested transaction is valid.

---

### 🔴 Authentication Bypass

The application required an OTP as part of the login process, but the OTP verification step could be bypassed.

During testing, returning to the login page and submitting the same credentials allowed the authentication flow to proceed without completing the expected OTP verification.

**Potential impact**

This could allow an attacker to bypass an intended authentication control and gain access without completing the full authentication process.

**Recommendation**

The authentication state and OTP verification status should be enforced entirely on the server side. Access to authenticated functionality should only be granted after all required authentication steps have been successfully completed.

---

### 🟠 Information Disclosure

The application exposed information that should not have been available to unauthorized users.

Information disclosure issues can make further attacks easier by providing attackers with additional knowledge about the application, its users, or its internal behavior.

**Recommendation**

Review all information returned by the application and ensure that sensitive data is only exposed to authorized users.

---

### 🟠 IDOR — Unauthorized User Data Access

The application exposed user-controlled identifiers that could be modified to access data belonging to other users.

By changing the `customer_id` parameter, data belonging to another user could be retrieved outside the current session.

**Potential impact**

This could expose sensitive personal information and potentially allow large-scale enumeration of user records.

**Recommendation**

Every request involving a user-controlled object identifier should be checked against the authenticated user's authorization on the server side.

---

### 🟠 IDOR — Transaction History

A similar access control issue was found in the transaction history functionality.

The application relied on a user-controlled customer identifier when retrieving transaction information. Changing the identifier allowed transaction data belonging to another user to be accessed.

**Recommendation**

Transaction history should always be associated with the authenticated user's identity on the server side rather than trusting an identifier supplied by the client.

---

### 🟠 Business Logic Flaw

The assessment also uncovered a business logic issue in the virtual account payment flow.

The transaction amount could be modified in the request. During testing, the amount was changed to a different value using Burp Suite, and the application still accepted the modified request and recorded the transaction.

This is particularly important because the issue is not caused by a traditional injection vulnerability. The application simply failed to enforce an important business rule on the server side.

**Potential impact**

- Transaction amount manipulation
- Financial loss
- Transaction fraud

**Recommendation**

Transaction amounts and other financial parameters should be validated against the expected transaction state and business rules on the server side.

---

### 🟡 Unrestricted File Upload

The profile functionality allowed an HTML file to be uploaded without sufficient validation of the uploaded content.

Allowing active content to be uploaded and served by the application increases the attack surface and can become particularly dangerous when combined with other client-side vulnerabilities.

**Recommendation**

Validate both the file type and content, restrict permitted extensions and MIME types, and store uploaded files outside executable web directories.

---

### 🟡 Reflected XSS

The dashboard search functionality reflected user-controlled input into the application response.

A crafted XSS payload could be used to trigger JavaScript execution when the resulting page was rendered.

**Recommendation**

Apply appropriate input validation and context-aware output encoding before rendering user-controlled data.

---

### 🟡 Stored XSS

The feedback functionality was vulnerable to Stored XSS.

A malicious payload could be stored by the application and subsequently executed whenever the affected feedback was displayed to users.

**Potential impact**

Depending on the user's privileges and application context, successful exploitation could lead to session compromise, unauthorized actions, or exposure of sensitive information.

**Recommendation**

Validate and sanitize user input, apply context-aware output encoding, and prevent untrusted HTML or JavaScript from being stored and rendered.

---

### 🟢 Information Disclosure — User Enumeration

The login functionality returned distinguishable responses that could be used to determine whether a username existed.

Although this finding has a lower severity than the authentication and authorization issues above, username enumeration can make subsequent credential attacks more effective.

**Recommendation**

Use consistent authentication error responses and avoid revealing whether a particular username exists.

---

## Proof of Concept

The repository includes selected screenshots from the original assessment report.

The screenshots were chosen to show the most relevant parts of the testing process rather than reproducing every page of the original report.

### 01 — OTP Brute Force

![OTP Brute Force](./screenshots/01-weak-authentication-otp-bruteforce.png)

Burp Suite was used to test the OTP verification mechanism and validate the lack of effective rate limiting.

---

### 02 — IDOR / Transfer Manipulation

![IDOR Transfer](./screenshots/02-idor-transfer-request.png)

The transfer request was intercepted and modified to test whether the application properly validated the source and destination accounts.

---

### 03 — Authentication Bypass

![Authentication Bypass](./screenshots/03-authentication-bypass.png)

The OTP verification step was bypassed during the authentication flow.

---

### 04 — Business Logic / Amount Manipulation

![Business Logic](./screenshots/04-business-logic-amount-manipulation.png)

The transaction amount was modified in the intercepted request and the application still processed the transaction.

---

### 05 — Stored XSS

![Stored XSS](./screenshots/05-stored-xss.png)

A stored XSS payload was successfully executed when the affected feedback was displayed.

---

## Tools & Techniques

### Tools

- Burp Suite
- Web browser
- HTTP request/response analysis

### Techniques

- Web application reconnaissance
- Authentication testing
- Authorization testing
- IDOR testing
- Parameter manipulation
- OTP brute-force testing
- Business logic testing
- File upload testing
- Cross-Site Scripting testing
- Information disclosure analysis
- Manual proof-of-concept validation

---

## Key Takeaways

This project gave me practical experience beyond simply identifying vulnerabilities.

In particular, it involved:

- Understanding how authentication and authorization were implemented
- Intercepting and modifying real HTTP requests
- Validating whether security controls were enforced server-side
- Testing application-specific business logic
- Confirming vulnerabilities through reproducible proof of concept
- Assessing the potential impact of each finding
- Translating technical findings into remediation recommendations

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
