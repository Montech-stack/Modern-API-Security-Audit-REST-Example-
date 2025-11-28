# 🔐 Modern API Security Audit: User Management REST Service

### 📝 Executive Summary

The targeted REST API, which manages customer account information, was found to have a severe **Broken Object Level Authorization (BOLA)** flaw. This indicates a systemic failure to validate resource ownership on the server side. The overall risk is **High** because any authenticated user can view, modify, and delete data belonging to other accounts, leading to a high volume of sensitive user data exposure.

---

### 🎯 Scope and Methodology

* **Target:** REST API endpoints managing user profiles, orders, and payment methods (`/api/v2/user/{user_id}`, `/api/v2/orders/{order_id}`).
* **Environment:** Development environment for the **User Management API v2.1**.
* **Focus:** Testing the integrity of authorization (IDOR/BOLA) and authentication (JWT handling). Tools used were **Burp Suite** and **Postman**.

---

### 🔍 Key Findings Summary

| ID | Finding Title | Severity | Endpoint Tested | Remediation Focus |
| :--- | :--- | :--- | :--- | :--- |
| **API-01** | **Broken Object Level Authorization (BOLA)** | **Critical** | `GET /api/v2/user/{user_id}` | Middleware/Authorization Check |
| **API-02** | Missing Rate Limiting on Login | High | `POST /api/v2/auth/login` | Infrastructure/API Gateway |
| **API-03** | Sensitive Data Exposure in JWT Payload | Medium | `/api/v2/auth/token` | Token Design/Claims |

---

### 🛠️ Finding Detail: Broken Object Level Authorization (API-01)

| Detail | |
| :--- | :--- |
| **Risk** | **Critical** |
| **CVE/CWE** | CWE-285 (Improper Authorization) |
| **Impact** | Allows an attacker to **enumerate and steal** the profiles of all users by manipulating the resource identifier (`user_id`) in the path. |

#### Proof of Concept (PoC)
1.  Log in as **User A** (ID: 101) and capture the valid **JWT token (Token A)**.
2.  Use **Token A** in the request header to attempt to retrieve the data for **User B** (ID: 102).
3.  The API successfully returns the sensitive profile data for User B (name, address, phone number).

#### Request/Response Snippet
> **Malicious Request (Attacker is User 101, targeting User 102):**
