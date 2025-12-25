# WordPress to Linux Root Compromise – Penetration Test Report

**Assessment Type:** Black-Box Penetration Test  
**Environment:** Lab-based Linux Server running WordPress  
**Tester:** Vishnu K Anand  

---

## 1. Scope
The assessment targeted a Linux-based host exposing a WordPress web application and SSH service.  
The objective was to identify vulnerabilities that could lead to unauthorized access, privilege escalation, and full system compromise.

**Out of scope:** Denial-of-service attacks and social engineering.

---

## 2. Methodology
The assessment followed a structured penetration testing approach:

- Network reconnaissance and service enumeration  
- Web application enumeration  
- Credential discovery and authentication testing  
- Remote code execution  
- Post-exploitation and privilege escalation  

---

## 3. Reconnaissance & Enumeration

### 3.1 Network Enumeration
Service enumeration identified the following exposed services:

| Port | Service | Notes |
|-----:|--------|-------|
| 22   | SSH    | Authentication required |
| 80   | HTTP   | WordPress application |
| 443  | HTTPS  | Encrypted web service |

The presence of a CMS indicated a potential attack surface through misconfiguration or exposed sensitive files.

---

## 4. Web Application Enumeration

### 4.1 Sensitive File Disclosure
Manual browsing and directory enumeration revealed publicly accessible files referenced in `robots.txt`, including:

- A downloadable wordlist file  
- A file containing sensitive information  

Further inspection of an additional web-accessible endpoint exposed **Base64-encoded credentials**, which were decoded to recover valid application login details.

**Impact:**
- Credential exposure  
- Unauthorized access to administrative functionality  

---

## 5. Exploitation

### 5.1 WordPress Administrative Access
Recovered credentials were successfully used to authenticate to the WordPress administrative panel, granting full administrative privileges.

**Impact:**
- Full control over application configuration  
- Ability to modify server-side code  

---

### 5.2 Remote Code Execution
Administrative access allowed modification of WordPress theme files.  
A server-side payload was introduced into a theme template and executed, resulting in a reverse shell on the underlying Linux host.

**Impact:**
- Remote command execution  
- Initial foothold on the operating system  

---

## 6. Post-Exploitation

### 6.1 Credential Discovery
Local enumeration revealed files containing hashed credentials associated with a secondary system user.  
The hash was cracked offline, enabling authentication as the lower-privileged user.

---

### 6.2 User-Level Access
Using the recovered credentials, access to restricted user-level files was obtained, confirming lateral movement within the system.

---

## 7. Privilege Escalation

### 7.1 SUID Binary Misconfiguration
Enumeration of SUID binaries identified a misconfigured instance of `nmap` running with elevated privileges.

Abusing this misconfiguration allowed execution of arbitrary commands with root privileges, resulting in full system compromise.

**Impact:**
- Root-level access  
- Complete loss of confidentiality, integrity, and availability  

---

## 8. Risk Summary

| Issue | Severity |
|-----|---------|
| Credential Exposure via Web Content | High |
| Remote Code Execution via CMS Admin | High |
| Weak Password Storage (MD5) | Medium |
| SUID Binary Misconfiguration | Critical |

---

## 9. Remediation Recommendations
- Remove sensitive files from web-accessible directories  
- Enforce least-privilege access for CMS administrators  
- Disable execution of arbitrary code through theme editing  
- Use strong password hashing algorithms (bcrypt/argon2)  
- Audit and restrict SUID binaries on production systems  

---

## 10. Conclusion
The assessment demonstrated how exposed sensitive files and weak credential handling within a CMS can be chained with local privilege misconfigurations to achieve full system compromise.  
This attack path reflects common real-world exploitation scenarios observed during penetration tests.

---

> **Disclaimer:**  
> This report documents testing performed in a controlled lab environment for educational and demonstration purposes only.
