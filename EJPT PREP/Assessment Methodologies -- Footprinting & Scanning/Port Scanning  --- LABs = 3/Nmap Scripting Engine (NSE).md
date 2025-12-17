path /usr/share/nmap/scripts

### **1. Script Categories Overview**

NSE scripts are categorized by purpose:

| Category    | Purpose                             |
| ----------- | ----------------------------------- |
| `auth`      | Authentication checks               |
| `broadcast` | Network broadcast checks            |
| `default`   | Scripts run by default              |
| `discovery` | Service and host discovery          |
| `dos`       | Denial-of-service testing           |
| `exploit`   | Exploit detection                   |
| `external`  | External service checks             |
| `fuzzer`    | Fuzzing targets                     |
| `intrusive` | Scripts that might crash the target |
| `malware`   | Malware detection                   |
| `safe`      | Safe scripts, no harm to target     |
| `version`   | Version detection                   |
| `vuln`      | Vulnerability detection             |

usage

nmap --script=category
example:
```
nmaa --script=vuln 192.11.144.11
```


or 
```
nmap --script=http-vuln* 192.11.113.11
```


Specific Service Checks
```
nmap -p 80,443 --script=http-enum,http-vuln* <target>
```

Only on your own targets:
```
nmap --script=exploit <target>
nmap --script=intrusive <target>
```

