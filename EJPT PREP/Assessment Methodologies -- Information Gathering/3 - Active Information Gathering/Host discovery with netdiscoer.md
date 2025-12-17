# What is Netdiscover

Netdiscover is a small ARP-based address reconnaissance tool (built on libpcap/libnet) used for fast discovery of hosts on a local Ethernet/Wi-Fi segment. It supports **active** mode (sends ARP requests) and **passive** mode (sniffs ARP traffic only).

# When to use it

- You’re on the same L2 network (ARP only works on the same broadcast domain).
    
- You want a **fast** map of IP ⇄ MAC ⇄ vendor without doing heavier IP-level scans.
    
- You need passive discovery (silent monitoring) to avoid sending probes

- **What it means:** You are physically connected to the same switch or Wi-Fi network as your targets. You are "local." Routers form the boundaries of a broadcast domain, and ARP traffic does not cross them.
    
- **Why a pentester cares:** This is the ideal scenario for initial access. If an attacker can plug into a jack in a conference room or get on the corporate Wi-Fi, they are now inside the first line of defense (the firewall). Their first question is: _"What's around me?"_ Netdiscover is the perfect tool to answer that question in this specific situation.

_____________

# Common options & behavior (quick)

- `netdiscover` — run interactive autoscan (will try common LAN ranges). [chousensha.github.io](https://chousensha.github.io/blog/2017/06/16/netdiscover-kali-linux-tools?utm_source=chatgpt.com)
    
- `-i <iface>` — choose network interface (e.g., `-i wlan0`, `-i eth0`). [chousensha.github.io](https://chousensha.github.io/blog/2017/06/16/netdiscover-kali-linux-tools?utm_source=chatgpt.com)
    
- `-r <range>` — scan a specific subnet/range (e.g., `-r 192.168.1.0/24`). [chousensha.github.io](https://chousensha.github.io/blog/2017/06/16/netdiscover-kali-linux-tools?utm_source=chatgpt.com)
    
- `-p` — **passive** mode: only sniff ARP traffic (no ARP requests sent). Good for stealth. [0xffsec](https://0xffsec.com/handbook/discovery-and-scanning/host-discovery/?utm_source=chatgpt.com)
    
- `-f` — fast/auto mode: probe a few addresses first to find active network quickly (useful with autoscan).

Active full-subnet ARP scan (typical):
```
sudo netdiscover -i eth0 -r 192.168.1.0/24
```

