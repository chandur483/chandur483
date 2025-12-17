```
ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 192.168.100.5  netmask 255.255.255.0  broadcast 192.168.100.255
        inet6 fe80::c3:d4ff:fe78:d54b  prefixlen 64  scopeid 0x20<link>
        ether 02:c3:d4:78:d5:4b  txqueuelen 1000  (Ethernet)
        RX packets 6144  bytes 441447 (431.1 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 13121  bytes 5873390 (5.6 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 11875  bytes 86680014 (82.6 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 11875  bytes 86680014 (82.6 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

my ip addr = 192.168.100.5

_______

## FIND LIVE HOSTS

```
sudo nmap -sn 192.168.100.0/24
Starting Nmap 7.92 ( https://nmap.org ) at 2025-10-17 17:06 IST
Nmap scan report for ip-192-168-100-1.ap-south-1.compute.internal (192.168.100.1)
Host is up (0.00017s latency).
MAC Address: 02:93:C9:AA:07:DB (Unknown)
Nmap scan report for ip-192-168-100-50.ap-south-1.compute.internal (192.168.100.50)
Host is up (0.00011s latency).
MAC Address: 02:11:42:53:83:1F (Unknown)
Nmap scan report for ip-192-168-100-51.ap-south-1.compute.internal (192.168.100.51)
Host is up (0.00014s latency).
MAC Address: 02:87:6E:A5:42:13 (Unknown)
Nmap scan report for ip-192-168-100-52.ap-south-1.compute.internal (192.168.100.52)
Host is up (0.00014s latency).
MAC Address: 02:DE:A6:E5:9D:E9 (Unknown)
Nmap scan report for ip-192-168-100-55.ap-south-1.compute.internal (192.168.100.55)
Host is up (0.00016s latency).
MAC Address: 02:BA:60:A7:6E:D9 (Unknown)
Nmap scan report for ip-192-168-100-63.ap-south-1.compute.internal (192.168.100.63)
Host is up (0.00014s latency).
MAC Address: 02:5B:3D:33:E1:EB (Unknown)
Nmap scan report for ip-192-168-100-67.ap-south-1.compute.internal (192.168.100.67)
Host is up (0.00014s latency).
MAC Address: 02:B3:26:D4:01:83 (Unknown)
Nmap scan report for ip-192-168-100-5.ap-south-1.compute.internal (192.168.100.5)
Host is up.
Nmap done: 256 IP addresses (8 hosts up) scanned in 1.83 seconds
```

includeing my ip there are 8 hosts.

## FPING 

reachable hosts 

```
sudo fping -a -g 192.168.100.1 192.168.100.254
192.168.100.1
192.168.100.5
192.168.100.50
192.168.100.51
192.168.100.52
192.168.100.55
192.168.100.67

```

this one seems odd

```
ping 192.168.100.63
PING 192.168.100.63 (192.168.100.63) 56(84) bytes of data.
```

this ip is live host but cant reachable 

_____________

run nmap on all live host

NMAP SCAM - ALL HOST
```
sudo nmap -Pn -sC -sV -O -vv -iL new_hosts.txt -oN all_nmap_scan.txt
Starting Nmap 7.92 ( https://nmap.org ) at 2025-10-17 17:17 IST
NSE: Loaded 155 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 17:17
Completed NSE at 17:17, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 17:17
Completed NSE at 17:17, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 17:17
Completed NSE at 17:17, 0.00s elapsed
Initiating ARP Ping Scan at 17:17
Scanning 7 hosts [1 port/host]
Completed ARP Ping Scan at 17:17, 0.05s elapsed (7 total hosts)
Initiating Parallel DNS resolution of 7 hosts. at 17:17
Completed Parallel DNS resolution of 7 hosts. at 17:17, 0.00s elapsed
Initiating SYN Stealth Scan at 17:17
Scanning 7 hosts [1000 ports/host]
Discovered open port 22/tcp on 192.168.100.67
Discovered open port 22/tcp on 192.168.100.52
Discovered open port 80/tcp on 192.168.100.52
Discovered open port 80/tcp on 192.168.100.51
Discovered open port 135/tcp on 192.168.100.51
Discovered open port 80/tcp on 192.168.100.50
Discovered open port 135/tcp on 192.168.100.50
Discovered open port 3389/tcp on 192.168.100.51
Discovered open port 445/tcp on 192.168.100.51
Discovered open port 3389/tcp on 192.168.100.50
Discovered open port 80/tcp on 192.168.100.55
Discovered open port 139/tcp on 192.168.100.51
Discovered open port 445/tcp on 192.168.100.50
Discovered open port 21/tcp on 192.168.100.51
Discovered open port 3389/tcp on 192.168.100.52
Discovered open port 3306/tcp on 192.168.100.52
Discovered open port 445/tcp on 192.168.100.52
Discovered open port 139/tcp on 192.168.100.52
Discovered open port 135/tcp on 192.168.100.55
Discovered open port 3389/tcp on 192.168.100.55
Discovered open port 139/tcp on 192.168.100.50
Discovered open port 445/tcp on 192.168.100.55
Discovered open port 139/tcp on 192.168.100.55
Discovered open port 21/tcp on 192.168.100.52
Discovered open port 49159/tcp on 192.168.100.51
Completed SYN Stealth Scan against 192.168.100.52 in 0.28s (6 hosts left)
Completed SYN Stealth Scan against 192.168.100.67 in 0.29s (5 hosts left)
Discovered open port 49155/tcp on 192.168.100.50
Discovered open port 49155/tcp on 192.168.100.51
Discovered open port 49154/tcp on 192.168.100.51
Discovered open port 49154/tcp on 192.168.100.50
Discovered open port 49152/tcp on 192.168.100.51
Discovered open port 49152/tcp on 192.168.100.50
Discovered open port 3389/tcp on 192.168.100.63
Discovered open port 49153/tcp on 192.168.100.51
Discovered open port 49153/tcp on 192.168.100.50
Completed SYN Stealth Scan against 192.168.100.55 in 4.59s (4 hosts left)
Completed SYN Stealth Scan against 192.168.100.51 in 4.67s (3 hosts left)
Discovered open port 49156/tcp on 192.168.100.50
Completed SYN Stealth Scan against 192.168.100.50 in 5.24s (2 hosts left)
Completed SYN Stealth Scan against 192.168.100.63 in 13.85s (1 host left)
Completed SYN Stealth Scan at 17:17, 13.88s elapsed (7000 total ports)
Initiating Service scan at 17:17
Scanning 35 services on 7 hosts
Completed Service scan at 17:18, 64.72s elapsed (35 services on 7 hosts)
Initiating OS detection (try #1) against 7 hosts
Retrying OS detection (try #2) against 7 hosts
Retrying OS detection (try #3) against 5 hosts
Retrying OS detection (try #4) against 5 hosts
Retrying OS detection (try #5) against 5 hosts
NSE: Script scanning 7 hosts.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 17:19
NSE: [ftp-bounce 192.168.100.52:21] PORT response: 500 Illegal PORT command.
NSE: [ftp-bounce 192.168.100.51:21] PORT response: 501 Server cannot accept argument.
Completed NSE at 17:19, 11.75s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 17:19
Completed NSE at 17:19, 0.29s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 17:19
Completed NSE at 17:19, 0.00s elapsed
Nmap scan report for ip-192-168-100-1.ap-south-1.compute.internal (192.168.100.1)
Host is up, received arp-response (0.00023s latency).
Scanned at 2025-10-17 17:17:38 IST for 104s
All 1000 scanned ports on ip-192-168-100-1.ap-south-1.compute.internal (192.168.100.1) are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
MAC Address: 02:93:C9:AA:07:DB (Unknown)
Too many fingerprints match this host to give specific OS details
TCP/IP fingerprint:
SCAN(V=7.92%E=4%D=10/17%OT=%CT=%CU=%PV=Y%DS=1%DC=D%G=N%M=0293C9%TM=68F22D42%P=x86_64-pc-linux-gnu)
SEQ(II=I)
U1(R=N)
IE(R=Y%DFI=N%TG=FF%CD=S)

Network Distance: 1 hop

Nmap scan report for ip-192-168-100-50.ap-south-1.compute.internal (192.168.100.50)
Host is up, received arp-response (0.00055s latency).
Scanned at 2025-10-17 17:17:38 IST for 104s
Not shown: 990 closed tcp ports (reset)
PORT      STATE SERVICE            REASON          VERSION
80/tcp    open  http               syn-ack ttl 128 Apache httpd 2.4.51 ((Win64) PHP/7.4.26)
|_http-server-header: Apache/2.4.51 (Win64) PHP/7.4.26
|_http-title: WAMPSERVER Homepage
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 79E32EEA338FA735AD22D36104C4337A
135/tcp   open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
139/tcp   open  netbios-ssn        syn-ack ttl 128 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       syn-ack ttl 128 Windows Server 2012 R2 Standard 9600 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server? syn-ack ttl 128
| rdp-ntlm-info: 
|   Target_Name: WINSERVER-01
|   NetBIOS_Domain_Name: WINSERVER-01
|   NetBIOS_Computer_Name: WINSERVER-01
|   DNS_Domain_Name: WINSERVER-01
|   DNS_Computer_Name: WINSERVER-01
|   Product_Version: 6.3.9600
|_  System_Time: 2025-10-17T11:49:10+00:00
| ssl-cert: Subject: commonName=WINSERVER-01
| Issuer: commonName=WINSERVER-01
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-10-16T11:23:28
| Not valid after:  2026-04-17T11:23:28
| MD5:   7c48 ba5d 84cb 9359 d079 d6fe e235 206c
| SHA-1: 43bf 34ce 1b92 e0c6 f76f 3d91 abec 292d 218b 776d
| -----BEGIN CERTIFICATE-----
| MIIC3DCCAcSgAwIBAgIQNTDRF76Be7BAWXM7O8UbZTANBgkqhkiG9w0BAQsFADAX
| MRUwEwYDVQQDEwxXSU5TRVJWRVItMDEwHhcNMjUxMDE2MTEyMzI4WhcNMjYwNDE3
| MTEyMzI4WjAXMRUwEwYDVQQDEwxXSU5TRVJWRVItMDEwggEiMA0GCSqGSIb3DQEB
| AQUAA4IBDwAwggEKAoIBAQC8fpFbEfPq3ZqOxuEd615ZuCsUUOXUDh2hFjIyoY0+
| vpZv1tmU9ym5mQxT/Urh2jFtp1nAIZvQhZuckYdXuorAozKsiqHfy0EFlbmX6Cj9
| yOdmC+Q+H8zKAkqbgjFN/FN8CjdTnrAI55Hu544YIZ0AlUzItvN2QdDyi9UenPtV
| 8hHOLVJ23FemyHr3+mblmUU0B2Lr9yUK2R9MUJL6OdeXWJrkqaO/ncYRFDHwhh0d
| LkVcYhDQc5rLzRrCNGVfQktCWdmsAJQWl/fjYw4ASJKR6E41gvyrD1eb+T5TXG62
| ePPtzoadRA7r8l95BsM5J6ZY48sO2SNeYilVILZs5uNTAgMBAAGjJDAiMBMGA1Ud
| JQQMMAoGCCsGAQUFBwMBMAsGA1UdDwQEAwIEMDANBgkqhkiG9w0BAQsFAAOCAQEA
| VZT547kjRxF9VnyTEOFSrzTgK+xjy9zaT99WAD6CsADTcf241HfbAzhErmzHywvp
| q2u0CzsVU9IUA7WgFLuVs9f5JxWylUHdO32RXadFsmfVb7aiDeGyN0FFwrPxWefB
| XQLbZKQUS0nJiS1UBpSLjezmEPHGmMWTZXRBMVLare5EQsRmNx8e2yQ/W40hA6NP
| oSeLgA71oZ2EFMpwWdq7BlrcUc55NgcZ4X+OhdXJ0t3OK5kwp9hXuSFD/I0fne90
| ScZKeR799/NP5dz6sDq+RKa+47HEaLo674KATefrwVlU2Q+Psv5KJ3M/mw3C/ujy
| Uw0wiQxmuaeJOpTilzcqGg==
|_-----END CERTIFICATE-----
|_ssl-date: 2025-10-17T11:49:21+00:00; -1s from scanner time.
49152/tcp open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
49153/tcp open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
49154/tcp open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
49155/tcp open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
49156/tcp open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
MAC Address: 02:11:42:53:83:1F (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=10/17%OT=80%CT=1%CU=31692%PV=Y%DS=1%DC=D%G=Y%M=021142%
OS:TM=68F22D42%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=10F%TI=I%CI=I%II=
OS:I%SS=S%TS=7)OPS(O1=M2301NW8ST11%O2=M2301NW8ST11%O3=M2301NW8NNT11%O4=M230
OS:1NW8ST11%O5=M2301NW8ST11%O6=M2301ST11)WIN(W1=2000%W2=2000%W3=2000%W4=200
OS:0%W5=2000%W6=2000)ECN(R=Y%DF=Y%T=80%W=2000%O=M2301NW8NNS%CC=Y%Q=)T1(R=Y%
OS:DF=Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=
OS:0%Q=)T3(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S
OS:=A%A=O%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R
OS:=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=
OS:AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%
OS:RUD=G)IE(R=Y%DFI=N%T=80%CD=Z)

Uptime guess: 0.019 days (since Fri Oct 17 16:51:57 2025)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=260 (Good luck!)
IP ID Sequence Generation: Incremental
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 28474/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 48827/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 40376/udp): CLEAN (Timeout)
|   Check 4 (port 25669/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 0s, deviation: 0s, median: 0s
| smb-os-discovery: 
|   OS: Windows Server 2012 R2 Standard 9600 (Windows Server 2012 R2 Standard 6.3)
|   OS CPE: cpe:/o:microsoft:windows_server_2012::-
|   Computer name: WINSERVER-01
|   NetBIOS computer name: WINSERVER-01\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2025-10-17T11:49:11+00:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| nbstat: NetBIOS name: WINSERVER-01, NetBIOS user: <unknown>, NetBIOS MAC: 02:11:42:53:83:1f (unknown)
| Names:
|   WINSERVER-01<00>     Flags: <unique><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WINSERVER-01<20>     Flags: <unique><active>
| Statistics:
|   02 11 42 53 83 1f 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00
| smb2-time: 
|   date: 2025-10-17T11:49:12
|_  start_date: 2025-10-17T11:23:14
| smb2-security-mode: 
|   3.0.2: 
|_    Message signing enabled but not required

Nmap scan report for ip-192-168-100-51.ap-south-1.compute.internal (192.168.100.51)
Host is up, received arp-response (0.00054s latency).
Scanned at 2025-10-17 17:17:38 IST for 104s
Not shown: 989 closed tcp ports (reset)
PORT      STATE SERVICE            REASON          VERSION
21/tcp    open  ftp                syn-ack ttl 128 Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 04-19-22  02:25AM       <DIR>          aspnet_client
| 04-19-22  01:19AM                 1400 cmdasp.aspx
| 04-19-22  12:17AM                99710 iis-85.png
| 04-19-22  12:17AM                  701 iisstart.htm
|_04-19-22  02:13AM                   22 robots.txt.txt
80/tcp    open  http               syn-ack ttl 128 Microsoft IIS httpd 8.5
|_http-server-header: Microsoft-IIS/8.5
| http-webdav-scan: 
|   WebDAV type: Unknown
|   Public Options: OPTIONS, TRACE, GET, HEAD, POST, PROPFIND, PROPPATCH, MKCOL, PUT, DELETE, COPY, MOVE, LOCK, UNLOCK
|   Server Date: Fri, 17 Oct 2025 11:49:10 GMT
|   Server Type: Microsoft-IIS/8.5
|   Allowed Methods: OPTIONS, TRACE, GET, HEAD, POST, COPY, PROPFIND, DELETE, MOVE, PROPPATCH, MKCOL, LOCK, UNLOCK
|   Directory Listing: 
|     http://ip-192-168-100-51.ap-south-1.compute.internal/
|     http://ip-192-168-100-51.ap-south-1.compute.internal/aspnet_client/
|     http://ip-192-168-100-51.ap-south-1.compute.internal/cmdasp.aspx
|     http://ip-192-168-100-51.ap-south-1.compute.internal/iis-85.png
|     http://ip-192-168-100-51.ap-south-1.compute.internal/iisstart.htm
|_    http://ip-192-168-100-51.ap-south-1.compute.internal/robots.txt.txt
|_http-svn-info: ERROR: Script execution failed (use -d to debug)
|_http-title: IIS Windows Server
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST COPY PROPFIND DELETE MOVE PROPPATCH MKCOL LOCK UNLOCK PUT
|_  Potentially risky methods: TRACE COPY PROPFIND DELETE MOVE PROPPATCH MKCOL LOCK UNLOCK PUT
135/tcp   open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
139/tcp   open  netbios-ssn        syn-ack ttl 128 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       syn-ack ttl 128 Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server? syn-ack ttl 128
| ssl-cert: Subject: commonName=WINSERVER-02
| Issuer: commonName=WINSERVER-02
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-10-16T11:23:08
| Not valid after:  2026-04-17T11:23:08
| MD5:   b42d caae 7b6e 77d2 91cb 477e 2249 0316
| SHA-1: c5a3 15bb 75dc c090 65dc 9576 e609 5ede 4c77 68ba
| -----BEGIN CERTIFICATE-----
| MIIC3DCCAcSgAwIBAgIQFSjBsxLD/o9AWjslJCoDdzANBgkqhkiG9w0BAQsFADAX
| MRUwEwYDVQQDEwxXSU5TRVJWRVItMDIwHhcNMjUxMDE2MTEyMzA4WhcNMjYwNDE3
| MTEyMzA4WjAXMRUwEwYDVQQDEwxXSU5TRVJWRVItMDIwggEiMA0GCSqGSIb3DQEB
| AQUAA4IBDwAwggEKAoIBAQCY1pc8V6jWMcejjuW22SpgS8Eo4Bnnw1zpqOzGvHiS
| yWvkhQDqz3TpkUDoTCDdz3XCT+rKlQej7pDN2XTOfpw8l8mkmmWkL0X+DGzLE16Q
| q60GHNJ2YHXqeOGckEgwpQL6IRZM0DAEnix6hAFtinuiOz5H0c1lcBHXTL4NupQu
| TTFBX7POTUQfUmyC4ajj10aj+FbEVQvAwOZrcHv+Zni4+6PkRJlH6/2JBMyJBpPK
| KzzL5pIx4hOLdluuVBJ6mLAEp2jqh61hyJEvWWZ3fhRncZNQwgO90HO/YcdyXVDy
| gZisgTfWdNzp5wL8NKxs+CJ/90ExIWT+gVpae9wkq9RVAgMBAAGjJDAiMBMGA1Ud
| JQQMMAoGCCsGAQUFBwMBMAsGA1UdDwQEAwIEMDANBgkqhkiG9w0BAQsFAAOCAQEA
| hddXIKJlKQxALsLk68/7H+0o79ulfjQTXkq/SOr27wBo1cLfPBWHF66eDnpYB2EL
| UTsSLqW9Jf/jJ1+t4z2ML1L1/LA0sYcX98JmLoA0QjtAxbXuD2dGhwUKRmiq27yW
| Tag/2eJ43phjuBGnp3auhrVc3/5HsSU4hCobUtiCRtw9YYN6UBYTmfAHTDaN9HYP
| JNcJjB0V8zzgiACAQLUycuh2yrTYYSb2kSCrQNoKdmYD0joKBcy03OepNpH3RD9w
| hJXwSgIVZb4G6AVxEBn353RW6mP4YoTs02SkMIWR0b8uOV6U2mlui45tRJhK27Y+
| uJAXOIW33tyY0ku/jg8Wow==
|_-----END CERTIFICATE-----
|_ssl-date: 2025-10-17T11:49:22+00:00; 0s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: WINSERVER-02
|   NetBIOS_Domain_Name: WINSERVER-02
|   NetBIOS_Computer_Name: WINSERVER-02
|   DNS_Domain_Name: WINSERVER-02
|   DNS_Computer_Name: WINSERVER-02
|   Product_Version: 6.3.9600
|_  System_Time: 2025-10-17T11:49:10+00:00
49152/tcp open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
49153/tcp open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
49154/tcp open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
49155/tcp open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
49159/tcp open  msrpc              syn-ack ttl 128 Microsoft Windows RPC
MAC Address: 02:87:6E:A5:42:13 (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=10/17%OT=21%CT=1%CU=37769%PV=Y%DS=1%DC=D%G=Y%M=02876E%
OS:TM=68F22D42%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=2%ISR=10C%TI=I%CI=I%II=
OS:I%SS=S%TS=7)OPS(O1=M2301NW8ST11%O2=M2301NW8ST11%O3=M2301NW8NNT11%O4=M230
OS:1NW8ST11%O5=M2301NW8ST11%O6=M2301ST11)WIN(W1=2000%W2=2000%W3=2000%W4=200
OS:0%W5=2000%W6=2000)ECN(R=Y%DF=Y%T=80%W=2000%O=M2301NW8NNS%CC=Y%Q=)T1(R=Y%
OS:DF=Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=
OS:0%Q=)T3(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S
OS:=A%A=O%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R
OS:=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=
OS:AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%
OS:RUD=G)IE(R=Y%DFI=N%T=80%CD=Z)

Uptime guess: 0.019 days (since Fri Oct 17 16:51:55 2025)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=257 (Good luck!)
IP ID Sequence Generation: Incremental
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 17469/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 29637/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 35584/udp): CLEAN (Timeout)
|   Check 4 (port 48677/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 0s, deviation: 0s, median: 0s
| nbstat: NetBIOS name: WINSERVER-02, NetBIOS user: <unknown>, NetBIOS MAC: 02:87:6e:a5:42:13 (unknown)
| Names:
|   WINSERVER-02<00>     Flags: <unique><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WINSERVER-02<20>     Flags: <unique><active>
| Statistics:
|   02 87 6e a5 42 13 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00
| smb-security-mode: 
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3.0.2: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2025-10-17T11:49:11
|_  start_date: 2025-10-17T11:22:58

Nmap scan report for ip-192-168-100-52.ap-south-1.compute.internal (192.168.100.52)
Host is up, received arp-response (0.00050s latency).
Scanned at 2025-10-17 17:17:38 IST for 104s
Not shown: 993 closed tcp ports (reset)
PORT     STATE SERVICE       REASON         VERSION
21/tcp   open  ftp           syn-ack ttl 64 vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 65534    65534         318 Apr 18  2022 updates.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.100.5
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp   open  ssh           syn-ack ttl 64 OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 bd:84:2e:fb:76:c2:ef:47:f3:54:c8:ab:b1:95:fa:ea (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC8wKOpa3u3XpKbmWWI/IfqWejosgl4qhfIuR2JX41LAvx7od9MMXsew1nYknvXi/+zoUhQN03OrN0J27yWUtB3Gsi7kvEIXj6UN1R5tAG1T/Q9LyTD55e0469+UzXxZamHA9VKCnWtreUGaAgTyIjuYk2hNiW/3+C4snaPnZz1HdLgyDxcsxRnwQ9Xfs/OT8humkrbAAms2Rc16oZckcIWy++tr2lzwJUgSxOpmvD6wT5eSsZsaDO73qWToPU4mjyMDEg6XxN2g1+0X9fSaAt3jJBECO9SRUVT0ys910QPPjrykvmGo2b1quqV0z53eeUQ3o+7I2Q6GdLsgJSrTTajZ8EwQvmTPSgh+c8MrEiIs8au00UdHyEUk8VIJq2N9XLh3P8+WQOnF0hcWHOMGkiv5KCna3BcYFf11+dIGdg9fa1dOWXZ+hYOtFqF5C88F/8li95AnOxG8fpr6EpcDWLT0Ojf1oDnHkHgj+NKr4brJPqVd7FDU2JjCZqrXosXrXc=
|   256 ef:82:f5:6c:6d:3b:8e:e1:b4:37:6f:66:98:94:87:76 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDNTrsfrr7fH+NIK/etCWyja1tBFM8H3bUDpPszU8nS584l4ijBguGI+aeke+fqkgNxu3NKYOQ2bNIaTHom7u9o=
|   256 f8:56:14:f8:83:f6:c2:13:43:ba:75:d0:92:bf:aa:f9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIavm0uxxAkPKFGQlujrgp0SbL+PLpeq33ZVqeBuXInk
80/tcp   open  http          syn-ack ttl 64 Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2018-02-21 17:28  drupal/
|_
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-title: Index of /
139/tcp  open  netbios-ssn   syn-ack ttl 64 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn   syn-ack ttl 64 Samba smbd 4.13.17-Ubuntu (workgroup: WORKGROUP)
3306/tcp open  mysql         syn-ack ttl 64 MySQL 5.5.5-10.3.34-MariaDB-0ubuntu0.20.04.1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.34-MariaDB-0ubuntu0.20.04.1
|   Thread ID: 37
|   Capabilities flags: 63486
|   Some Capabilities: LongColumnFlag, DontAllowDatabaseTableColumn, IgnoreSigpipes, Speaks41ProtocolOld, ConnectWithDatabase, SupportsTransactions, Support41Auth, InteractiveClient, ODBCClient, SupportsCompression, Speaks41ProtocolNew, SupportsLoadDataLocal, IgnoreSpaceBeforeParenthesis, FoundRows, SupportsAuthPlugins, SupportsMultipleStatments, SupportsMultipleResults
|   Status: Autocommit
|   Salt: I4e$}n0B;?k|PXJIq|=W
|_  Auth Plugin Name: mysql_native_password
3389/tcp open  ms-wbt-server syn-ack ttl 64 xrdp
MAC Address: 02:DE:A6:E5:9D:E9 (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=10/17%OT=21%CT=1%CU=36952%PV=Y%DS=1%DC=D%G=Y%M=02DEA6%
OS:TM=68F22D42%P=x86_64-pc-linux-gnu)SEQ(SP=FD%GCD=1%ISR=109%TI=Z%CI=Z%II=I
OS:%TS=A)OPS(O1=M2301ST11NW7%O2=M2301ST11NW7%O3=M2301NNT11NW7%O4=M2301ST11N
OS:W7%O5=M2301ST11NW7%O6=M2301ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F
OS:4B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M2301NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T
OS:=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R
OS:%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=
OS:40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0
OS:%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R
OS:=Y%DFI=N%T=40%CD=S)

Uptime guess: 30.975 days (since Tue Sep 16 17:55:33 2025)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=254 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: Host: IP-192-168-100-52; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.13.17-Ubuntu)
|   Computer name: ip-192-168-100-52
|   NetBIOS computer name: IP-192-168-100-52\x00
|   Domain name: ap-south-1.compute.internal
|   FQDN: ip-192-168-100-52.ap-south-1.compute.internal
|_  System time: 2025-10-17T11:49:12+00:00
|_clock-skew: mean: 1s, deviation: 0s, median: 0s
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2025-10-17T11:49:12
|_  start_date: N/A
| nbstat: NetBIOS name: IP-192-168-100-, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| Names:
|   IP-192-168-100-<00>  Flags: <unique><active>
|   IP-192-168-100-<03>  Flags: <unique><active>
|   IP-192-168-100-<20>  Flags: <unique><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WORKGROUP<1e>        Flags: <group><active>
| Statistics:
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 16654/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 23685/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 18648/udp): CLEAN (Failed to receive data)
|   Check 4 (port 54868/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked

Nmap scan report for ip-192-168-100-55.ap-south-1.compute.internal (192.168.100.55)
Host is up, received arp-response (0.00061s latency).
Scanned at 2025-10-17 17:17:38 IST for 104s
Not shown: 995 closed tcp ports (reset)
PORT     STATE SERVICE       REASON          VERSION
80/tcp   open  http          syn-ack ttl 128 Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
135/tcp  open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 128 Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  syn-ack ttl 128 Windows Server 2019 Datacenter 17763 microsoft-ds
3389/tcp open  ms-wbt-server syn-ack ttl 128 Microsoft Terminal Services
| ssl-cert: Subject: commonName=WINSERVER-03
| Issuer: commonName=WINSERVER-03
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-10-16T11:22:41
| Not valid after:  2026-04-17T11:22:41
| MD5:   7962 2689 9517 4b08 2efc 04b0 8ffd 8a82
| SHA-1: a6fd 8550 b22e 6b04 8dbc 1451 1c42 7020 c04a b611
| -----BEGIN CERTIFICATE-----
| MIIC3DCCAcSgAwIBAgIQFw5BFW8fELxBU6golABibTANBgkqhkiG9w0BAQsFADAX
| MRUwEwYDVQQDEwxXSU5TRVJWRVItMDMwHhcNMjUxMDE2MTEyMjQxWhcNMjYwNDE3
| MTEyMjQxWjAXMRUwEwYDVQQDEwxXSU5TRVJWRVItMDMwggEiMA0GCSqGSIb3DQEB
| AQUAA4IBDwAwggEKAoIBAQCxlYhPCcHetobBrs1cGoj4xhDqNLLc2hD/+oKGCPdO
| w+7bqr8uGRj/uac9arPxwgDxJU32hKy6nNljNCitOGGFkYyMdFxZEV7G5064usIc
| 8RYdgLjSww54VuOQY4sBqghe0l+OzXLw6BD7nrjTpauJYi+wRoj/rTglSZbRRQEB
| AJLU8f9C50YopRcEVEtz89TFH7QlG6vNVfDMD/S3Pzh7DnvNjYxbmOQZxwj10ZP0
| RUjWgHMrx02yWXHphzXwrvZgMoq0ncjhc+QkSKk8d06b0qeGcRFMEM3tFUFN46Id
| fHbyDxDvL6pDV1ptmWRt13za0GW0t0kcgVvuyMqXIyJRAgMBAAGjJDAiMBMGA1Ud
| JQQMMAoGCCsGAQUFBwMBMAsGA1UdDwQEAwIEMDANBgkqhkiG9w0BAQsFAAOCAQEA
| DHPxcuzG5Yli7sdPPw4pCA1haOZZJdSm7wtGHUCa1YOR7EKHbUqaeD8Qwjql1la0
| MnQtEp5ErWQiYSOaXSIQAg7avWLZaHe7wju5/KIMQe+nDTZK5ytuo6YzRFT7Oog0
| DgAH+5vWiyztfShmVe81y7D8TXjAT4cA6TEK93LaKj/EEvB+CJiiOx/dfvjMJprR
| //uQAl5InSRbZFn07PIoKjoRP6Cx6P9yiLJnx6gcsk2eWCnKNcfYxHDc5buznWcY
| mEo8efIEmo16fJZ77xueZH2zURACsYBBMcecr31kZfERlzoKQPBZeu8Zr8Y0t2az
| xAnAnCGjWME7gzqmqblItA==
|_-----END CERTIFICATE-----
| rdp-ntlm-info: 
|   Target_Name: WINSERVER-03
|   NetBIOS_Domain_Name: WINSERVER-03
|   NetBIOS_Computer_Name: WINSERVER-03
|   DNS_Domain_Name: WINSERVER-03
|   DNS_Computer_Name: WINSERVER-03
|   Product_Version: 10.0.17763
|_  System_Time: 2025-10-17T11:49:13+00:00
|_ssl-date: 2025-10-17T11:49:22+00:00; 0s from scanner time.
MAC Address: 02:BA:60:A7:6E:D9 (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=10/17%OT=80%CT=1%CU=35333%PV=Y%DS=1%DC=D%G=Y%M=02BA60%
OS:TM=68F22D42%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=109%TI=I%CI=I%II=
OS:I%SS=S%TS=U)OPS(O1=M2301NW8NNS%O2=M2301NW8NNS%O3=M2301NW8%O4=M2301NW8NNS
OS:%O5=M2301NW8NNS%O6=M2301NNS)WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%
OS:W6=FF70)ECN(R=Y%DF=Y%T=80%W=FFFF%O=M2301NW8NNS%CC=Y%Q=)T1(R=Y%DF=Y%T=80%
OS:S=O%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=
OS:Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R
OS:%O=%RD=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=
OS:80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0
OS:%Q=)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R
OS:=Y%DFI=N%T=80%CD=Z)

Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: Incremental
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows Server 2019 Datacenter 17763 (Windows Server 2019 Datacenter 6.3)
|   Computer name: WINSERVER-03
|   NetBIOS computer name: WINSERVER-03\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2025-10-17T11:49:12+00:00
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 25159/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 57939/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 55212/udp): CLEAN (Failed to receive data)
|   Check 4 (port 18048/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 0s, deviation: 0s, median: 0s
| nbstat: NetBIOS name: WINSERVER-03, NetBIOS user: <unknown>, NetBIOS MAC: 02:ba:60:a7:6e:d9 (unknown)
| Names:
|   WINSERVER-03<00>     Flags: <unique><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WINSERVER-03<20>     Flags: <unique><active>
|   WORKGROUP<1e>        Flags: <group><active>
|   WORKGROUP<1d>        Flags: <unique><active>
|   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
| Statistics:
|   02 ba 60 a7 6e d9 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00
| smb2-time: 
|   date: 2025-10-17T11:49:13
|_  start_date: N/A

Nmap scan report for ip-192-168-100-63.ap-south-1.compute.internal (192.168.100.63)
Host is up, received arp-response (0.00038s latency).
Scanned at 2025-10-17 17:17:38 IST for 104s
Not shown: 999 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
3389/tcp open  ms-wbt-server syn-ack ttl 128 Microsoft Terminal Services
| ssl-cert: Subject: commonName=EC2AMAZ-IK4QFED
| Issuer: commonName=EC2AMAZ-IK4QFED
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-10-16T11:21:25
| Not valid after:  2026-04-17T11:21:25
| MD5:   c19c 8018 1f93 a1e5 ff3a ef2f 4722 6d63
| SHA-1: b3ce 4dca 58d2 41f8 6f9c 3d37 fcf1 8441 a01f 6130
| -----BEGIN CERTIFICATE-----
| MIIC4jCCAcqgAwIBAgIQVo0b5TRg4bRAyTSfy1ayzjANBgkqhkiG9w0BAQsFADAa
| MRgwFgYDVQQDEw9FQzJBTUFaLUlLNFFGRUQwHhcNMjUxMDE2MTEyMTI1WhcNMjYw
| NDE3MTEyMTI1WjAaMRgwFgYDVQQDEw9FQzJBTUFaLUlLNFFGRUQwggEiMA0GCSqG
| SIb3DQEBAQUAA4IBDwAwggEKAoIBAQCYp3YdL1lZJsaTYP3z0Zwoo9+Jqtg05mqY
| iDTcbvT7kYJNvhasfupe2sDIf3fgipHGdv/oDY0jJZGbvrVYIRPGb3oev5ViHGni
| gp3SSRErH6J+cdCQyZ7aBUSKVqTLBopdf9ihVKPOdlqpaC/xZPY9upXJiMausV/8
| h3TlDCuJsy/UWVb8a3UzBnKJNHyjhABRNUp202nTkNiTdmftoUP9OCbIG2vv+syJ
| 68DAUr/ANZgiQyrTCWZdmtELZO/SZpguthBVBeReDAyQHbImNPb3w29bXTIsJ7EE
| AgE11I/K7JRcggkeABQXXbGRulUc+bpRr4Ahu25IWMcmpkFY0NOtAgMBAAGjJDAi
| MBMGA1UdJQQMMAoGCCsGAQUFBwMBMAsGA1UdDwQEAwIEMDANBgkqhkiG9w0BAQsF
| AAOCAQEARYz1diEUfflVzxq5o6YyfUsIA9tO7XiZaAieRMCY7coUVgnIq62i5ZDd
| XovqdHBt0sb47Ag/KddEe2CgqAK++2TsMHu5TOhv4r1NO2G2zl/CTDT+FbjsIgvR
| 27Pvt/JQUUkB9BJ2Q3SDlF1UO55wHmuXcw1im0VmRYYLKlt2s9nyTpldJYFPGi/N
| N1sCEr9DYbunSxBaGBkMMMME5pa6fO9Mjt0ga9r/pJ2guA0pEs8fSDsivTirSSb/
| zt6u7vNgUvQC+ekl6+tfK7hK96tw3oZTKbr4ZAwhizPm8Vh3V+JNCdncnOi18T+M
| xdqSVKtyJ9G+QTkXKfb3ovigCT4yPg==
|_-----END CERTIFICATE-----
|_ssl-date: 2025-10-17T11:49:22+00:00; 0s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: EC2AMAZ-IK4QFED
|   NetBIOS_Domain_Name: EC2AMAZ-IK4QFED
|   NetBIOS_Computer_Name: EC2AMAZ-IK4QFED
|   DNS_Domain_Name: EC2AMAZ-IK4QFED
|   DNS_Computer_Name: EC2AMAZ-IK4QFED
|   Product_Version: 10.0.14393
|_  System_Time: 2025-10-17T11:49:12+00:00
MAC Address: 02:5B:3D:33:E1:EB (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
No OS matches for host
TCP/IP fingerprint:
SCAN(V=7.92%E=4%D=10/17%OT=3389%CT=%CU=%PV=Y%DS=1%DC=D%G=N%M=025B3D%TM=68F22D42%P=x86_64-pc-linux-gnu)
SEQ(SP=105%GCD=2%ISR=101%TI=I%TS=A)
OPS(O1=M5B4NW0ST11%O2=M5B4NW0ST11%O3=M5B4NW0NNT11%O4=M5B4NW0ST11%O5=M5B4NW0ST11%O6=M5B4ST11)
WIN(W1=FA00%W2=FA00%W3=FA00%W4=FA00%W5=FA00%W6=FA00)
ECN(R=Y%DF=Y%TG=80%W=FA00%O=M5B4NW0NNS%CC=Y%Q=)
T1(R=Y%DF=Y%TG=80%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=N)
U1(R=N)
IE(R=N)

Uptime guess: 0.020 days (since Fri Oct 17 16:51:16 2025)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: Incremental
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 0s, deviation: 0s, median: 0s

Nmap scan report for ip-192-168-100-67.ap-south-1.compute.internal (192.168.100.67)
Host is up, received arp-response (0.00033s latency).
Scanned at 2025-10-17 17:17:38 IST for 104s
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 64 OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 fc:6e:ba:41:fe:d8:2a:87:92:39:89:cd:f2:67:37:80 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDik95z27M+Ksdm1iTf/0Xi0Ts84Q+GRV0fXvGcmfnPlV51ocPyM/t4SAQG04194T5B/G9wTPDx88yng/vwMFF0phZjTtWExzzya3wy8xaVakuOfCVE+0MPvwkqpPWLx3wzReq/1zIidl13DhTikM8UsJQpeZFWY6ZG2OIxyJKbdJSbiJ4rUu38m8VRibZtCt0RlofIZwt26ZCorxiDaQXz2uPmNSflO9lq3cD0/Gg59f6HQOfYz3MwE9sKH/q8Y4XJm+oylsUDEA0YLfr1DQGwvYRmBCwhryOC4oM8+MLomrqRqlkROT374kHS2rMSWGNb25uw+4VCzF+WBeQa3x1NeYdXgO/eGRQO5JBR5PujdeG+777COKibQI++p32F3lXR4iDpP9VxqrMC6LKTYg0srti2+KeGfWdINjdRe6RQ+yNiiDtq1zp8gr/GOGelxwhNuHMPEhnbUemjub4VLlxkNrYzjEGleBD4owkZ0OyhyHjwawKN001v1cHONHVjopk=
|   256 1a:2d:66:b3:2f:39:58:4d:24:fa:c4:f6:85:a5:ed:01 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPKlGOJF2auevk/Cz9y7OATb6HXc5Klrm8bqFpL8z8mGgaGGxIgw4uh7IvRrC65V+stEPWPtM015A2OvYlgpZYQ=
|   256 f7:c9:28:56:72:6f:2a:94:ef:84:00:ec:ab:6d:d5:41 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINXqUjgWIABFG2OIJU4YwZHIYgkY509aALLDhfrESOK+
MAC Address: 02:B3:26:D4:01:83 (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=10/17%OT=22%CT=1%CU=31903%PV=Y%DS=1%DC=D%G=Y%M=02B326%
OS:TM=68F22D42%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=5%ISR=109%TI=Z%CI=Z%II=
OS:I%TS=A)OPS(O1=M2301ST11NW6%O2=M2301ST11NW6%O3=M2301NNT11NW6%O4=M2301ST11
OS:NW6%O5=M2301ST11NW6%O6=M2301ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=
OS:F4B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M2301NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%
OS:T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=
OS:R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T
OS:=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=
OS:0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(
OS:R=Y%DFI=N%T=40%CD=S)

Uptime guess: 22.915 days (since Wed Sep 24 19:21:05 2025)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=259 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 17:19
Completed NSE at 17:19, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 17:19
Completed NSE at 17:19, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 17:19
Completed NSE at 17:19, 0.00s elapsed
Post-scan script results:
| clock-skew: 
|   0s: 
|     192.168.100.63 (ip-192-168-100-63.ap-south-1.compute.internal)
|     192.168.100.52 (ip-192-168-100-52.ap-south-1.compute.internal)
|     192.168.100.55 (ip-192-168-100-55.ap-south-1.compute.internal)
|     192.168.100.51 (ip-192-168-100-51.ap-south-1.compute.internal)
|_    192.168.100.50 (ip-192-168-100-50.ap-south-1.compute.internal)
Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 7 IP addresses (7 hosts up) scanned in 104.87 seconds
           Raw packets sent: 9666 (452.270KB) | Rcvd: 5419 (234.590KB)

```


192.168.100.50 - WINSERVER-01
192.168.100.51 - WINSERVER-02
192.168.100.55 - WINSERVER-03
