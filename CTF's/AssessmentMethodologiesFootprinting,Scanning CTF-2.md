Reconnaissance is the initial phase of a penetration testing process where information about a target system is gathered to identify potential vulnerabilities. This phase involves actively or passively collecting data such as server headers, open ports, exposed directories, and system configurations. Techniques like scanning, querying DNS records, examining web application files (e.g., robots.txt), and analyzing response headers help uncover critical information that can aid in later exploitation phases. Effective reconnaissance allows testers to map the attack surface, prioritize targets, and plan their approach with minimal detection by the system's defenses.

This lab is designed to test your knowledge and skills in performing reconnaissance and identifying hidden information on a target web server.

__________
### Completing Skill Check Labs

Skill Check Labs are interactive, hands-on exercises designed to validate the knowledge and skills you’ve gained in this course through real-world scenarios. Each lab presents practical tasks that require you to apply what you’ve learned. Unlike other INE labs, solutions are not provided, challenging you to demonstrate your understanding and problem-solving abilities. Your performance is graded, allowing you to track progress and measure skill growth over time.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali Linux machine. The target machine will be accessible at **http://target.ine.local**.

**Objective:** Perform reconnaissance on the target and capture all the flags hidden within the environment.

**Flags to Capture:**

- **Flag 1**: The server proudly announces its identity in every response. Look closely; you might find something unusual.
- **Flag 2**: The gatekeeper's instructions often reveal what should remain unseen. Don't forget to read between the lines.
- **Flag 3**: Anonymous access sometimes leads to forgotten treasures. Connect and explore the directory; you might stumble upon something valuable.
- **Flag 4**: A well-named database can be quite revealing. Peek at the configurations to discover the hidden treasure.

# Tools

The best tools for this lab are:

- Nmap
- FTP
- MySQL

---

### Note

In this lab, the flag will follow the format: FLAG1_MD5Hash. For example, FLAG1_0f4d0db3668dd58cabb9eb409657eaa8. You need to submit only the MD5 hash string, excluding the underscore (_). For instance: 0f4d0db3668dd58cabb9eb409657eaa8.
_______________

**Flag 1**: The server proudly announces its identity in every response. Look closely; you might find something unusual.

means : "they probably put the **flag** (or part of it) in a banner/header/banner-like field that the server emits every time — most likely the **HTTP `Server` header** (or another response header / protocol banner)."

so use any tools to get banner eg: nmap , curl

i use curl :
```
curl -s -i http://target.ine.local
HTTP/1.1 200 OK
Server: Werkzeug/3.0.6 Python/3.10.12
Date: Sun, 21 Sep 2025 05:40:10 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 2557
Server: FLAG1_b7be074271e645d4bb8a23989fd8d674
Connection: close

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="shortcut icon" href="#">
    <title>CTF Challenge</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #1c1c1c;
            color: #fff;
        }
```

OR

use nmap:
```
sudo nmap -sC -sV -A http://target.ine.local
```

FLAG1_b7be074271e645d4bb8a23989fd8d674

________

**Flag 2**: The gatekeeper's instructions often reveal what should remain unseen. Don't forget to read between the lines.

MEANS:
- **“Gatekeeper’s instructions”**  
    → Usually means _some sort of file or banner that tells you what’s allowed or disallowed_.  
    Classic examples in CTFs:
    
    - **`robots.txt`** → tells search engines what _not_ to index (often hides juicy paths).
        
    - **`.htaccess` / `.gitignore` / README / instructions.txt** → “rules” files left around.
        
    - **FTP/SSH/MOTD banners** that say “Authorized users only” but may leak info.
        
- **“Often reveal what should remain unseen”**  
    → Those files are meant to “hide” or “restrict,” but in CTFs they ironically **expose secret directories, hidden flags, or credentials**.


so http://target.ine.local/robots.txt

```
User-agent: googlebot 
Disallow: /photos

User-agent: *
Disallow: /secret-info/
Disallow: /data/
```

so /secret-info , /data not allowed

http://target.ine.local/secret-info/
```
0	"flag.txt"
```

FLAG2_3f5213f9b8a341078a85de28362a1599

_____________

**Flag 3**: Anonymous access sometimes leads to forgotten treasures. Connect and explore the directory; you might stumble upon something valuable.

# What it’s referencing

“Anonymous access sometimes leads to forgotten treasures. Connect and explore the directory; you might stumble upon something valuable.”  
→ Check services that commonly allow anonymous/guest access and often contain leftover files: **FTP (anonymous), SMB/Samba guest shares, WebDAV, public S3 buckets, NFS exports, anonymous SMTP/IMAP, and open Git repos or file-hosting endpoints.**

**check all active open port in 192.191.214.3
```
sudo nmap -sC -A -O -vv -T3 192.191.214.3
```

