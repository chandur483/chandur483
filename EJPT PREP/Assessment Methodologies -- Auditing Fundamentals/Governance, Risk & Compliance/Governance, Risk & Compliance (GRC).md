# **Governance, Risk & Compliance (GRC)**

### 1. **Governance** 🏛️

👉 Framework of **policies, procedures, and practices** that guide how an organization is run securely.

**Key components:**

- **Policy Development:** Write rules (e.g., password policy requiring MFA).
    
- **Roles & Responsibilities:** Define who does what (e.g., Security Officer manages audits, IT Admin applies patches).
    
- **Accountability:** Make sure people are answerable (e.g., CEO signs off on compliance reports).
    

**Example:**  
At a bank, governance ensures there are **clear policies for handling customer data**, managers are **responsible for enforcing rules**, and **audits check accountability**.

---

### 2. **Risk** ⚠️

👉 Process of **identifying, analyzing, and reducing risks** to protect assets.

**Key components:**

- **Risk Identification:** Spot threats (e.g., phishing, ransomware, insider abuse).
    
- **Risk Assessment:** Estimate **likelihood + impact** (e.g., phishing is highly likely + high impact).
    
- **Risk Mitigation:** Apply controls (e.g., email filtering, awareness training, incident response drills).
    

**Example:**  
An e-commerce site identifies that **SQL Injection** is a high-risk threat → they test their apps, patch code, and monitor logs to reduce risk.

---

### 3. **Compliance** ✅

👉 Ensuring the company **follows laws, regulations, and standards**.

**Key components:**

- **Regulatory Requirements:** GDPR (EU data privacy), HIPAA (healthcare), PCI DSS (payment security).
    
- **Internal Policies:** Follow company’s own rules.
    
- **Audits & Assessments:** Check regularly if rules are followed.
    

**Example:**  
A hospital must follow **HIPAA** → ensure patient records are encrypted, staff access is logged, and third-party vendors comply.

---

# 🧩 **Why GRC Matters in Penetration Testing**

1. **Comprehensive Security Assessment:**  
    A pentester who knows GRC doesn’t just hack → they check if weaknesses also **violate policies or regulations**.
    
    - Example: If a system stores credit card data without encryption, it’s not only a vulnerability → it’s a **PCI DSS violation**.
        
2. **Enhanced Reporting:**  
    Pentest findings can be framed in GRC terms.
    
    - Instead of just saying _“SQL Injection found”_, you say:  
        _“SQL Injection in payment system → violates PCI DSS 6.5.1 (secure coding practices).”_
        
3. **Strategic Recommendations:**  
    Reports are more valuable when they tie fixes to **business risk & compliance**.
    
    - Example: _“Implement MFA → reduces risk of credential theft AND supports compliance with NIST guidelines.”_
        

---

# 🏢 **Real-World Example of GRC + Pentesting**

Imagine **SecureBank Ltd.**

- **Governance:** They have a **policy** requiring customer data encryption, with **IT managers accountable** for compliance.
    
- **Risk:** Their risk assessment shows online banking apps are a top target → penetration testing is prioritized.
    
- **Compliance:** As a financial institution, they must follow **PCI DSS** and **local banking laws**.
    

When you pentest their system and find **unencrypted API responses leaking card data**:

- It’s a **risk** (attackers could steal data).
    
- It breaks **governance policy** (violates encryption rule).
    
- It violates **compliance** (PCI DSS).