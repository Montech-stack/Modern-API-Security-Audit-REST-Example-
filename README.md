# ⚙️ OWASP Top 10 Vulnerability Assessment: Juice Shop Target

### 📝 Executive Summary

This security assessment targeted the OWASP Juice Shop application and identified **12 High and Medium severity findings**, primarily related to **Injection Flaws** (SQLi, XSS) and **Broken Authentication**. These observed vulnerabilities present a significant risk of **data theft** and **unauthorized access**. Immediate remediation is critical to mitigate exposure to common, high-impact attacks (as defined by the current OWASP Top 10).

---

### 🎯 Scope and Methodology

* **Target Application:** OWASP Juice Shop v1.x (Simulated Environment).
* **Scope:** All public-facing login, registration, and product viewing pages (`/login`, `/register`, `/rest/products`).
* **Testing Approach:** Black Box testing utilizing **Burp Suite Professional** for traffic interception and manual techniques focused on validating the top ten risks.

---

### 🔍 Key Findings Summary

| ID | Finding Title | Severity | Location | OWASP Category |
| :--- | :--- | :--- | :--- | :--- |
| **JS-01** | **Unauthenticated SQL Injection (Login Bypass)** | **Critical** | Login Form `email` parameter | A03: Injection |
| **JS-02** | Stored Cross-Site Scripting (XSS) | High | Customer Feedback Form | A07: Identification and Auth Failures |
| **JS-03** | Broken Function Level Authorization (BFLA) | Medium | `/api/users/{id}` endpoint | A01: Broken Access Control |
| **JS-04** | Insecure Direct Object Reference (IDOR) | Medium | Order History Page | A01: Broken Access Control |

---

### 🛠️ Finding Detail: Unauthenticated SQL Injection (JS-01)

| Detail | |
| :--- | :--- |
| **Risk** | **Critical** |
| **CVE/CWE** | CWE-89 (Improper Neutralization of Special Elements used in an SQL Command) |
| **Impact** | Allows an **unauthenticated attacker** to bypass the login mechanism and gain full administrative access, leading to database exfiltration. |

#### Proof of Concept (PoC)
1.  Navigate to the `/login` page.
2.  In the **Email** field, enter the payload: `' OR 1=1 --`
3.  In the **Password** field, enter any value (e.g., `password123`).
4.  Submitting the form results in a successful login as the **first user** in the database (typically the administrator).

#### Request/Response Snippet
> **Malicious Request:**
