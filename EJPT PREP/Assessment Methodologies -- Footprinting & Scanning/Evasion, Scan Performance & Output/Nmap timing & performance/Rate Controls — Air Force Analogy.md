# Rate Controls — Air Force Analogy

### **What it is**

- Nmap’s `--min-rate` and `--max-rate` control **how many probes per second (pps) are “fired”**.
    
- Think of **each probe as a missile** being launched by a jet.
    

|Flag|Analogy|Effect|
|---|---|---|
|`--min-rate <pps>`|Minimum number of missiles the squadron must fire per second|Ensures scan doesn’t slow down too much; guarantees pace|
|`--max-rate <pps>`|Maximum number of missiles the squadron can fire per second|Prevents overloading radar, network, or IDS; limits “noise”|
```
nmap -sS --max-rate 10 TARGET
```
- Max 10 missiles per second per jet squadron.
    
- Jets move carefully, avoiding detection by radar (IDS/IPS).
    
- Safe for production networks or sensitive CTF labs.

Scenario 2 — Fast Attack
```
nmap -sS --min-rate 1000 TARGET
```

- Each squadron must fire at least 1000 missiles/sec.
    
- Aggressive, high-speed strike — fast scan, but highly noticeable.
    
- Use in LAN or controlled lab environment where detection isn’t a concern.

__________

### **How it interacts with Parallelism & Hostgroup**

- **Parallelism (missiles per jet)** × **Hostgroup (jets in formation)** × **Rate (missiles/sec)** = **total firepower per second**
    
- Example:
    
    - `--max-parallelism 5` → each jet fires 5 missiles at once
        
    - `--max-hostgroup 10` → 10 jets flying together
        
    - `--max-rate 100` → total 100 missiles per second
        
- Nmap will throttle or batch probes to obey the max/min rate while still using parallelism and hostgroups.
    

