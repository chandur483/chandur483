#### Windows Recon: Nmap Host Discovery

Ping the target machine to see if it’s alive or not.

```
ping -c 5 demo.ine.local
```

```
nmap -Pn demo.ine.local
```

We can see multiple ports are open on the target machine.

Now, we will scan any random port that isn’t open. In this case, scan port 443. If the port is not open, we would receive “filtered” output from that port.

```
nmap -Pn -p 443 demo.ine.local
```

We can observe in the Nmap output that the host is up, but port 443 is filtered.

**About Filtered port:**

Nmap cannot determine whether the port is open because packet filtering prevents its probes from reaching the port. The filtering could be from a dedicated firewall device, router rules, or host-based firewall software. These ports frustrate attackers because they provide so little information. Sometimes they respond with ICMP error messages such as type 3 code 13 (destination unreachable: communication administratively prohibited), but filters that simply drop probes without responding are far more common. This forces Nmap to retry several times just in case the probe was dropped due to network congestion rather than filtering. This slows down the scan

Similarly, if we want to discover the running application on port 80, we could use option -sV, and this option is used to determine the application version information.

**Command:**

```
nmap -Pn -sV -p 80 demo.ine.local
```

________

## **INE MOTHOD 

**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/1699c822fc8308860e257287c827ebec20c7aae01e4d5dbaffa58dc620f8434e.jpg)

**Step 2:** Ping the target machine to see if it’s alive or not.

**Command:**

```
ping -c 5 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/1d1f000a9a296458b5de30512f5121321ec0d6736c9b34d651233a7ca5f94452.jpg)

We can observe that the target is not responding to the ping requests, so this does not confirm if it’s alive or down.

**Step 3:** Run a Nmap scan against the target.

**Command:**

```
nmap demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/847bccce13deb17080eec4729959426aa5572f26ee8d43f89f1b56ea95b3c221.jpg)

Nmap also could not detect whether the host was up or not. Many security tools first ping the host before they start scanning or exploiting the target. In that case, one has to use advanced Nmap options, i.e., -A or -T5, etc., in order to get the correct output.

In the nmap, there is one option, i.e., -Pn (Treat all hosts as online; skip host discovery). This option will force the scanning even if it has detected the target as down in host discovery.

**Step 4:** Running Nmap using the -Pn option to discover all alive ports.

**Command:**

```
nmap -Pn demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/f8797e2482cc84c1335b09a939742dfed5d697f6ba1cf2064e8c39327d0b041d.jpg)

We can see multiple ports are open on the target machine.

Now, we will scan any random port that isn’t open. In this case, scan port 443. If the port is not open, we would receive “filtered” output from that port.

**Command:**

```
nmap -Pn -p 443 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/3fc2f9aa2f278e7d00ae893c440aa5ff5c51bcbaa96e4d1a4e2459884b575daf.jpg)

We can observe in the Nmap output that the host is up, but port 443 is filtered.

**About Filtered port:**

Nmap cannot determine whether the port is open because packet filtering prevents its probes from reaching the port. The filtering could be from a dedicated firewall device, router rules, or host-based firewall software. These ports frustrate attackers because they provide so little information. Sometimes they respond with ICMP error messages such as type 3 code 13 (destination unreachable: communication administratively prohibited), but filters that simply drop probes without responding are far more common. This forces Nmap to retry several times just in case the probe was dropped due to network congestion rather than filtering. This slows down the scan dramatically.

**Source:** https://nmap.org/book/man-port-scanning-basics.html

**Step 5:** Similarly, if we want to discover the running application on port 80, we could use option -sV, and this option is used to determine the application version information.

**Command:**

```
nmap -Pn -sV -p 80 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/e26678f97b0d5a70041c0cb55fd1ab9dc893ae128b42b1b3f39238189b5204bb.jpg)

This is one of the ways we can discover a machine that is behind a firewall, forcing tools for scanning.

# Conclusion

In this lab, we saw a standard method to discover hosts using Nmap, which is behind a firewall.