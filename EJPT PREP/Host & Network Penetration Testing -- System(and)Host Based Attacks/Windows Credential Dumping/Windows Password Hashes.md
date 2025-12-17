The Windows OS stores hashed user account passwords locally in the `SAM (Security Accounts Manager) database`

Hashing is the process of converting a piece of data into another value. A hashing function or algorithm is used to generate the new value. The result of a hashing algorithm is known as a hash or hash value.

Authentication and verification of user credentials is facilitated by the Local Security Authority (LSA).

Windows versions up to Windows Server 2003 utilize two different types of hashes: 
+ LM 
+ NTLM 
+ Windows disables LM hashing and utilizes NTLM hashing from Windows Vista onwards

## **LSASS (Local Security Authority Subsystem Service)**

- Process: `lsass.exe`
    
- Role: **Authentication & Security Authority**
    
- Responsible for:
    
    - Verifying logon attempts (local & remote).
        
    - Creating **access tokens** after successful logon.
        
    - Managing password changes and enforcing security policies.
        
    - Storing credentials in memory (this is why attackers target LSASS to dump hashes/tickets).
        

Think of LSASS as the **security gatekeeper** that decides _who you are_ and _what token you get_.

---

## **SAM (Security Account Manager)**

- Database file: `%SystemRoot%\System32\config\SAM`
    
- Role: **User Accounts & Credentials Storage**
    
- Responsible for:
    
    - Storing **local user accounts and password hashes** (encrypted).
        
    - Part of the Windows Registry hive.
        
    - Protected by the system so only LSASS (and SYSTEM) can access it normally.
        

Think of SAM as the **secure vault** that holds all the **local account usernames + password hashes**.

---

### **Key Difference**

- **SAM = Storage (where credentials live)**
    
- **LSASS = Enforcer (uses those credentials to authenticate, create tokens, and apply security rules)**

