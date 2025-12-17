# **Security Auditing with Lynis**

## 1. **What is Lynis?**

- **Lynis** is an **open-source security auditing tool** for **Linux, Unix, and macOS** systems.
    
- It checks your system’s **security posture, compliance, and hardening** level.
    
- Think of it as an **automated auditor** that reviews your system against security best practices.
    

---

## 2. **Purpose of Lynis**

- Identify **misconfigurations**.
    
- Check for **known vulnerabilities** or weak settings.
    
- Verify **compliance** with standards like PCI DSS, HIPAA, ISO 27001, etc.
    
- Provide a **hardening index** (numerical score of how secure your system is).
    

👉 Example: A company hosting sensitive medical data can use Lynis to ensure servers meet **HIPAA security rules**.

---

## 3. **How Lynis Works**

Lynis performs a **local scan** of the system and checks:

- **File permissions** (are critical files too open?).
    
- **Authentication** settings (password policies, root login restrictions).
    
- **Services & processes** (are insecure services like Telnet running?).
    
- **Software updates** (outdated packages, missing security patches).
    
- **Logging & auditing** (is auditing enabled? are logs rotated?).
    
- **Kernel & system hardening** (firewall rules, SELinux/AppArmor, sysctl configs).
    

---

## 4. **Basic Commands**

### Install (Debian/Ubuntu example):
```
sudo apt install lynis
```

Run a full system audit:
```
sudo lynis audit system
```

- Output: A report showing **warnings, suggestions, and a hardening index**.
    
- Report is also saved in `/var/log/lynis.log`.

5. **Sample Output (Simplified)**
```
[+] Security Tests
------------------------------------
- Checking password file permissions [ OK ]
- Checking for password hashing method [ WARNING ]
   * Weak password hashing (MD5 detected)
- Checking firewall status [ SUGGESTION ]
   * Firewall not active, consider enabling ufw/iptables

[+] Hardening Index: 63 [of 100]
   Suggestions: 5 Warnings: 2
```

## 6. **How It Fits into Security Auditing**

- **Security Audit Process:**
    
    1. Collect data → Lynis scans system configs.
        
    2. Identify weaknesses → Warnings & suggestions.
        
    3. Compare with standards → PCI DSS requires strong encryption, Lynis checks SSL configs.
        
    4. Report findings → Generates actionable recommendations.
        
- **Example in a company:**
    
    - **Audit Finding (from Lynis):** Weak SSH configuration (root login enabled).
        
    - **Recommendation:** Disable root login via SSH (`PermitRootLogin no`).
        
    - **Business Impact:** Reduces risk of brute-force attacks.
        

---

## 7. **Advantages of Lynis**

✅ Lightweight, no agent required.  
✅ Works across many Unix/Linux flavors.  
✅ Provides **compliance checks**.  
✅ Regularly updated with new security tests.  
✅ Easy to integrate into **CI/CD pipelines** or scheduled audits.

---

## 8. **Real-World Use Case**

- A **payment gateway company** needs to be PCI DSS compliant.
    
- They run **Lynis weekly** to check:
    
    - Is cardholder data encrypted?
        
    - Is logging enabled and tamper-proof?
        
    - Are unnecessary services disabled?
        
- The results are shared with auditors to show **continuous compliance monitoring**.
    

---

✨ In short:  
**Lynis = Security Auditor for Linux/Unix systems**.  
It scans configs, highlights risks, checks compliance, and suggests hardening steps → making it a **key tool in security audits**.