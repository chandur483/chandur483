An SMB relay attack is a type of network attack where an attacker intercepts SMB (Server Message Block) traffic, manipulates it, and relays it to a legitimate server to gain unauthorized access to resources or perform malicious actions. 

This type of attack is common in Windows networks, where SMB is used for file sharing, printer sharing, and other network services.

## How SMB Relay Attacks Work

Interception: The attacker sets up a man-in-the-middle position between the client and the server. This can be done using various techniques, such as ARP spoofing, DNS poisoning, or setting up a rogue SMB server. 

Capturing Authentication: When a client connects to a legitimate server via SMB, it sends authentication data. The attacker captures this data, which might include NTLM (NT LAN Manager) hashes. 

Relaying to a Legitimate Server: Instead of decrypting the captured NTLM hash, the attacker relays it to another server that trusts the source. This allows the attacker to impersonate the user whose hash was captured. 

Gain Access: If the relay is successful, the attacker can gain access to the resources on the server, which might include sensitive files, databases, or administrative privileges. This access could lead to further lateral movement within the network, compromising additional systems