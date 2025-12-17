# Nmap Parallelism & Hostgroup — Air Force Analogy

### 1️⃣ Parallelism = How many missiles each jet can fire **at the same time**

- Imagine **each host** as a **jet**.
    
- **Ports on that host** are **targets on the ground**.
    
- `--max-parallelism` = how many missiles each jet can launch simultaneously at different targets.
    

**Example**:
```
--max-parallelism 5
```

- Each jet (host) can fire **5 missiles (probes)** at the same time.
    
- More missiles → faster, but jets may be noticed by radar (IDS/IPS).
    
- Fewer missiles → stealthier, slower.
    

**Diagram** (per host / jet):
```
Host1 (Jet1): |--P1--|--P2--|--P3--|--P4--|--P5--|
Host2 (Jet2): |--P1--|--P2--|--P3--|--P4--|--P5--|
```

P = probe/missile

_________

### 2️⃣ Hostgroup = How many jets are flying together **at the same time**

- `--max-hostgroup` = how many jets (hosts) fly **in formation**, attacking at the same time.
    
- More jets → faster scan across network, more noticeable on radar.
    
- Fewer jets → stealthier, slower, safer.
    

**Example**:
```
--max-hostgroup 3
```

- Up to **3 jets** fly together. Each jet fires missiles (parallelism) at its assigned targets (ports).
    

**Diagram (jets + missiles)**:
```
Time →
Hostgroup1: Jet1: |P1 P2 P3|  Jet2: |P1 P2 P3|  Jet3: |P1 P2 P3|
Hostgroup2: Jet4: |P1 P2 P3|  Jet5: |P1 P2 P3|  Jet6: |P1 P2 P3|
```

- Each jet fires 3 missiles in parallel.
    
- 3 jets in formation scan together.
    
- Once finished, the next group of jets takes off.