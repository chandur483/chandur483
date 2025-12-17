- Rate = **how fast your squadron fires overall**.
    
- Delays/retransmit = **how carefully each jet times its shots**.


# Delays & Retransmit Controls — Air Force Analogy

### 1️⃣ The concept

- Each **host = a jet** flying over a target zone.
    
- Each **port/probe = a missile** fired at a ground target.
    
- Delays & retransmit settings control **how long a jet waits before firing the next missile** and **how many times it retries if the missile doesn’t hit**.
    

---

### 2️⃣ Key flags and their analogy

| Nmap Flag                                 | Jet Analogy                                                              | Effect                                                                |
| ----------------------------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------- |
| `--scan-delay <time>`                     | Jet waits <time> before firing the next missile at the same target       | Avoid firing too quickly; stay stealthy; reduce radar/IDS detection   |
| `--max-scan-delay <time>`                 | Jet will never wait longer than this between missiles, even if adapting  | Caps maximum wait time for responsiveness                             |
| `--max-retries <n>`                       | If missile misses target (no response), jet fires **up to n more times** | Increases chances of hitting a target despite interference or jamming |
| `--initial-rtt-timeout <time>`            | Estimated time jet expects missile to hit target initially               | Faster missiles on LAN vs slower on Internet                          |
| `--min-rtt-timeout` / `--max-rtt-timeout` | Adjusts wait time dynamically based on radar response                    | Jet adapts timing based on how quickly the target responds            |
```
nmap -sS --scan-delay 500ms --max-retries 2 --initial-rtt-timeout 100ms TARGET
```

**Step-by-step analogy:**

1. **Jet fires first missile (probe) at Port 80.**
    
    - Waits 100ms for radar (host) to respond (initial RTT).
        
2. **No response?**
    
    - Jet retries **up to 2 more times** (`--max-retries 2`) at Port 80.
        
    - Between retries, it waits **scan-delay = 500ms** for safety.
        
3. **Jet fires next missile (Port 443)**
    
    - Waits 500ms (`--scan-delay`) before launching next missile.
        
    - Adjusts wait based on previous radar response times (`RTT timeouts`).
        
4. **Observation:**
    
    - If radar responds quickly, jet fires next missile sooner.
        
    - If radar slow, jet adapts but never exceeds `--max-scan-delay`.
        

**Visual timeline (jets/missiles):**


```
Time →
Jet1:
Port80: |----fire----wait/retry----fire----wait/retry----|
Port443:          |----fire----wait/retry----|
Port22:                      |----fire----|
```

- Each bar = missile fired
    
- Gaps = adaptive wait (`--scan-delay` / RTT)
    
- Retries = repeated attempts at same target.