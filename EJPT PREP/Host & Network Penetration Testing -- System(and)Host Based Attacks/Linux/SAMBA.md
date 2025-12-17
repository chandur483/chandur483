SMB (Server Message Block) is a network file sharing protocol that is used to facilitate the sharing of files and peripherals between computers on a local network (LAN). 

SMB uses port 445 (TCP). However, originally, SMB ran on top of NetBIOS using port 139. 

Samba is the Linux implementation of SMB, and allows Windows systems to access Linux shares and device.

## Exploiting SAMBA

SAMBA utilizes username and password authentication in order to obtain access to the server or a network share. 

We can perform a` brute-force` attack on the SAMBA server in order to obtain legitimate credentials.

After obtaining legitimate credentials, we can use a utility called `SMBMap` in order to enumerate SAMBA share drives, list the contents of the shares as well as download files and execute remote commands on the target.

We can also utilize a tool called `smbclient`. smbclient is a client that is part of the SAMBA software suite. It communicates with a LAN Manager server, offering an interface similar to that of the ftp program. It can be used to download files from the server to the local machine, upload files from the local machine to the server as well as retrieve directory information from the server.

