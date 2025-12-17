# Fragmentation & Packet-Level Timing — Air Force Analogy

### 1️⃣ Concept

- Each **host = enemy base**
    
- Each **port = target at the base**
    
- Each **probe = missile** fired at a target
    

Now, imagine your **missiles are very large** or have special payloads. Some enemy radar systems can detect a full-size missile easily. So instead of firing a single big missile:

- You **break it into smaller pieces** (fragmentation, `-f` or `--mtu`)
    
- Each piece travels separately and recombines at the target (like IP reassembly)
    
- You can also **pad missiles with extra useless material** (`--data-length`) to confuse radar, make pattern detection harder, or bypass filters
    

---

### 2️⃣ Nmap Flags & Jet Analogy

|Flag|Analogy|Effect|
|---|---|---|
|`-f`|Jet splits a big missile into smaller mini-missiles|Forces IP fragmentation; each “mini-missile” may bypass simple radar/IDS but scan is slower due to more packets|
|`--mtu <value>`|Limit size of each missile|Smaller MTU → more fragments; affects speed and packet reassembly|
|`--data-length <num>`|Jet adds extra padding to missile payload|Changes size, can cause fragmentation, may evade signature-based detection|

```
nmap -sS -p 1-1000 -f --mtu 24 --data-length 20 TARGET
```

**Jet analogy step-by-step:**

1. **Jet fires a missile at Port 80**
    
    - The missile is **too big for radar detection**
        
    - Jet **splits it into tiny pieces** (`-f` with `--mtu 24`)
        
2. **Mini-missiles travel separately**
    
    - Each fragment may evade simple radar sensors (IDS)
        
    - Target host will **reassemble fragments** to process the attack
        
3. **Missile is padded**
    
    - Extra 20 units of padding (`--data-length 20`) added
        
    - Radar sees unusual size and pattern → may confuse logging or detection
        
4. **Outcome**
    
    - Stealthier scan
        
    - Slower because many fragments must be sent and reassembled
        
    - Potential to bypass poorly configured IDS/firewalls
        

---

### 4️⃣ Visual timeline / analogy

```
Original Missile (full TCP probe)
   |
   v
[Fragment 1] [Fragment 2] [Fragment 3]  -- (arrives separately at host)
   |           |           |
   +-----------+-----------+   -> reassembled by target
```


- Each fragment is like a **mini-missile**
    
- Fragments still need **timing control** (delays, RTT, parallelism)
    
- Combined with `--data-length`, **size and pattern of missiles changes**, affecting IDS detection
    

---

### 5️⃣ Key takeaways

1. **Fragmentation = splitting a big probe into smaller pieces** → slower scan but can evade detection.
    
2. **MTU = size of each fragment** → smaller → more fragments → more packets → slower but stealthier.
    
3. **Data-length = padding payload** → changes pattern → may trigger or bypass detection systems.
    
4. **Works with timing flags** → fragmentation interacts with parallelism, hostgroups, delays, and rate; small fragments + high parallelism = careful balance.
    

---

