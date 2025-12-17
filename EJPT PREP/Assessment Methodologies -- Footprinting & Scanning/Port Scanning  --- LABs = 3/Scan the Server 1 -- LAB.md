# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine will be accessible at **demo.ine.local**.

**Objective:** This lab covers the process of performing port scanning and service detection with Nmap.

# Tools

The best tools for this lab are:

- Nmap

## **MY METHOD 1

first this is internal penteset so find ip adderess
```
ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.1.0.8  netmask 255.255.0.0  broadcast 10.1.255.255
        ether 02:42:0a:01:00:08  txqueuelen 0  (Ethernet)
        RX packets 928  bytes 98113 (95.8 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 794  bytes 1907172 (1.8 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.94.84.2  netmask 255.255.255.0  broadcast 192.94.84.255
        ether 02:42:c0:5e:54:02  txqueuelen 0  (Ethernet)
        RX packets 16  bytes 1376 (1.3 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 2150  bytes 2788376 (2.6 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2150  bytes 2788376 (2.6 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

now we found ip so check active hosts
```
sudo nmap -sn 192.94.84.2
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-20 22:39 IST
Nmap scan report for INE (192.94.84.2)
Host is up.
Nmap done: 1 IP address (1 host up) scanned in 0.00 seconds

┌──(root㉿INE)-[~]
└─# sudo nmap -sn 192.94.84.0/24
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-20 22:39 IST
Nmap scan report for 192.94.84.1
Host is up (0.000045s latency).
MAC Address: 02:42:6C:CF:E9:33 (Unknown)
Nmap scan report for demo.ine.local (192.94.84.3)
Host is up (0.000025s latency).
MAC Address: 02:42:C0:5E:54:03 (Unknown)
Nmap scan report for INE (192.94.84.2)
Host is up.
Nmap done: 256 IP addresses (3 hosts up) scanned in 1.98 seconds
```

now we have to find OS and sevice versions 
```
sudo nmap -Pn -sS -vv -A -T4 -p- demo.ine.local                                                                                                                                        
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-20 22:42 IST
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 22:42
Completed NSE at 22:42, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 22:42
Completed NSE at 22:42, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 22:42
Completed NSE at 22:42, 0.00s elapsed
Initiating ARP Ping Scan at 22:42
Scanning demo.ine.local (192.94.84.3) [1 port]
Completed ARP Ping Scan at 22:42, 0.04s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 22:42
Scanning demo.ine.local (192.94.84.3) [65535 ports]
Discovered open port 6421/tcp on 192.94.84.3
Discovered open port 55413/tcp on 192.94.84.3
Discovered open port 41288/tcp on 192.94.84.3
Completed SYN Stealth Scan at 22:42, 2.33s elapsed (65535 total ports)
Initiating Service scan at 22:42
Scanning 3 services on demo.ine.local (192.94.84.3)
Completed Service scan at 22:42, 11.03s elapsed (3 services on 1 host)
Initiating OS detection (try #1) against demo.ine.local (192.94.84.3)
Retrying OS detection (try #2) against demo.ine.local (192.94.84.3)
Retrying OS detection (try #3) against demo.ine.local (192.94.84.3)
Retrying OS detection (try #4) against demo.ine.local (192.94.84.3)
Retrying OS detection (try #5) against demo.ine.local (192.94.84.3)
NSE: Script scanning 192.94.84.3.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 22:42
Stats: 0:00:25 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE: Active NSE Script Threads: 423 (0 waiting)
NSE Timing: About 0.00% done
Completed NSE at 22:42, 3.17s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 22:42
Completed NSE at 22:42, 0.01s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 22:42
Completed NSE at 22:42, 0.00s elapsed
Nmap scan report for demo.ine.local (192.94.84.3)
Host is up, received arp-response (0.000061s latency).
Scanned at 2025-09-20 22:42:26 IST for 28s
Not shown: 65532 closed tcp ports (reset)
PORT      STATE SERVICE   REASON         VERSION
6421/tcp  open  mongodb   syn-ack ttl 64 MongoDB 2.6.10 2.6.10
| mongodb-databases: 
|   ok = 1.0
|   databases
|     0
|       empty = false
|       sizeOnDisk = 83886080.0
|       name = local
|     1
|       empty = true
|       sizeOnDisk = 1.0
|       name = admin
|_  totalSize = 83886080.0
| mongodb-info: 
|   MongoDB Build info
|     OpenSSLVersion = OpenSSL 1.0.2g  1 Mar 2016
|     sysInfo = Linux lgw01-12 3.19.0-25-generic #26~14.04.1-Ubuntu SMP Fri Jul 24 21:16:20 UTC 2015 x86_64 BOOST_LIB_VERSION=1_58
|     loaderFlags = -fPIC -pthread -Wl,-z,now -rdynamic
|     version = 2.6.10
|     gitVersion = nogitversion
|     compilerFlags = -Wnon-virtual-dtor -Woverloaded-virtual -fPIC -fno-strict-aliasing -ggdb -pthread -Wall -Wsign-compare -Wno-unused-function -Wno-unused-variable -Wno-maybe-uninitialized -Wno-unknown-pragmas -Winvalid-pch -pipe -Werror -O3 -Wno-unused-local-typedefs -Wno-unused-function -Wno-deprecated-declarations -fno-builtin-memcmp
|     ok = 1.0
|     allocator = tcmalloc
|     bits = 64
|     versionArray
|       0 = 2
|       3 = 0
|       2 = 10
|       1 = 6
|     javascriptEngine = V8
|     debug = false
|     maxBsonObjectSize = 16777216
|   Server status
|     globalLock
|       currentQueue
|         readers = 0
|         writers = 0
|         total = 0
|       activeClients
|         readers = 0
|         writers = 0
|         total = 0
|       totalTime = 298744000
|       lockTime = 42043
|     host = demo.ine.local:6421
|     uptimeMillis = 298744
|     opcountersRepl
|       query = 0
|       insert = 0
|       command = 0
|       delete = 0
|       getmore = 0
|       update = 0
|     opcounters
|       query = 9
|       insert = 1
|       command = 4
|       delete = 0
|       getmore = 0
|       update = 0
|     asserts
|       user = 0
|       warning = 0
|       rollovers = 0
|       regular = 0
|       msg = 0
|     extra_info
|       note = fields vary by platform
|       heap_usage_bytes = 62620592
|       page_faults = 2
|     indexCounters
|       accesses = 2
|       hits = 2
|       misses = 0
|       missRatio = 0.0
|       resets = 0
|     metrics
|       storage
|         freelist
|           search
|             requests = 6
|             scanned = 11
|             bucketExhausted = 0
|       document
|         updated = 0
|         deleted = 0
|         inserted = 1
|         returned = 0
|       ttl
|         passes = 4
|         deletedDocuments = 0
|       operation
|         idhack = 0
|         scanAndOrder = 0
|         fastmod = 0
|       cursor
|         open
|           noTimeout = 0
|           pinned = 0
|           total = 0
|         timedOut = 0
|       queryExecutor
|         scannedObjects = 0
|         scanned = 0
|       record
|         moves = 0
|       getLastError
|         wtime
|           totalMillis = 0
|           num = 0
|         wtimeouts = 0
|       repl
|         preload
|           indexes
|             totalMillis = 0
|             num = 0
|           docs
|             totalMillis = 0
|             num = 0
|         buffer
|           sizeBytes = 0
|           maxSizeBytes = 268435456
|           count = 0
|         apply
|           batches
|             totalMillis = 0
|             num = 0
|           ops = 0
|         network
|           readersCreated = 0
|           bytes = 0
|           getmores
|             totalMillis = 0
|             num = 0
|           ops = 0
|     ok = 1.0
|     recordStats
|       admin
|         pageFaultExceptionsThrown = 0
|         accessesNotInMemory = 0
|       local
|         pageFaultExceptionsThrown = 0
|         accessesNotInMemory = 0
|       pageFaultExceptionsThrown = 0
|       accessesNotInMemory = 0
|     backgroundFlushing
|       last_ms = 0
|       last_finished = 1758388312397
|       total_ms = 1
|       average_ms = 0.25
|       flushes = 4
|     mem
|       bits = 64
|       supported = true
|       virtual = 382
|       mappedWithJournal = 160
|       mapped = 80
|       resident = 43
|     connections
|       current = 2
|       totalCreated = 5
|       available = 838858
|     writeBacksQueued = false
|     version = 2.6.10
|     cursors
|       totalOpen = 0
|       timedOut = 0
|       note = deprecated, use server status metrics
|       clientCursors_size = 0
|       pinned = 0
|       totalNoTimeout = 0
|     network
|       bytesIn = 65
|       bytesOut = 3005
|       numRequests = 1
|     localTime = 1758388371114
|     dur
|       journaledMB = 0.0
|       compression = 0.0
|       writeToDataFilesMB = 0.0
|       commits = 30
|       earlyCommits = 0
|       timeMs
|         writeToJournal = 0
|         dt = 3070
|         remapPrivateView = 0
|         prepLogBuffer = 0
|         writeToDataFiles = 0
|       commitsInWriteLock = 0
|     locks
|       local
|         timeAcquiringMicros
|           r = 522
|           w = 0
|         timeLockedMicros
|           r = 1924
|           w = 0
|       .
|         timeAcquiringMicros
|           R = 2618
|           W = 733
|         timeLockedMicros
|           R = 4206
|           W = 42043
|       admin
|         timeAcquiringMicros
|           r = 20
|           w = 0
|         timeLockedMicros
|           r = 348
|           w = 0
|     uptime = 299.0
|     uptimeEstimate = 295.0
|     pid = 35
|_    process = mongod
41288/tcp open  memcached syn-ack ttl 64 Memcached
55413/tcp open  ftp       syn-ack ttl 64 vsftpd 3.0.3
MAC Address: 02:42:C0:5E:54:03 (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=9/20%OT=6421%CT=1%CU=35634%PV=N%DS=1%DC=D%G=Y%M=024
OS:2C0%TM=68CEE096%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10A%TI=Z%CI=Z
OS:%TS=A)SEQ(SP=107%GCD=1%ISR=10A%TI=Z%CI=Z%II=I%TS=A)OPS(O1=M5B4ST11NW7%O2
OS:=M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW7%O5=M5B4ST11NW7%O6=M5B4ST11)W
OS:IN(W1=7C70%W2=7C70%W3=7C70%W4=7C70%W5=7C70%W6=7C70)ECN(R=Y%DF=Y%T=40%W=7
OS:D78%O=M5B4NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T
OS:3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S
OS:=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%
OS:RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 12.245 days (since Mon Sep  8 16:50:35 2025)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=263 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Unix

TRACEROUTE
HOP RTT     ADDRESS
1   0.06 ms demo.ine.local (192.94.84.3)

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 22:42
Completed NSE at 22:42, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 22:42
Completed NSE at 22:42, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 22:42
Completed NSE at 22:42, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 28.20 seconds
           Raw packets sent: 65646 (2.892MB) | Rcvd: 65606 (2.628MB)
```


_____

## **INE MEHTOD 2

**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/9ac5413f24fc5ace8989b2c72ce79f13d038b8d153103125d7db15a2216c1046.png)

**Step 2:** Check if the target machine is reachable:

**Command:**

```
ping -c 4 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/8f481239b14fa8c1ee3dd8462980689610e4e4ddc97a29c6debe7053b4814989.png)

The target is reachable.

**Step 3:** Port scanning with Nmap

We can now perform a default Nmap port scan on the target to identify the open ports on the target system, this can be done by running the following command:

**Command:**

```
nmap demo.ine.local
```

As shown in the following screenshot, the default Nmap scan does not reveal any open ports. This is because the default Nmap scan profile only scans 1000 of the most commonly used ports.

![Content Image](https://assets.ine.com/lab/learningpath/be26941edca47591594610b0d7b42ba57fd2cd541ad32d747293afc3fe712067.png)

In order to get an accurate idea of the open ports on the target system, we will need to scan the entire TCP port range (65,535 ports). This can be done by running the following command:

**Command:**

```
nmap demo.ine.local -p-
```

As shown in the following screenshot, the Nmap scan reveals that the target system has 3 open ports.

![Content Image](https://assets.ine.com/lab/learningpath/2af8a9fc7e20c0e7e21f27d54760145ddb3c2d1e372d59b91096588093f67245.png)

**Step 4:** Service detection with Nmap

Now that we have identified the open ports on the target, we can learn more about the services running on the open ports by performing a service detection scan with Nmap.

This can be done by running the following command:

**Command:**

```
nmap demo.ine.local -p 6421,41288,55413 -sV
```

As shown in the following screenshot, the Nmap service detection scan reveals the names and versions of the services running on the open ports on the target system.

![Content Image](https://assets.ine.com/lab/learningpath/1057f34ec8e03099a88a950be783942a9edde525c38eb096a1a9ef25480c4bc8.png)

# Conclusion

In this lab, we explored the process of performing port scanning and service detection with Nmap.