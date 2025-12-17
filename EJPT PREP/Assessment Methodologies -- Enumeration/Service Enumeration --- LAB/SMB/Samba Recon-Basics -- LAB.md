**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/998654f1201eb804e56f0477bfcfd3b3e99ef3b0710ee97d28de136367dec9ba.jpg)

**Step 2:** Find the default tcp ports used by smbd.

**Command:**

```
nmap demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/063a6002f018c79896655f23bec057894c5e985c7fbf7ac0c3e543a75d2a0746.jpg)

139, 445.

**Step 3:** Find the default udp ports used by nmbd.

**Command:**

```
nmap -sU --top-ports 25 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/c32cda52b79d65f57a9edc5e58abc8eed69e6045fa04ba7d785668b3d5189656.jpg)

137, 138.

**Step 4:** What is the workgroup name of samba server?

**Command:**

```
nmap -sV -p 445 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/622b0eee3bad306964f48dfd85b31af0919a1eb93cf674f1e9319289fa7a2336.jpg)

RECONLABS

**Step 5:** Find the exact version of samba server by using appropriate nmap script.

**Command:**

```
nmap --script smb-os-discovery.nse -p 445 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/c6043bad289215ac17fb2515d26aac58e742251c7cfbff08233bddd508fea479.jpg)

Samba 4.3.11-Ubuntu

**Step 6:** Find the exact version of samba server by using smb_version metasploit module.

**Commands:**

```
msfconsole -q
use auxiliary/scanner/smb/smb_version
set RHOSTS demo.ine.local
exploit
```

![Content Image](https://assets.ine.com/lab/learningpath/887374f912cf552f5408d65125aec5df8861fae1c2e38caf34898cb88816294d.jpg)

Samba 4.3.11-Ubuntu

**Step 7:** What is the NetBIOS computer name of samba server? Use appropriate nmap scripts.

**Command:**

```
nmap --script smb-os-discovery.nse -p 445 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/aae0a6167a36a0110e037e4e09909cbc0efba8ddfcd9be3669e56b1637ca8c44.jpg)

SAMBA-RECON

**Step 8:** Find the NetBIOS computer name of samba server using nmblookup.

**Command:**

```
nmblookup -A demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/d9285917fb1d8a9844c86b2a91f7bc371b45984ca23c8f954bccb267271e08b1.jpg)

SAMBA-RECON

**Step 9:** Using smbclient determine whether anonymous connection (null session) is allowed on the samba server or not.

**Command:**

```
smbclient -L demo.ine.local -N
```

Anonymous connection is allowed since shares are displayed without requirement of password.

![Content Image](https://assets.ine.com/lab/learningpath/06922968d77f0a9a4d45cff0d48defc01eef5d580cf5bb6bd192bb8ff445bb50.jpg)

Allowed.

**Step 10:** Using rpcclient determine whether anonymous connection (null session) is allowed on the samba server or not.

**Command:**

```
rpcclient -U "" -N demo.ine.local
```

Anonymous connection is allowed since no errors are thrown while connecting to samba serverwithout any credentials.

![Content Image](https://assets.ine.com/lab/learningpath/fbbe271c80e3791e4ed0c590ce35f3003663e1e239ae91c5879e94e85657b879.jpg)

Allowed.

# Conclusion

In this lab, we learned how to perform SMB server reconnaissance and identify and extract information from SMB servers on a network.

# References

1. [Samba](https://www.samba.org/)
2. [smbclient](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html)
3. [rpcclient](https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html)
4. [nmblookup](https://www.samba.org/samba/docs/current/man-html/nmblookup.1.html)
5. [Nmap Script: smb-os-discovery](https://nmap.org/nsedoc/scripts/smb-os-discovery.html)
6. [Metasploit Module: SMB Version Detection](https://www.rapid7.com/db/modules/auxiliary/scanner/smb/smb_version)