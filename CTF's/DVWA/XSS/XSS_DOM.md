##  DVWA XSS DOM Level: High

**Executive Summary:** A DOM based XSS vulnerability was discovered in the url that sends language preference of the Damn Vulnerable Web Application, the vulnerability allows an attacker to run arbitrary javascript commands.

## Vulnerability Details
* **Description:** The application is not adequately sanitising user supplied data from the url that sends language preference, the application is not differenciating user supplied data from code, allowing an attacker to run javasript commands on the page 
* **Affected Components:** The url language parameter within the web application
* **Parameters:** The **default** parameter supplied through the drop down menu
* **Impact:**
	1. **Confidentiality:** An attacker can steal sensitive user information including passwords.
	2. **Reputational Damage:**Leaking sensitive information of users can have significant reputational damage.
	3. **Severity:** **High.** The persistence nature of the **SQLi** and the potential of information leakage makes this a high-severity vulnerability.
	4. **Likelihood of Exploitation:** **High.** crafting the payload is relatively easy.
## Proof Of Concept(POC)
1. navigate to http://127.0.0.1:42001/vulnerabilities/xss_d/
2. notice the drop down menu for selecting the default laguange
3. when the language is chosen the url becomes http://127.0.0.1:42001/vulnerabilities/xss_d/?default=Spanish
4. change this to ``http://127.0.0.1:42001/vulnerabilities/xss_d/?#default=<script>alert("vulnerability found")</script>``
**Remediation:**
  * **Recommended Solution:** use content security policy(CSP) to prevent scripts originating from untrusted sources from executing.
  * **Priority:** **medium**. Due to the potential impact, this vulnerability should be addressed immediately.
  
  **Reporter Information:**

  * **Name:** Henry Mate Security Analyst
  * **Date Discovered:** July 20, 2025

**References:**
* [OWASP SQL injection](https://owasp.org/www-community/attacks/XSS)

