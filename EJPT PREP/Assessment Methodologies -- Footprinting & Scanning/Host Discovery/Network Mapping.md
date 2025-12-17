After collecting information about a target organization during the passive information gathering stage, a penetration tester typically moves on to active information gathering phase which involves discovering hosts on a network, performing port scanning and enumeration.

As you know, every host connected to the Internet or a private network must have a unique IP address that uniquely identifies it on said network.

Network mapping in the context of penetration testing (pentesting) refers to the process of discovering and identifying devices, hosts, and network infrastructure elements within a target network

Pentesters use network mapping as a crucial initial step to gather information about the network's layout, understand its architecture, and identify potential entry points for further **exploitation**

## Example - Why Map a Network?

A company asks for you/your company to perform a penetration test, and the following address block is considered in scope: 200.200.0.0/16.

`# How to compute

- Prefix `/N` → host bits = `32 - N`.
    
- Usable hosts (normal networks) = `2^(32-N) - 2` (subtract network & broadcast).
    
- Special cases: `/31` is used for point-to-point (2 usable addresses per RFC 3021). `/32` denotes a single host.

So 
```
CIDR  - 200.200.0.0/16
Network - 200.200.0.0
Broadcast - 200.200.255.255
Usable range - 200.200.0.1 → 200.200.255.254
Usable hosts - 2^16 - 2 = 65,534
```

The first job for the penetration tester will involve determining which of the 65536 IP addresses are assigned to a host, and which of those hosts are online/active


