## **Overview

Web Application Penetration Testing is a critical process in identifying and exploiting vulnerabilities within web applications to assess their security posture. This type of testing simulates real-world attacks to uncover weaknesses such as SQL Injection, Cross-Site Scripting (XSS), Local File Inclusion (LFI), and others that could be exploited by malicious actors. Penetration testers use a combination of automated tools and manual techniques to probe the application for vulnerabilities, validate their impact, and suggest mitigation strategies. By performing these tests, organizations can identify potential security flaws before they are exploited by attackers, ensuring the integrity and confidentiality of sensitive data and safeguarding the application from future threats.

This lab is designed to test your knowledge and skills in identifying web application vulnerabilities and uncovering hidden information on a target web server.

## **Task

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali Linux machine. The target website is accessible at **http://target.ine.local**.

**Objective**: Identify web application vulnerabilities in the target website and capture all the flags hidden within the environment.

**Useful wordlists:**

```
/usr/share/wordlists/dirb/common.txt 
/usr/share/seclists/Usernames/top-usernames-shortlist.txt 
/root/Desktop/wordlists/100-common-passwords.txt
```

**Flags to Capture:**

- **Flag 1:** Sometimes, important files are hidden in plain sight. Check the root ('/') directory for a file named 'flag.txt' that might hold the key to the first flag.
- **Flag 2:** Explore the structure of the server's directories. Enumeration might reveal hidden treasures.
- **Flag 3:** The login form seems a bit weak. Trying out different combinations might just reveal the next flag.
- **Flag 4:** The login form behaves oddly with unexpected inputs. Think of injection techniques to access the 'admin' account and find the flag.

# Tools

The best tools for this lab are:

- Nmap
- Gobuster
- Hydra

CHECKOUT : https://shagzz.medium.com/web-application-penetration-testing-ctf-1-1a3a64ea73bf

FOR ANSWERS......