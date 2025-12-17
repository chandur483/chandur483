SMTP (Simple Mail Transfer Protocol) is a communication protocol that is used for the transmission of email. 
+ SMTP uses TCP port 25 by default. It is can also be configured to run on TCP port 465 and 587. 
+ We can utilize auxiliary modules to enumerate the version of SMTP as well as user accounts on the target system.

An SMTP (Simple Mail Transfer Protocol) server is a mail server that sends and receives emails using the SMTP protocol. It's responsible for sending, receiving, and relaying outgoing mail between email senders and recipients.

***In this lab we will look at the basics of Postfix SMTP server reconnaissance.

**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/a129fa98bd787a4b492903c1aebbed959d02f5ade670e192d50d0d573af572ee.jpg)

**Step 2:** What is the SMTP server name and banner.

**Answer:**

```
Server: Postfix
Banner: openmailbox.xyz ESMTP Postfix: Welcome to our mail server.
```

**Command:**

```
nmap -sV -script banner demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/49a93cd264d434fe6e21440d21281b0626dcd35f8c9838d76fc4d4372950fc99.jpg)

**Step 3:** Connect to SMTP service using netcat and retrieve the hostname of the server (domain name).

**Answer:**

```
openmailbox.xyz
```

**Command:**

```
nc demo.ine.local 25
```

![Content Image](https://assets.ine.com/lab/learningpath/868ad587a43554487aa03427a9cd4b24113611f747902b04b8365ed9c2d84cb3.jpg)

**Step 4:** Does user "admin" exist on the server machine? Connect to SMTP service using netcat and check manually.

**Answer:**

```
Yes
```

**Command:**

```
VRFY admin@openmailbox.xyz
```

![Content Image](https://assets.ine.com/lab/learningpath/814ebe5c97a400735286f83933d4e33777c754dfe68d70905e779393b8141f15.jpg)

**Step 5:** Does user "commander" exist on the server machine? Connect to SMTP service using netcat and check manually.

**Answer:**

```
No
```

**Command:**

```
VRFY commander@openmailbox.xyz
```

![Content Image](https://assets.ine.com/lab/learningpath/5905fc7af9690ca89a1239eea61610beeb83c40ec4396a4c8aed573325b65ec6.jpg)

**Step 6:** What commands can be used to check the supported commands/capabilities? Connect to SMTP service using telnet and check.

**Commands:**

```
telnet demo.ine.local 25
HELO attacker.xyz
EHLO attacker.xyz
```

![Content Image](https://assets.ine.com/lab/learningpath/b854e628a2ad1dcaa1f9beb81fd7caac5de5b798cac2c8539fcb1692dfe8fd7f.jpg)

**Step 7:** How many of the common usernames present in the dictionary /usr/share/commix/src/txt/usernames.txt exist on the server. Use smtp-user-enum tool for this task.

**Answer:**

```
8
```

**Command:**

```
smtp-user-enum -U /usr/share/commix/src/txt/usernames.txt -t demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/7496f0fa24e6561eb8a8cdd8fa86a8de78b827856530f85b34da39d67a6b1403.jpg)

**Step 8:** How many common usernames present in the dictionary /usr/share/metasploit-framework/data/wordlists/unix_users.txt exist on the server. Use suitable metasploit module for this task.

**Answer:**

```
22
```

**Commands:**

```
msfconsole -q
use auxiliary/scanner/smtp/smtp_enum
set RHOSTS demo.ine.local
exploit
```

![Content Image](https://assets.ine.com/lab/learningpath/6dc40a55337390ce5dcc46a1bc534d66eb6c51f4540599d55b7222ce852fa128.png)

**Step 9:** Connect to SMTP service using telnet and send a fake mail to root user.

**Commands:**

```
telnet demo.ine.local 25
HELO attacker.xyz
mail from: admin@attacker.xyz
rcpt to:root@openmailbox.xyz
data
Subject: Hi Root
Hello,
This is a fake mail sent using telnet command.
From,
Admin
.
```

**Note:** There is a dot(.) in the last line which indicates the termination of data.

![Content Image](https://assets.ine.com/lab/learningpath/e833531e29d6074d84d48508390cde795e876376aabfb84427e5e0bf0cc78445.jpg)

**Step 10:** Send a fake mail to root user using sendemail command.

**Command:**

```
sendemail -f admin@attacker.xyz -t root@openmailbox.xyz -s demo.ine.local -u Fakemail -m "Hi root, a fake from admin" -o tls=no
```

## Common `sendemail` options and what they do

- `-f FROM`  
    Envelope sender (the address used in the SMTP `MAIL FROM` command). **Use an address you control**.
    
- `-t TO`  
    Recipient(s). Can be comma-separated: `-t root@localhost,ops@example.com`.
    
- `-u SUBJECT`  
    Message subject.
    
- `-m MESSAGE`  
    Message body text. Newlines are literal `\n` or use a heredoc / file.
    
- `-o message-file=FILENAME`  
    Read message body (and headers) from a file instead of `-m`.
    
- `-s SMTP[:PORT]`  
    SMTP server and optional port.
    
- `-xu USER` and `-xp PASS`  
    SMTP authentication credentials.
    
- `-o tls=yes`  
    Enable STARTTLS.
    
- `-a FILE`  
    Attach a file. Use multiple `-a` for several attachments.
    
- `-v`  
    Verbose — prints the SMTP transaction (good for debugging).
    
- `-q`  
    Quiet mode.
    
- `-o message-charset=UTF-8`  
    Set charset.
    
- `-o timeout=X`  
    Connection timeout seconds.
    
![Content Image](https://assets.ine.com/lab/learningpath/fa8fa943c15631fab0c4b428d67079facc98c839cb14d91857af652c20deea56.jpg)

# Conclusion

In this lab, we looked at the basics of Postfix SMTP server reconnaissance.

# References

1. [http://www.postfix.org/]
2. [https://tools.kali.org/information-gathering/smtp-user-enum]
3. [http://www.postfix.org/sendmail.1.html]
4. [https://www.rapid7.com/db/modules/auxiliary/scanner/smtp/smtp_enum]