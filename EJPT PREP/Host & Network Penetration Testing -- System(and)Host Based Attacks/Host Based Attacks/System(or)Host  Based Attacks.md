What Are System/Host Based Attacks?
+ System/Host based attacks are attacks that are targeted towards a specific system or host running a specific operating system, for example, Windows or Linux

Network services are not the only attack vector that can be targeted during a penetration test.

System/Host based attacks usually come in to play after you have gained access to a target network, whereby, you will be required to exploit servers, workstations or laptops on the internal network.

System/Host based attacks are primarily focused on exploiting inherent vulnerabilities on the target OS

Unlike network based attacks, host based attacks are much more specialized and require an understanding of the target operating system and the vulnerabilities that affect said operating systems.

System/Host based attacks involve exploiting misconfigurations and inherent vulnerabilities within the target OS

_______
# How they fit into an engagement

- In a black-box network test you might first find a foothold via a network service (web, RDP, SMB).
    
- **Host attacks** are what you use _after_ that foothold (or when attacking directly): escalate privileges, extract credentials, install backdoors, or compromise adjacent hosts.
    

---

# Typical goals

- **Privilege escalation** (user → admin/root)
    
- **Credential harvesting** (passwords, hashes, tokens)
    
- **Persistence** (to survive reboots and remain stealthy)
    
- **Lateral movement** (use the host as a springboard to other hosts)
    
- **Data exfiltration** (sensitive files, secrets)
    
- **Covering tracks / evasion**
    

---

# Common host-level techniques (high level)

> I’ll keep these descriptive so they’re useful for defenders and testers without providing step-by-step exploit instructions.

**Windows**

- Credential theft from memory or stored caches (e.g., tools exist that extract cached credentials).
    
- Abuse of weak service configurations (unquoted service paths, vulnerable service permissions).
    
- DLL search-order hijacks and DLL planting.
    
- Misconfigured permissions on scheduled tasks, services or registry keys.
    
- Exploiting Windows privileges/UAC bypasses to escalate.
    
- Abusing delegates, cached domain credentials, or Pass-the-Hash/Pass-the-Ticket concepts for lateral movement.
    

**Linux / Unix**

- Misconfigured `sudo` rules, world-writable sensitive files, or improperly set SUID/SGID binaries.
    
- Exposed private keys, weak SSH configs, or credential files in user directories.
    
- Vulnerable cron jobs or startup scripts that run as root which can be modified.
    
- Kernel or local privilege escalation vulnerabilities (when unpatched).
    

**Cross-platform**

- Exploiting outdated / unpatched software on the host.
    
- Abuse of agent software (management tools) with weak auth.
    
- Malware/backdoor installation for persistence.