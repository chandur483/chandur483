you **don’t have to** use UDP — you use UDP **when the service you want lives on UDP** or when you need to test how UDP traffic is treated by the network/firewall.

Here it is, quick and practical:

# Why scan/use UDP

- **Some important services use UDP** (no TCP handshake): DNS (53), SNMP (161), NTP (123), syslog (514), SIP, TFTP (69), many game/VoIP services, some custom apps. If the target runs one of these, TCP scans won’t find it.
    
- **Find different attack surface:** Some vulnerabilities and misconfigurations exist only in UDP services.
    
- **Network behavior / firewall testing:** UDP is commonly filtered or dropped. Scanning UDP shows how the network treats connectionless protocols (rate-limiting, silent drops, ICMP blocking).
    
- **Real-world checks:** Many infra components (monitoring, time sync, DNS) depend on UDP; auditing them requires UDP probes.