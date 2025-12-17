A ping sweep is a network scanning technique used to discover live hosts (computers, servers, or other devices) within a specific IP address range on a network.

The basic idea is to send a series of ICMP Echo Request (ping) messages to a range of IP addresses and observe the responses to determine which addresses are active or reachable


Ping sweeps work by sending one or more specially crafted ICMP packets (Type 8 - echo request) to a host. 
● If the destination host replies with an ICMP echo reply (Type 0) packet, then the host is alive/online.
● In the context of ICMP (Internet Control Message Protocol), the ICMP Echo Request and Echo Reply messages are used for the purpose of ping. These messages have specific ICMP type and code values associated with them.

-----


ICMP Echo Request: + Type: 8 + Code: 0

ICMP Echo Reply: + Type: 0 + Code: 0

______

The "Type" field in the ICMP header indicates the purpose or function of the ICMP message, and the "Code" field provides additional information or context related to the message type. 

● In the case of ICMP Echo Request and Echo Reply, the Type value 8 represents Echo Request, and the Type value 0 represents Echo Reply.

● So, when a device sends an ICMP Echo Request, it creates an ICMP packet with Type 8, Code 0.

● When the destination device receives the Echo Request and responds with an Echo Reply, it creates an ICMP packet with Type 0, Code 0.

____

When the host is offline or not reachable, the ICMP Echo Request message sent by the ping utility will not receive a corresponding ICMP Echo Reply. 

 The absence of a response doesn't necessarily mean that the host is permanently offline; it could be due to various reasons, such as network congestion, temporary unavailability, or firewall settings that block ICMP traffic. 

The ping utility provides a quick and simple way to check the reachability of a host, but it's important to interpret the results in the context of the network conditions and host configuration.

_____________

## `ping` vs `fping`

### 1. **ping**

- **Purpose**: Tests connectivity to a host (ICMP Echo Request/Reply).
    
- **Behavior**:
    
    - Sends ICMP packets to a **single host**.
        
    - Runs continuously (until stopped with `Ctrl+C`) unless you set limits (`-c`).
        
    - Simple, comes preinstalled on almost all systems.
        
- **Use case**: Quick check if one host is alive.
    

📌 **Example**
```
ping -c 4 8.8.8.8
```

→ Sends 4 ICMP echo requests to Google DNS.
```
ping -c 5 www.example.com
```

→ Resolves and pings the domain 5 times.

_________

### 2. **fping**

- **Purpose**: Advanced version of ping, **designed for multiple hosts**.
    
- **Behavior**:
    
    - Pings many hosts quickly (in parallel).
        
    - Great for host discovery in a subnet.
        
    - More control: you can set timeouts, number of retries, etc.
        
- **Use case**: Fast scanning of a whole network to see which hosts are alive.
    

📌 **Examples**

**Ping multiple hosts**
```
fping -c 2 google.com yahoo.com bing.com
```

→ Sends 2 pings each to 3 domains.

**Ping all IPs in a subnet
```
fping -a -g 192.168.1.0/24 2>/dev/null
```
- `-a`: show only alive hosts.
    
- `-g`: generate all IPs in the subnet.
    
- `2>/dev/null`: hide unreachable host errors.
