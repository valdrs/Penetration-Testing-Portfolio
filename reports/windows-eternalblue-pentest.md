# Windows SMB Exploitation – MS17-010 (EternalBlue) Penetration Test Report

**Assessment Type:** Black-Box Network Penetration Test  
**Environment:** Lab-based Windows Host  
**Tester:** Vishnu K Anand  

---

## 1. Scope
The assessment targeted a Windows-based host exposing SMB services over the network.  
The objective was to identify remotely exploitable vulnerabilities that could lead to unauthorized access, privilege escalation, and full system compromise.

**Out of scope:** Denial-of-service attacks and social engineering.

---

## 2. Methodology
The assessment followed a standard network penetration testing workflow:

- Network reconnaissance and service enumeration  
- Vulnerability identification  
- Remote exploitation  
- Post-exploitation and credential access  
- Impact assessment and reporting  

---

## 3. Reconnaissance & Vulnerability Identification

### 3.1 Network Enumeration
A vulnerability-focused service scan was conducted to identify exposed services and known weaknesses.

**Findings:**

| Port | Service | Notes |
|-----:|--------|-------|
| 135  | MSRPC  | Exposed |
| 139  | NetBIOS | Exposed |
| 445  | SMB    | Vulnerable |

The target host was identified as **vulnerable to MS17-010 (EternalBlue)**, a critical SMB vulnerability that allows unauthenticated remote code execution.

---

## 4. Exploitation

### 4.1 Remote Code Execution via MS17-010
A publicly available exploit targeting MS17-010 was used to achieve remote code execution over the SMB service.

The exploit was configured with a reverse shell payload and executed successfully, resulting in a remote command shell on the target system.

**Impact:**
- Unauthenticated remote code execution  
- Immediate compromise of the target host  

---

### 4.2 Session Stabilization
The initial shell was unstable and therefore upgraded to a Meterpreter session to enable reliable post-exploitation activities and system interaction.

---

## 5. Post-Exploitation

### 5.1 Privilege Verification
Privilege checks confirmed that code execution was achieved in the context of:

This represents the highest privilege level on a Windows system.

---

### 5.2 Credential Access
Password hashes were extracted from the compromised host and saved for offline analysis.

Offline cracking revealed plaintext credentials, demonstrating the risk of weak password policies and credential reuse.

**Impact:**
- Credential disclosure  
- Potential for lateral movement within a Windows environment  

---

### 5.3 Sensitive Data Access
System-wide searches confirmed unrestricted access to user directories and sensitive files, validating full system compromise.

---

## 6. Risk Summary

| Issue | Severity |
|-----|---------|
| MS17-010 (EternalBlue) Remote Code Execution | Critical |
| SYSTEM-Level Access | Critical |
| Credential Hash Exposure | High |

---

## 7. Remediation Recommendations
- Apply security patches addressing MS17-010 immediately  
- Disable SMBv1 where not explicitly required  
- Restrict network access to SMB services  
- Enforce strong password policies and credential hygiene  
- Monitor for exploit activity targeting SMB services  

---

## 8. Conclusion
The assessment demonstrated that an unpatched SMB vulnerability could be exploited remotely to gain SYSTEM-level access without authentication.  
Such vulnerabilities pose severe risk in enterprise environments, enabling attackers to fully compromise hosts and harvest credentials for further attacks.

---

> **Disclaimer:**  
> This report documents testing performed in a controlled lab environment for educational and demonstration purposes only.