```
└─# sudo nmap -sC -A -O -vv -T3 192.191.214.3
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-21 11:41 IST
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 11:41
Completed NSE at 11:41, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 11:41
Completed NSE at 11:41, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 11:41
Completed NSE at 11:41, 0.00s elapsed
Initiating ARP Ping Scan at 11:41
Scanning 192.191.214.3 [1 port]
Completed ARP Ping Scan at 11:41, 0.05s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 11:41
Scanning target.ine.local (192.191.214.3) [1000 ports]
Discovered open port 3306/tcp on 192.191.214.3
Discovered open port 25/tcp on 192.191.214.3
Discovered open port 22/tcp on 192.191.214.3
Discovered open port 80/tcp on 192.191.214.3
Discovered open port 21/tcp on 192.191.214.3
Discovered open port 143/tcp on 192.191.214.3
Discovered open port 993/tcp on 192.191.214.3
Completed SYN Stealth Scan at 11:41, 0.04s elapsed (1000 total ports)
Initiating Service scan at 11:41
Scanning 7 services on target.ine.local (192.191.214.3)
Completed Service scan at 11:43, 86.18s elapsed (7 services on 1 host)
Initiating OS detection (try #1) against target.ine.local (192.191.214.3)
Retrying OS detection (try #2) against target.ine.local (192.191.214.3)
Retrying OS detection (try #3) against target.ine.local (192.191.214.3)
Retrying OS detection (try #4) against target.ine.local (192.191.214.3)
Retrying OS detection (try #5) against target.ine.local (192.191.214.3)
NSE: Script scanning 192.191.214.3.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 11:43
NSE: [ftp-bounce 192.191.214.3:21] Couldn't resolve scanme.nmap.org, scanning 10.0.0.1 instead.
NSE: [ftp-bounce 192.191.214.3:21] PORT response: 500 Illegal PORT command.
Completed NSE at 11:43, 0.34s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 11:43
Completed NSE at 11:43, 2.69s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 11:43
Completed NSE at 11:43, 0.00s elapsed
Nmap scan report for target.ine.local (192.191.214.3)
Host is up, received arp-response (0.000056s latency).
Scanned at 2025-09-21 11:41:50 IST for 100s
Not shown: 993 closed tcp ports (reset)
PORT     STATE SERVICE  REASON         VERSION
21/tcp   open  ftp      syn-ack ttl 64 vsftpd 3.0.5
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.191.214.2
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 0        0              22 Oct 28  2024 creds.txt
|_-rw-r--r--    1 0        0              39 Sep 21 05:36 flag.txt

```

now we know ftp have anonymoous login


```
ftp anonymous@192.191.214.2
```

```
└─# ftp anonymous@192.191.214.3
Connected to 192.191.214.3.
220 (vsFTPd 3.0.5)
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||55874|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              22 Oct 28  2024 creds.txt
-rw-r--r--    1 0        0              39 Sep 21 05:36 flag.txt
226 Directory send OK.
ftp> cat flag.txt
?Invalid command.
ftp> get flag.txt
local: flag.txt remote: flag.txt
229 Entering Extended Passive Mode (|||25763|)
150 Opening BINARY mode data connection for flag.txt (39 bytes).
100% |*********************************************************************************************************************************************|    39      140.02 KiB/s    00:00 ETA
226 Transfer complete.
39 bytes received in 00:00 (64.00 KiB/s)
ftp> get cred.txt
local: cred.txt remote: cred.txt
229 Entering Extended Passive Mode (|||19086|)
550 Failed to open file.
ftp> 
ftp> get creds.txt
local: creds.txt remote: creds.txt
229 Entering Extended Passive Mode (|||12394|)
150 Opening BINARY mode data connection for creds.txt (22 bytes).
100% |*********************************************************************************************************************************************|    22      294.30 KiB/s    00:00 ETA
226 Transfer complete.
22 bytes received in 00:00 (63.18 KiB/s)
ftp> exit
221 Goodbye.
```

now we have flag3 and creds.txt

```
┌──(root㉿INE)-[~]
└─# cat flag.txt                                                                                                                                                                          
FLAG3_c4b92c919e0849a9b21cab15eb471395

┌──(root㉿INE)-[~]
└─# cat creds.txt                                                                                                                                                                         
db_admin:password@123
```

FLAG3_c4b92c919e0849a9b21cab15eb471395

_____________

**Flag 4**: A well-named database can be quite revealing. Peek at the configurations to discover the hidden treasure.

we know 
```
3306/tcp open  mysql    syn-ack ttl 64 MySQL 8.0.39-0ubuntu0.22.04.1
|_ssl-date: TLS randomness does not represent time
```

so use creds.txt to mysql

```
mysql -h 192.191.214.3 -u db_admin -p
```

```
└─# mysql -h 192.191.214.3 -u db_admin -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 23
Server version: 8.0.39-0ubuntu0.22.04.1 (Ubuntu)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Support MariaDB developers by giving a star at https://github.com/MariaDB/server
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> show databases
    -> ;
+----------------------------------------+
| Database                               |
+----------------------------------------+
| FLAG4_be70bff2c32d42b182b6eb3b533991e7 |
| information_schema                     |
| mysql                                  |
| performance_schema                     |
| sys                                    |
+----------------------------------------+
5 rows in set (0.002 sec)

MySQL [(none)]> 

```

FLAG4_be70bff2c32d42b182b6eb3b533991e7
