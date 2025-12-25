# Linux Web Exploitation & Privilege Escalation Penetration Test Report

**Assessment Type:** Black-Box Penetration Test  
**Environment:** Lab-based Linux Server  
**Tester:** Vishnu K Anand  

---

## 1. Scope
The assessment targeted a Linux-based host exposing multiple network services, including a custom web application running on a non-standard port.  
The objective was to identify vulnerabilities leading to initial access, privilege escalation, and full system compromise.

**Out of scope:** Denial-of-service attacks and social engineering.

---

## 2. Methodology
Testing followed a structured penetration testing approach:

- Network reconnaissance and service enumeration  
- Web application enumeration  
- File upload and remote code execution testing  
- Post-exploitation enumeration  
- Local privilege escalation  

---

## 3. Reconnaissance & Enumeration

### 3.1 Network Enumeration
A service version scan identified the following exposed services:

| Port | Service | Notes |
|-----:|--------|-------|
| 21 | FTP | Accessible |
| 22 | SSH | Authentication required |
| 139/445 | SMB | Potential file shares |
| 3128 | Squid Proxy | Potential pivot point |
| 3333 | HTTP | Custom web application |

The presence of a web service on a non-standard port suggested potential misconfiguration or internal exposure.

---

## 4. Web Application Enumeration

### 4.1 Directory & Endpoint Discovery
Directory enumeration against the web service identified multiple accessible paths, including an **internal** endpoint hosting a file upload functionality.

This upload feature became the primary attack surface for further testing.

---

## 5. Exploitation

### 5.1 File Upload Bypass → Remote Code Execution
The file upload mechanism enforced extension-based restrictions.  
Initial attempts to upload server-side scripts were blocked.

Through controlled testing of file extensions, it was discovered that files with a `.phtml` extension were accepted and executed by the server.

A server-side payload was uploaded and triggered, resulting in a reverse shell on the target system.

**Impact:**
- Arbitrary remote code execution  
- Initial foothold on the host  

---

## 6. Post-Exploitation

### 6.1 User-Level Access
Local enumeration following initial access confirmed compromise of a non-privileged user account and access to restricted user-level files.

---

## 7. Privilege Escalation

### 7.1 SUID `systemctl` Misconfiguration
Enumeration of SUID binaries identified `systemctl` running with elevated privileges.

This misconfiguration allowed the creation and execution of a malicious service unit, resulting in execution of arbitrary commands as the root user.

Privilege escalation was successfully achieved, granting full root access to the system.

**Impact:**
- Full system compromise  
- Complete loss of confidentiality and integrity  

---

## 8. Risk Summary

| Issue | Severity |
|-----|---------|
| Arbitrary File Upload → Remote Code Execution | High |
| SUID `systemctl` Misconfiguration | Critical |
| Insecure Service Exposure | Medium |

---

## 9. Remediation Recommendations
- Enforce strict server-side validation for file uploads  
- Disable execution permissions on upload directories  
- Remove unnecessary SUID permissions from binaries  
- Restrict exposure of internal services and non-standard ports  
- Conduct regular privilege and permission audits  

---

## 10. Conclusion
The assessment demonstrated how an insecure file upload mechanism combined with privilege misconfigurations can be chained to fully compromise a Linux system.  
Such vulnerabilities are commonly exploited in real-world attacks and highlight the importance of layered security controls.

---

> **Disclaimer:**  
> This report documents testing performed in a controlled lab environment for educational and demonstration purposes only.
