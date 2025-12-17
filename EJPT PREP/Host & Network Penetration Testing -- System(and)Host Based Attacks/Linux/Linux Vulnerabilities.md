## 📌 Frequently Exploited Linux Services

| **Protocol / Service**           | **Port(s)**                | **Purpose**                                                                                               | **Common Exploits / Risks**                                                                                                                                                     |
| -------------------------------- | -------------------------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Apache Web Server**            | TCP 80 (HTTP), 443 (HTTPS) | Popular open-source web server for hosting websites and web apps. Powers a large portion of the internet. | Vulnerabilities in outdated Apache versions, web application misconfigurations, directory traversal, RCE (Remote Code Execution), and misconfigured modules (e.g., mod_php).    |
| **SSH (Secure Shell)**           | TCP 22                     | Secure remote login and command execution over an encrypted channel. Replacement for Telnet.              | Brute-force attacks on weak passwords, key-based authentication theft, outdated SSH versions with CVEs, misconfigured root login.                                               |
| **FTP (File Transfer Protocol)** | TCP 21                     | Transfers files between client and server. Still widely used in legacy systems.                           | Cleartext transmission (usernames & passwords can be sniffed), anonymous login, buffer overflow exploits, misconfigured permissions allowing directory traversal or data theft. |
| **Samba (SMB Implementation)**   | TCP 445                    | Linux implementation of SMB/CIFS protocol to share files, printers, and devices with Windows systems.     | EternalBlue-like vulnerabilities, misconfigured shares allowing unauthenticated access, sensitive file exposure, privilege escalation.                                          |
|                                  |                            |                                                                                                           |                                                                                                                                                                                 |


**✅ **Key Takeaway for Pentesters:**

- Always enumerate open ports and services (e.g., using `nmap`).
    
- Check for outdated versions and weak configurations.
    
- Look for default credentials, anonymous access, or misconfigured permissions.
    
- Services like **SSH, FTP, Apache, and Samba** are high-value targets because they are so common and often expose sensitive data or remote access.**


