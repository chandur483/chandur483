Domain Name System (DNS) is a protocol that is used to resolve domain names/hostnames to IP addresses. 
+ During the early days of the internet, users would have to remember the IP addresses of the sites that they wanted to visit, DNS resolves this issue by mapping domain names (easier to recall) to their respective IP addresses.
+ A DNS server (nameserver) is like a telephone directory that contains domain names and their corresponding IP addresses. 
+ A plethora of public DNS servers have been set up by companies like Cloudflare (1.1.1.1) and Google (8.8.8.8). These DNS servers contain the records of almost all domains on the internet

## DNS Records

+ A -Resolves a hostname or domain to an IPv4 address.
+ AAAA -Resolves a hostname or domain to an IPv6 address. 
+ NS -Reference to the domains nameserver. 
+ MX -Resolves a domain to a mail server. 
+ CNAME -Used for domain aliases.
+ TXT -Text record. 
+ HINFO -Host information. 
+ SOA -Domain authority.
+ SRV -Service records. 
+ PTR -Resolves an IP address to a hostname

## DNS Interrogation

DNS interrogation is the process of enumerating DNS records for a specific domain.
+ The objective of DNS interrogation is to probe a DNS server to provide us with DNS records for a specific domain. 
+ This process can provide with important information like the IP address of a domain, subdomains, mail server addresses etc
________________

## DNS Zone Transfer

In certain cases DNS server admins may want to copy or transfer zone files from one DNS server to another. This process is known as a zone transfer.
+ If misconfigured and left unsecured, this functionality can be abused by attackers to copy the zone file from the primary DNS server to another DNS server
+ . + A DNS Zone transfer can provide penetration testers with a holistic view of an organization's network layout. 
+ Furthermore, in certain cases, internal network addresses may be found on an organization's DNS servers.


**AXFR** (ASCII Zone File Transfer) is a type of **DNS zone transfer**. It is a mechanism used to replicate a DNS database (a "zone") from a **primary DNS server** to a **secondary DNS server**.

Think of it as a "copy-paste" or "database replication" for DNS servers, ensuring all servers have the exact same, up-to-date records.

#### What is its Purpose?

A domain's DNS information (like `example.com`) is stored in a "zone file" on an **authoritative nameserver**. To provide redundancy and load balancing, there are multiple authoritative servers (e.g., `ns1.example.com`, `ns2.example.com`).

AXFR is the protocol used by a **secondary server** to request a full copy of the entire zone file from the **primary server**. This ensures all servers have consistent data.

#### How Does it Work?

The process typically works like this:

1. The secondary server determines it needs to update its zone, either based on a schedule or a notification from the primary (via **NOTIFY**).
    
2. The secondary sends an **AXFR query** to the primary server.
    
3. The primary server responds by sending the entire zone file, broken down into multiple DNS messages over a TCP connection (TCP is used instead of UDP because the data is often too large and requires a reliable connection).
    

#### 3. The Security Problem

**AXFR provides no authentication by itself.** This makes it a significant security risk if misconfigured.

- **Information Leak:** If an attacker can perform an AXFR request (a "zone transfer") to your DNS server, they can get a **complete list of all your hosts and subdomains**. This is a goldmine for planning further attacks, as it reveals your network's structure (e.g., `mail.corporate.com`, `vpn.corporate.com`, `dev-server.corporate.com`).
    
- **Reconnaissance:** This is a classic step in the "footprinting" or "reconnaissance" phase of a cyber attack.