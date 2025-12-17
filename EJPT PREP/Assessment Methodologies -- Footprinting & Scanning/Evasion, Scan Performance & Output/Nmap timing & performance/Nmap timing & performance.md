# `-T<0-5>` — Timing templates (the easiest knob)

What: presets that tune many internal timing values (retries, timeouts, parallelism, delays).  
Why: quickest way to change aggressiveness.

- `-T0` (Paranoid) — _very_ slow, long delays. Use when avoiding IDS/alerts or TCP timing side-channels.
    
- `-T1` (Sneaky) — still slow; stealthy.
    
- `-T2` (Polite) — reduces load on network/target (limits parallelism).
    
- `-T3` (Normal) — default behaviour.
    
- `-T4` (Aggressive) — good for LANs and fast scans; assumes low latency and reliable network.
    
- `-T5` (Insane) — extremely fast; for very reliable internal networks only.
    

Example: `nmap -sS -p- -T4 10.0.0.0/24`

Use-case: `-T4` for internal pentests/CTFs; `-T1`/`-T0` when trying to be stealthy or when scanning production hosts.


_________

# Parallelism & grouping controls

These adjust how many hosts/ports are scanned concurrently.

- `--min-parallelism <n>` / `--max-parallelism <n>`  
    What: lower/upper bounds on number of parallel probe threads.  
    Why: control CPU/network load and detection surface.  
    Use-case: Lower parallelism to be polite/stealthy; raise it to speed up large scans on LAN.  
    Example: `nmap --max-parallelism 100 -T4 10.0.0.0/16`
    
- `--min-hostgroup <n>` / `--max-hostgroup <n>`  
    What: how many hosts are scanned as a group at once.  
    Why: larger hostgroups = more concurrency across hosts.  
    Example: `nmap --max-hostgroup 256 -T4 10.0.0.0/16`

____________

# Rate controls

- `--min-rate <pps>` / `--max-rate <pps>`  
    What: force sending at least / at most probes per second.  
    Why: strict rate control — good for bandwidth-limited environments or to avoid IDS thresholds.  
    Example: `nmap --max-rate 100 -p1-65535 TARGET` (limit to 100 probes/sec)
    

Use-case: `--max-rate` to stay below network/device thresholds; `--min-rate` for guaranteed minimum speed (useful in scripts).

________________
# Delays and retransmit controls

- `--scan-delay <time>` / `--max-scan-delay <time>`  
    What: add a delay between probes to the same target; max-scan-delay caps adaptive delays.  
    Why: reduce load or slow scanning to evade simple rate-based detectors.  
    Example: `nmap --scan-delay 200ms TARGET`
    
- `--min-rtt-timeout <time>` / `--max-rtt-timeout <time>` / `--initial-rtt-timeout <time>`  
    What: tune RTT (round-trip time) timeouts used to wait for replies.  
    Why: helpful for high-latency links (increase) or LANs (decrease).  
    Example: `nmap --initial-rtt-timeout 50ms --max-rtt-timeout 500ms TARGET`
    
- `--max-retries <n>`  
    What: max resend attempts for probes that get no response.  
    Why: reduce wasted time on flaky networks or increase retries on lossy links.  
    Example: `nmap --max-retries 2 TARGET`
    
- `--host-timeout <time>`  
    What: abort probing a host after this total time.  
    Why: avoid long stalls on dead/slow hosts when scanning large ranges.  
    Example: `nmap --host-timeout 10m 10.0.0.0/8`
    

---

# Fragmentation & packet-level timing (affects performance/detection)

Not strictly timing, but impacts behavior and often used with timing flags.

- `-f`, `--mtu <value>` — force IP fragmentation (can slow scan and affect reassembly).
    
- `--data-length <num>` — pad packets (changes size, may cause fragmentation).
    

Use-case: use with conservative timing (e.g., `-T2`) if trying to evade signature checks; expect slower results.

---

# Source-port and socket controls (affect path handling)

- `-g <port>` / `--source-port <port>` — spoof source port (may require root).
    
- `--send-eth`/`--send-ip` — low-level send modes (affect reliability and speed).
    

These can interact with timing (e.g., require root; raw socket ops can be slower).