+ Armitage is a free Java based GUI front-end for the Metasploit Framework developed by Raphael Mudge and is used to simplify network discovery, exploitation and post exploitation.

Overview 

Armitage is a graphical cyber attack management tool for Metasploit, which is one of the most widely used frameworks for penetration testing. Armitage simplifies the use of Metasploit by providing a user-friendly interface, making it easier for users to manage exploits, payloads, and sessions during penetration tests. This lab covers the process of performing port scanning & enumeration with Armitage.

Task 

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine will be accessible at **demo1.ine.local**.

**Objective:** Enumerate the target machine and perform port scanning using Armitage.

# Tools

- Armitage
- Metasploit Framework

**Step 1:** Open the lab link to access the Kali GUI instance.

![Content Image](https://assets.ine.com/lab/learningpath/4d044927c6afe1362e64e93434c224e76cba628288ed6ba9bb752347673b26bf.png)

**Step 2:** Check if the target machine is reachable.

**Command:**

```
ping -c 5 demo1.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/f79d4996a9346c6af04ce5e379f7206ae8631a0c2bb16383fdefca76aa8471e3.png)

**Step 3:** Port scanning & enumeration with Armitage.

Before starting up Armitage, you will need to start the postgresql database service, this can be done by running the following command:

**Commands:**

```
service postgresql start
service postgresql status
```

![Content Image](https://assets.ine.com/lab/learningpath/497b592fe9900d2e4fff77e9574592e88b02f0ca53121a66e0a118d05de3ad13.png)

We can start up Armitage by running the following command:

**Command:**

```
armitage
```

After starting up Armitage, you will be prompted to connect to the MSF database as shown in the following screenshot.

![Content Image](https://assets.ine.com/lab/learningpath/7435a52f8d4efb220804c1d94f5baf69fe14911708bedfd91c617ece47b2f5bd.png)

Click on "Yes."

![Content Image](https://assets.ine.com/lab/learningpath/86d04bb4bd56f4305ff69c0078e5b72024adc71cd1ec0d4675a39a9b38223f85.png)

**Note:** Armitage might take around 1 minute to get fully loaded.

After connecting to the MSF database, Armitage will open up as shown in the following screenshot.

![Content Image](https://assets.ine.com/lab/learningpath/74ae3df886af3e3eab4897d618ca941885f5714d4d72bdce29f39e305eeb60e6.png)

We can get started by adding the IP address/domain name of the target machine; this can be done by clicking on Hosts on the toolbar and on Add Hosts in the sub-menu, as shown in the following screenshot.

![Content Image](https://assets.ine.com/lab/learningpath/6eeec5c7fc72868ce9c7114b5c130d15ce7d747026ca98c0a0adaf46724aaee8.png)

You will then be prompted to add the host address of the target system as shown in the following screenshot.

Enter "demo1.ine.local" and click on "Add."

![Content Image](https://assets.ine.com/lab/learningpath/596c2d28c25b9d729cb87f963e436a5c83cb0a5ceacaeaf5b5d90e8d179f2754.png)

After adding the target machine's address, the system will be added to the top panel.

We can perform a port scan on the target system by right-clicking on the system and clicking on Scan as shown in the following screenshot.

![Content Image](https://assets.ine.com/lab/learningpath/cb989608f4391be54febc1edd0f3452d2d04e53c1c962ee8ca09223f852def92.png)

This will begin the port scan on the target system, after which the results will be displayed in the bottom output pane as shown in the following screenshot.

![Content Image](https://assets.ine.com/lab/learningpath/3f769132d33ea977f12aa9364483426e9d484b42ac0710f44848cfa261f22a5f.png)

We can also perform an Nmap port scan on the target system by clicking on the hosts menu in the toolbar and the Nmap Scan option in the submenu as shown in the following screenshot.

![Content Image](https://assets.ine.com/lab/learningpath/22aa77fd1301381984e71149d80adb2307dfd64f3ca0aa1ddacf6bf6ebed6789.png)

You will then be prompted to add the target machine's address as shown in the following screenshot.

Enter "demo1.ine.local" and click on "OK."

![Content Image](https://assets.ine.com/lab/learningpath/c2d186dff46d596b1491aee565de0f9ce409f0cb978144873d83e348f1a4125d.png)

This will begin the Nmap port scan; wait for the scan to get completed.

![Content Image](https://assets.ine.com/lab/learningpath/2701f72dc61b5d5d07f3cf534c37ff4cd16efff96345b1b9254a32e93b594298.png)

After which, the results will be displayed in the output pane at the bottom as shown in the following screenshot.

![Content Image](https://assets.ine.com/lab/learningpath/1a3f4a91b7b80f80bfc7898c882176f077480c6d28f737848d46d2039c9373e5.png)

You can now view the open ports and services running on the target system by right-clicking on the system and clicking on services as shown in the following screenshot.

![Content Image](https://assets.ine.com/lab/learningpath/95e4087dd1c144ade15510e37c4b91fa66eb2a224a2576b5983787e966bd778d.png)

As shown in the following screenshot, this will display the list of open ports and services discovered during the port scan.

![Content Image](https://assets.ine.com/lab/learningpath/b72ef7a72d00525be5abccb59ce83b192da873fd51d8e4ea2a774577e29a2f83.png)

# Conclusion

In this lab, we explored the process of performing port scanning and enumeration with Armitage.