Windows machine (Server 2012) is provided to you.

Your task is to discover the live host machines using the provided Zenmap tool. The subnet mask you need to focus on is "255.255.240.0" and CIDR 20. 

[Zenmap](https://nmap.org/zenmap/)

- A GUI version of the Nmap tool
- Easy for beginners use
- Ability to store scan data into the database.
- Plot the network diagram based on the scan result i.e services, hostname, etc.

Objective: Discover all available live hosts.

Instructions:

- You can check the IP address of the machine by running "ipconfig" command on the command prompt i.e cmd.exe
- Do not attack the gateway located at IP address 10.0.0.1

____________________

so first understand question

so open lab and run "ipconfig"

In my case 
```
IPV4 - 10.5.29.169
```

so 

subnet mask is 255.255.240.0 in 32bit

## Step 1: Group into 4 octets

The subnet mask is 32 bits. Split into 8-bit groups:
```
11111111 . 11111111 . 11110000 . 00000000
```

## Step 2: Convert each group from binary → decimal

- `11111111` = 128+64+32+16+8+4+2+1 = **255**
    
- `11111111` = **255**
    
- `11110000` = 128+64+32+16 = **240**
    
- `00000000` = **0**

## Step 3: Put them back together

So the dotted-decimal mask is:
```
255.255.240.0
```

## Step 4: Why this is `/20`

Count the number of 1-bits:

- First octet: 8
    
- Second octet: 8
    
- Third octet: 4  
    Total = 20 bits = **/20**

✅ That’s why `/20` = `255.255.240.0`.

__________

our IP = 10.5.29.169

third octae is 29

so 240 - 256 = 16 
so 0-15 ,16-32 , ......

so 29 falls BTW 16 - 32

so 10.5.16.0/20 - 10.5.32.255

bit confusing feel free to use chatgpt 

______________________________

