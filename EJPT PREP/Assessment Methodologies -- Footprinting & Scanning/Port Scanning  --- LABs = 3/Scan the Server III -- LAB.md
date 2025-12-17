**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/2eafb66b920ea80841896da10d55144fc1d50e228fe08d083e49ec1bc635385f.png)

**Step 2:** Check if the target machine is reachable:

**Command:**

```
ping -c 4 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/454e7bef4c62766a314fed227126ab35ab805b1a0e42b45b3cadeadb4ef5e9fa.png)

The target is reachable.

**Step 3:** Port scanning with Nmap

To begin with, we can perform an Nmap port scan on the entire TCP port range (65,535 ports) to identify all the open ports on the target system. This can be done by running the following command:

**Command:**

```
nmap demo.ine.local -T4 -p-
```

As shown in the following screenshot, target system does not have any open TCP ports.

![Content Image](https://assets.ine.com/lab/learningpath/d88b97fd9f395712d84614adad24e047075d603dd40ea33ba768be114998a06d.png)

Given that the target system does not have any TCP ports open, we can perform a UDP port scan do discover any open UDP ports on the target system.

This can be done by running the following command:

**Command:**

```
nmap demo.ine.local -T4 -sU
```

As shown in the following screenshot, the Nmap scan reveals that the target system only has one UDP port open (port 161) that is typically used by the SNMP service.

![Content Image](https://assets.ine.com/lab/learningpath/586dedf5b92c32f72bd445dc4daa730fe038fe86539e9aedc532defc36ff456c.png)

**Step 4:** Service detection with Nmap

Now that we have identified the open UDP port on the target, we can learn more about the service running on the open port by performing a service detection and script scan with Nmap.

This can be done by running the following command:

**Command:**

```
nmap demo.ine.local -T4 -sU -p 161 -A
```

As shown in the following screenshot, the Nmap service detection scan confirms that an SNMP server is running on port 161, the scan also enumerates information from the SNMP server.

![Content Image](https://assets.ine.com/lab/learningpath/c409ebc15e82cebc7faf589254e04ff0a6d6c61b26185422e64204e7495ff4a1.png)

![Content Image](https://assets.ine.com/lab/learningpath/585cc4898d76a8d9bf2931f45518d166eab7f54497764f1a79c58ea3d555a4cc.png)

Go through the results produced by the aforementioned Nmap scan to learn more about the target system.

# Conclusion

In this lab, we explored the process of performing port scanning and service detection with Nmap.