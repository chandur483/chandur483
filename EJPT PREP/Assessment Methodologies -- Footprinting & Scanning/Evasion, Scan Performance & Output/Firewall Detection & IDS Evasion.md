- `-f` — fragment IP packets (tiny pieces).
    
- `--data-length <num>` — append `<num>` random bytes to each probe’s payload.
    
- `--mtu <value>` — set the IP MTU (forces fragmentation/padding to that size).
    
- `-g <port>` (aka `--source-port`) — use the specified source port for probes.


# `-f` — IP packet fragmentation

What it does

- Tells Nmap to set the “fragment” bit (and send smaller IP fragments) so the probe is split across multiple IP packets instead of one.
    

Why people use it

- Attempt to evade poorly written IDS/IPS signatures that only inspect the first fragment or only reassemble partially.
    
- Hide suspicious payloads by splitting them so pattern matchers miss them.

```
sudo nmap -sS -f TARGET
```

_______
# `--data-length <num>` — append random filler bytes

What it does

- Adds `<num>` random bytes to the end of each probe packet’s payload so packets are larger / have different signatures.
    

Why use it

- Change the fingerprint or signature of probes to dodge simple signature-based detection.
    
- Can help when IDS signatures match a specific probe payload length or pattern.]

```
sudo nmap -sV --data-length 50 TARGET
```


________

# `--mtu <value>` — set packet MTU

What it does

- Forces Nmap to use an MTU of `<value>` bytes for the packets it sends. If the packet would be larger than the MTU, it will be fragmented (like `-f` behaviour but controlled by size).
    

Why use it

- Test how network devices handle small MTUs.
    
- Another way to force fragmentation to attempt evasion or test path MTU behaviour.

```
sudo nmap -sS --mtu 24 TARGET
```


____________

# `-g <port>` / `--source-port <port>` — set the source port

What it does

- Sends probes with the specified source port number instead of a random ephemeral port.
    

Why use it

- Bypass simplistic firewall rules that allow traffic only from trusted source ports (e.g., 53 for DNS, 80/443).
    
- Make scanning appear to come from a service port which some firewalls treat as allowed/less suspicious.

```
sudo nmap -sT -g 53 TARGET    # try to look like DNS source port
```

