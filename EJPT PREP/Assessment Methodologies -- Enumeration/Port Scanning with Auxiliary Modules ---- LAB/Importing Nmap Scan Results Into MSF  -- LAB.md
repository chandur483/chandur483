This lab covers the process of importing Nmap scan results in to the Metasploit Framework.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machines will be accessible at **demo.ine.local** running a vulnerable RDP service.

**Objective:** To import Nmap scan results into MSF.

# Tools

The best tools for this lab are:

- Nmap
- msfconsole
_________
**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/f2a3a1c83c3c4edbd21ac1a3e2e02158554dbd64fade8e1e52930e3ba01551ea.png)

**Step 2:** Check if the target machine is reachable:

**Command:**

```
ping -c 4 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/34246eeac4f6a8cbac8df1fa25268ec7ea50c1befdd996f9cf880db4fdf808e3.png)

The target machine is not reachable. Let's use Nmap to skip the host discovery phase and assume that the hosts are online. This is useful in environments where ICMP echo requests (ping) are blocked by firewalls, which would normally prevent Nmap from detecting if a host is up.

**Step 3:** Importing Nmap scan results into MSF

Perform the Nmap scan and save the output in XML format.

**Command:**

```
nmap -sV -Pn -oX myscan.xml demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/ea83b84c66fd99bb24935637ea806bc4e705238916bce7704cba6557a1968817.png)

After performing an Nmap scan and exporting a scan results XML format, you can import the results directly into the MSF.

To begin with, you will need to start the postgresql database service. This can be done by running the following command:

**Command:**

```
service postgresql start
```

![Content Image](https://assets.ine.com/lab/learningpath/df2d59c7b7ce98799e5af6b10c7399e623c6a97b583627054485b5a3f66c4256.png)

We can now start the Metasploit Framework console (msfconsole) by running the following command:

**Command:**

```
msfconsole
```

![Content Image](https://assets.ine.com/lab/learningpath/80419b4eac399a56e97ab454285da02f8b75a9ed96032ce48fdc158e7c24583b.png)

Once the Metasploit Framework console (msfconsole) is started, you can verify that the MSF database is connected by running the following command:

**Command:**

```
db_status
```

As shown in the following screenshot, MSF is connected to the database.

![Content Image](https://assets.ine.com/lab/learningpath/8952f43aedcd44562b4ea4ba0ff0cfeeee58f10c79b19ea8d9b9d2ac41e2d6d4.png)

You can now import the Nmap scan results, in this case, we will be importing the scan results from the previous lab exercise.

This can be done by running the following command:

**Command:**

```
db_import myscan.xml
```

As shown in the following screenshot, the results are successfully imported.

![Content Image](https://assets.ine.com/lab/learningpath/8f325ca84b6655b8a0362fbfd2edde78011c3c2130c92ab5d8ad83c06deb9689.png)

We can now view the results by running the following commands:

**Command:**

```
hosts
services
```

As shown in the following screenshot, the Nmap scan results have been successfully imported into MSF.

![Content Image](https://assets.ine.com/lab/learningpath/eaf5edc3bfe5372e31553ebbd6f3448adaa4cd24332f23a254e029f1e40ba212.png)

# Conclusion

This lab demonstrates the process of importing Nmap scan results into the Metasploit Framework, enabling efficient integration of network discovery and vulnerability exploitation.