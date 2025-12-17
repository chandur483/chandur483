# Core concepts

- **Host discovery** (aka “ping scan”) is discovering _which IPs are alive_ before doing slower port/os scans.
    
- On a local Ethernet network, **ARP** is the most reliable and fastest — Nmap will prefer it automatically when you scan a local subnet.

# Basic commands (quick reference)

Local subnet (fast ARP-based discovery):
```
sudo nmap -sn 192.168.1.0/24
```

Explanation: `-sn` = ping scan (no port scan). On a local LAN Nmap will use ARP which is accurate and fast.

# Useful flags for discovery scans

- `-sn` — ping scan (no port scan).
    
- `-sP` — legacy equivalent (old name).
    
- `-PR` — ARP ping.
    
- `-PE`, `-PP`, `-PM` — ICMP echo, timestamp, and netmask requests.
    
- `-PS` — TCP SYN ping to specified ports.
    
- `-PA` — TCP ACK ping.
    
- `-PU` — UDP ping.
    
- `-n` — no DNS resolution (faster).
    
- `-R` — force reverse DNS resolution.
    
- `-v` / `-vv` — verbose.
    
- `-oA <basename>` — save in all output formats.
    
- `-oG <file>` — grepable output (easy to parse).
    
- `-T0..T5` — timing template (T0 slowest, T5 fastest/aggressive).
    
- `--max-retries`, `--host-timeout` — fine tune retries and host timeouts.
    
- `--traceroute` — append traceroute info for discovered hosts.