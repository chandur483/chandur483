Versions of SNMP: 
+ SNMPv1: The earliest version, using community strings (essentially passwords) for authentication. 
+ SNMPv2c: An improved version with support for bulk transfers but still relying on community strings for authentication. 
+ SNMPv3: Introduced security features, including encryption, message integrity, and user-based authentication. 

Ports: 
+ Port 161 (UDP): Used for SNMP queries. 
+ Port 162 (UDP): Used for SNMP traps (notifications).

_________
## SNMP Enumeration

SNMP enumeration in penetration testing involves querying SNMP enabled devices to gather information useful for identifying potential vulnerabilities, misconfigurations, or points of attack.

Here are the key objectives and outcomes of SNMP enumeration during a pentest:

Identify SNMP-Enabled Devices: Determine which devices on the network have SNMP enabled and whether they are vulnerable to information leakage or attacks. 

Extract System Information: Collect system-related data like device names, operating systems, software versions, network interfaces, and more.

Identify SNMP Community Strings: Test for default or weak community strings, which can grant unauthorized access to device information. 

Retrieve Network Configurations: Gather information about routing tables, network interfaces, IP addresses, and other network-specific details. 

Collect User and Group Information: In some cases, SNMP can reveal user account information and access permissions. 

Identify Services and Applications: Find out which services and applications are running on the target devices, potentially leading to further attack vectors.

_________

# What SNMP is _supposed_ to do (legit purpose)

Think of SNMP as a **remote control + thermometer** for network devices:

- A company has **routers, switches, printers, servers** spread everywhere.
    
- Instead of logging in to each device one by one, they use **SNMP** to **ask questions** like:
    
    - “How much CPU is this router using?”
        
    - “How many users are connected to this switch port?”
        
    - “Is this printer out of ink?”
        
- And sometimes even to **make changes** (if allowed), like:
    
    - “Shut down this network port.”
        
    - “Change this route.”
        

SNMP is useful because:  
✅ IT admins can monitor health (uptime, bandwidth, errors).  
✅ They can collect this into a dashboard (Nagios, Zabbix, SolarWinds, etc.).  
✅ They get alerts (“trap messages”) if something fails.

**Analogy:** SNMP is like having a **remote thermometer and switchboard** in a factory. You can read machine temperatures (monitor) and flip some switches (configure).


_______

# How attackers misuse SNMP

If SNMP is left **open with weak/default settings** (very common):

1. **Info Gathering (Reconnaissance)**
    
    - Attackers can ask the device questions with SNMP and get answers.
        
    - Example: “Tell me all IP addresses you know,” “What software version are you running?”
        
    - This gives them a **map of the network** (devices, connections, OS versions).
        
2. **Changing Configurations (if write access enabled)**
    
    - Some devices allow SNMP to not only read but also **write**.
        
    - If attacker finds the “write password” (community string), they can:
        
        - Change routes (redirect traffic).
            
        - Disable ports (knock devices offline).
            
        - Change DNS (send users to fake websites).
            
3. **Amplification for DDoS**
    
    - Because SNMP uses **UDP**, attackers can send small fake requests that trigger **big responses** sent to a victim → used for denial-of-service attacks.