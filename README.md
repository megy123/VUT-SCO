# Bug Bounty Web Security Research

Security assessment of a real-world production web application performed within the **OpenBugBounty responsible disclosure framework**.

The project focused on practical web application penetration testing, vulnerability validation, impact assessment, and responsible disclosure.

## Overview

The tested application was an actively deployed e-commerce platform handling customer accounts, personal information, addresses, orders, and other sensitive data.

Testing combined manual analysis with automated security tooling and focused primarily on:

* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Improper Access Control
* Input validation
* Session and authorization mechanisms
* General web application attack surface analysis

The assessment resulted in the discovery and responsible disclosure of **two reflected Cross-Site Scripting vulnerabilities**.

The vulnerabilities were later retested and are no longer reproducible, indicating that they have been remediated.

## Vulnerabilities Discovered

### Reflected XSS – Address Validation

A reflected XSS vulnerability was identified in the user address workflow.

User-controlled input was inserted into a dynamically generated validation dialog without sufficient sanitization, allowing JavaScript execution in the context of the application.

Example test payload:

```html
<img src=x onerror=alert(1)>
```

The issue was manually validated by analyzing HTTP traffic and application behavior.

### Reflected XSS – Discount Code Handling

A second reflected XSS vulnerability was identified in the shopping cart discount-code workflow.

Invalid user input was returned inside an HTML error message without appropriate output encoding, allowing attacker-controlled HTML and JavaScript to be interpreted by the browser.

This represented a separate vulnerable code path from the address-validation issue.

## Additional Security Testing

The assessment also included testing for:

### CSRF

Automated scanning initially indicated missing anti-CSRF tokens.

The behavior was subsequently investigated manually using Burp Suite to determine whether the identified condition could be practically exploited.

### Access Control

Authorization controls were tested by attempting:

* horizontal privilege escalation,
* manipulation of resource identifiers,
* access to resources without an authenticated session,
* access to administrative functionality from a regular user account.

No exploitable access-control vulnerability was confirmed during these tests.

## Methodology

The penetration testing process was divided into four main stages:

1. **Application reconnaissance and technology identification**
   The target application was first explored to understand its architecture, technologies, available endpoints and user-controlled input points. HTTP traffic was captured for further analysis, and OWASP ZAP was used to identify potential low-level security issues.
2. **Attack surface analysis and test planning**
   Collected information was analyzed to identify functionality handling sensitive data and potentially higher-risk areas. Based on this analysis, specific attack classes and application components were selected for further testing.
3. **Manual vulnerability testing and validation**
   Potential weaknesses were manually tested using Burp Suite Proxy and Repeater. Testing included input fuzzing for Cross-Site Scripting, manipulation of HTTP requests, bypassing client-side validation and testing authorization controls through identifier manipulation.
4. **Finding validation and documentation**
   Identified anomalies and confirmed vulnerabilities were documented with reproduction steps and supporting evidence. The final output also included an assessment of the findings and recommendations for remediation.

## Tools

The following tools were used during the assessment:

* **Burp Suite** – HTTP interception, request modification and manual vulnerability validation
* **OWASP ZAP** – automated vulnerability scanning
* **Firefox Developer Tools** – DOM, JavaScript and network analysis
* **Wappalyzer** – technology reconnaissance

## Responsible Disclosure

Testing was performed within the scope and rules of the **OpenBugBounty** responsible disclosure program.

## Full Report

A more detailed technical report describing the methodology, testing process and findings is available here:

[Technical Report](./report.pdf)

## Disclaimer

This repository documents authorized security research conducted for educational purposes and responsible vulnerability disclosure.

The techniques described here should only be used against systems that you own or have explicit permission to test.
