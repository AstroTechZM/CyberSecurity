##  DVWA SQL Injection Level: low

**Executive Summary:** An SQL Injection vulnerability was discovered in the search field of the Damn Vulnerable Web Application, Hence forth known as the **DVWA**, the vulnerability allows an attacker to query and retrieve sensitive information from the database.

## Vulnerability Details
* **Description:** The application is not sanitising user supplied data from the search input field, specifically SQL statements are not adequately escaped before running them, allowing attacker supplied queries to be executed and displayed back on the page 
* **Affected Components:** The search input field within the web application
* **Parameters:** The **ID** parameter supplied through the search input field
* **Impact:**
	1. **Confidentiality:** An attacker can steal sensitive user information including passwords.
	2. **Integrity:** An attacker could potentially change the contents of the database.
	3. **Availability:**
	4. **Reputational Damage:**Leaking sensitive information of users can have significant reputational damage.
	5. **Severity:** **High.** The persistence nature of the **SQLi** and the potential of information leakage makes this a high-severity vulnerability.
	6. **Likelihood of Exploitation:** **High.** crafting the payload is relatively easy.
## Proof Of Concept(POC)
1. In the web application navigate to http://127.0.0.1:42001/vulnerabilities/sqli/ 
2. Locate the search input field at the top of the page
3. In the search input field, enter the following payload:
	```1' or '1'='1'# ```
4. this will be return all user full names
	![payload output](SQLi%20low.png)	
	this confirms the vulnerability, to take it further to demonstrate severity proceed to step 5.
5. In the search input field, enter the following payload.
	```1' UNION SELECT 1, table_name FROM information_schema.tables WHERE table_schema = DATABASE()# ```
6. the output will be a list of tables in the database.
	![payload output](SQLi%20tables%20low.png)
7. run the following command in the search input field to retrieve a list of columns in the ``users`` table.
	```1' UNION SELECT 1, column_name FROM information_schema.columns WHERE table_schema = DATABASE() AND table_name = 'users'#```
8. The output reveals valuable information about the structure of the database
![payload output](DB%20schema%20low.png)
9. run the following command to get the passwords of the users
	``1' UNION SELECT 1, password FROM users#``
	![payload output for passwords](password%20low.png) 

**Remediation:**

  * **Recommended Solution:** use parameterized queries (prepared statements) and validate user input on the server-side.
  * **Priority:** **High**. Due to the potential impact, this vulnerability should be addressed immediately.
  
  **Reporter Information:**

  * **Name:** Henry Mate Security Analyst
  * **Date Discovered:** July 10, 2025

**References:**
* [OWASP SQL injection](https://owasp.org/www-community/attacks/SQL_Injection)

