
# **Example: Sequential Approach – SecurePayments Inc.**

**Company:** SecurePayments Inc.  
**Business:** Processes credit card payments → must follow **PCI DSS**.  
**Approach:** First a **Security Audit**, then a **Penetration Test**.

---

## **Step 1 – Security Audit (Independent Firm)**

🔎 The audit found:

1. **Weak Encryption** → Cardholder data not strongly protected in transit.
    
2. **Weak Network Security** → Poor traffic monitoring and controls.
    
3. **Weak Access Controls** → Employees had too many permissions.
    
4. **Outdated Incident Response Plan** → Team not ready for breaches.
    

✅ Recommendations:

- Use **strong encryption** (TLS).
    
- Fix access controls → **least privilege**.
    
- Update and test incident response procedures.
    
- Improve **network monitoring**.
    

📌 SecurePayments Inc. applied these fixes.

---

## **Step 2 – Penetration Test (Your Role)**

🎯 **Goal:** Test if the **new technical fixes** are effective.

---

### **Phase 1: Planning & Preparation**

- Scope = Cardholder Data Environment (CDE).
    
- Review **network diagrams** + PCI DSS documents.
    
- Define **targets**: network + applications.
    

---

### **Phase 2: Information Gathering**

- Check updated **policies**: encryption, access control, incident response.
    
- Review the **audit report** for weak spots.
    

---

### **Phase 3: Penetration Test Execution**

- Run **network scans** and **vulnerability assessments**.
    
- Try **exploiting weaknesses** in systems.
    
- Test **new encryption** and **access control rules**.
    

---

### **Phase 4: Findings**

❌ Discovered new issues:

1. **Exposed admin interface** (could be accessed without proper security).
    
2. **SQL injection** in a customer web app.
    

✅ Recommendations:

- Lock down the admin interface (stronger authentication + IP restrictions).
    
- Patch SQL injection and do a **secure code review**.
    

---

## **Step 3 – Results (Sequential Approach Summary)**

- **Security Audit (Step 1):**
    
    - Found compliance/policy issues.
        
    - Recommended fixes for encryption, access, monitoring, IR plan.
        
- **Penetration Test (Step 2):**
    
    - Tested the **actual technical defenses**.
        
    - Found real exploitable vulnerabilities (admin interface + SQLi).
        

---

# 🔄 **Why Sequential Approach Works**

- **Audit** = “Paper & policy check” → finds **gaps in rules and processes**.
    
- **Pentest** = “Real-world hacking test” → finds **technical flaws attackers can exploit**.
    
- Together = A **complete security picture** (policy + technology).
    

---

✅ **Analogy:**

- Security Audit = Doctor checks your medical history, blood tests, and lifestyle.
    
- Penetration Test = Doctor actually runs a **stress test on a treadmill** to see if your heart holds up in real conditions.
    
- Both together = Full health check-up.