# Lab Environment

In this lab environment, you will use Nmap to identify ports used by Bind DNS, TFTP, and SNMP servers. No prior setup is required. Clear instructions are provided for scan and port identification. The target machine will be accessible at **demo.ine.local**.

**Objective:** This lab covers the process of performing port scanning and service detection with Nmap.

The objectives of this lab are to:

- Identify the port running a Bind DNS server.
- Identify the port running a TFTP server.
- Identify the port running the SNMP server.

# Tools

The best tools for this lab are:

- Nmap
_____________


we know target : demo.ine.local 

so  check open ports
```
nmap demo.ine.local -p- -T4 -Pn -A -sS
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-20 22:54 IST
Stats: 0:00:52 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.32% done; ETC: 22:54 (0:00:00 remaining)
Nmap scan report for demo.ine.local (192.134.204.3)
Host is up (0.000062s latency).
Not shown: 65534 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
177/tcp open  domain  ISC BIND 9.10.3-P4 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.10.3-P4-Ubuntu
MAC Address: 02:42:C0:86:CC:03 (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=9/20%OT=177%CT=1%CU=34557%PV=N%DS=1%DC=D%G=Y%M=0242
OS:C0%TM=68CEE366%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=107%TI=Z%CI=Z%
OS:TS=A)SEQ(SP=102%GCD=1%ISR=107%TI=Z%CI=Z%II=I%TS=A)OPS(O1=M5B4ST11NW7%O2=
OS:M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW7%O5=M5B4ST11NW7%O6=M5B4ST11)WI
OS:N(W1=7C70%W2=7C70%W3=7C70%W4=7C70%W5=7C70%W6=7C70)ECN(R=Y%DF=Y%T=40%W=7D
OS:78%O=M5B4NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3
OS:(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=
OS:Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=
OS:Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%R
OS:IPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.06 ms demo.ine.local (192.134.204.3)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 52.90 seconds
```

**we found 117 BLIND

**COMPLETED : Identify the port running a Bind DNS server.

Now we need to find remaining two 

- Identify the port running a TFTP server.
- Identify the port running the SNMP server.

```
sudo nmap demo.ine.local -p1-250 -sU
```

- `-sU` = UDP scan.
    
- UDP is **stateless**: no handshake, just send a probe.
    
- If the host replies with **ICMP Port Unreachable → CLOSED**.
    
- If the host replies with **nothing** (or firewall blocks), Nmap marks it as **open|filtered**.

```
sudo nmap demo.ine.local -p1-250 -sU                                                                                                                                                   
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-20 23:02 IST
Stats: 0:00:37 elapsed; 0 hosts completed (1 up), 1 undergoing UDP Scan
UDP Scan Timing: About 19.16% done; ETC: 23:05 (0:02:40 remaining)
Stats: 0:03:23 elapsed; 0 hosts completed (1 up), 1 undergoing UDP Scan
UDP Scan Timing: About 81.32% done; ETC: 23:06 (0:00:47 remaining)
Nmap scan report for demo.ine.local (192.134.204.3)
Host is up (0.00012s latency).
Not shown: 247 closed udp ports (port-unreach)
PORT    STATE         SERVICE
134/udp open|filtered ingres-net
177/udp open|filtered xdmcp
234/udp open|filtered unknown
MAC Address: 02:42:C0:86:CC:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 267.02 seconds
```

In this we already know port 177 
So check remaining port servie version

```
 sudo nmap demo.ine.local -p134,234 -sUV                                                                                                                                           
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-20 23:19 IST
Stats: 0:00:34 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 0.00% done
Stats: 0:00:39 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 0.00% done
Stats: 0:01:09 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 50.00% done; ETC: 23:21 (0:01:07 remaining)
Nmap scan report for demo.ine.local (192.134.204.3)
Host is up (0.000053s latency).

PORT    STATE         SERVICE    VERSION
134/udp open|filtered ingres-net
234/udp open          snmp       SNMPv1 server; net-snmp SNMPv3 server (public)
MAC Address: 02:42:C0:86:CC:03 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 100.29 seconds
```

Now we know Port 234 SNMP

so check Port 134

use NSE scripts

so i we didnt find any thing useful so one way to test is port 134 tftp

```
┌──(root㉿INE)-[~]
└─# tftp demo.ine.local 134
tftp> 
tftp> 
```

so now we know port 134 is tftp.

____________

## **INE METHOD
**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/e4a9ea22bfb761b7a7ac8b61376834d15b12612dee72976eacd7914fbec02ce7.png)

**Step 2:** Check if the target machine is reachable:

**Command:**

```
ping -c 4 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/89e5e0951fbfaf6b53b6cc1a4ec1a21e49bc18929c96e8c807cd41281e57b77a.png)

The target is reachable.

**Step 3:** Port scanning with Nmap

To begin with, we can perform an Nmap port scan on the target system to identify whether the BIND DNS server is open. This can be done by running the following command:

**Command:**

```
nmap demo.ine.local -p 177 -A
```

As shown in the following screenshot, the DNS BIND server is running on port 177.

![Content Image](https://assets.ine.com/lab/learningpath/de7cc4ee466eaf6085d937e6c6da148da146f5ce3e03a74e5f976398ebb276cb.png)

We can now perform a UDP port scan on the port range 1-250, this can be done by running the following command:

**Command:**

```
nmap demo.ine.local -p 1-250 -sU
```

As shown in the following screenshot, the Nmap scan reveals that the target system has 3 open UDP ports.

![Content Image](https://assets.ine.com/lab/learningpath/9e4ba8cfe1fc785d6de0d58ff1b52ec7ed61797e3c3f470f1d5b8c6a115b63bd.png)

**Step 4:** Service detection with Nmap

Now that we have identified the open UDP ports on the target, we can learn more about the services running on the open ports by performing a service detection scan with Nmap.

This can be done by running the following command:

**Command:**

```
nmap demo.ine.local -p 134,177,234 -sUV
```

As shown in the following screenshot, the Nmap service detection scan reveals the names and versions of the services running on the open UDP ports on the target system.

![Content Image](https://assets.ine.com/lab/learningpath/a0060c2164eacbd52e754c13e50ef3c6ae455fa68495c352a8cc971ee33e1ef4.png)

The Nmap scan reveals the services running on ports 177 and 234, but not 134. We can perform an Nmap script scan to enumerate information from port 134 by running the following command:

**Command:**

```
nmap demo.ine.local -p 134 -sUV --script=discovery
```

As shown in the following screenshot, the Nmap script scan does not reveal any useful information regarding the service running on port 134.

![Content Image](https://assets.ine.com/lab/learningpath/c9bc885419b806a4f7b0ab932e1b347c93c32af147ab43ce69d3793e21b4536a.png)

Given that we have discovered that UDP ports 177 and 234 are running a DNS and SNMP server respectively, we can assume that port 134 is running the TFTP server.

We can confirm this by running the following command:

**Command:**

```
tftp demo.ine.local 134 
```

As shown in the following screenshot, the authentication with the TFTP server is successful and we are provided with an FTP console.

![Content Image](https://assets.ine.com/lab/learningpath/e875c20cf235e62da1d06b7937a718eef1018803ae5e15d9cdac0410783e37d2.png)

We have successfully been able to identify the ports running the BIND DNS server, SNMP server and TFTP server.

# Conclusion

In this lab, we explored the process of performing port scanning and service detection with Nmap.