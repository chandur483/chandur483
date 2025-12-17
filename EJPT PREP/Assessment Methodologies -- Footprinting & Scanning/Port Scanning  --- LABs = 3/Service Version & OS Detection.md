-sV for version detection
--version-intensity 0 - 8 to aggresive version detection.

-O for operating system(OS)
--osscan-guess for aggresive OS detection.

```
nmap -sS -sV --version-intensity 8 -O --osscan-guess -T4  -p- 'IP"
```
