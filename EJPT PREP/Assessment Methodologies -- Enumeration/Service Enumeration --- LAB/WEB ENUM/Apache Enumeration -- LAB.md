Apache enumeration is a crucial step in the reconnaissance phase of a penetration test, where the goal is to gather as much information as possible about the Apache web server being targeted. This information can help in identifying potential vulnerabilities or misconfigurations that can be exploited later in the test. In this lab, we will learn about Apache enumeration using the Metasploit framework modules.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine will be accessible at "victim-1".

**Objective:** Run the following auxiliary modules against the target (victim-1):

- auxiliary/scanner/http/apache_userdir_enum
- auxiliary/scanner/http/brute_dirs
- auxiliary/scanner/http/dir_scanner
- auxiliary/scanner/http/dir_listing
- auxiliary/scanner/http/http_put
- auxiliary/scanner/http/files_dir
- auxiliary/scanner/http/http_login
- auxiliary/scanner/http/http_header
- auxiliary/scanner/http/http_version
- auxiliary/scanner/http/robots_txt

# Tools:

- Metasploit Framework
____________

**Step 1:** Open the lab link to access the Kali machine.

![Content Image](https://assets.ine.com/lab/learningpath/1a76fa579cff97588497606e0cc59592f14d3b2393fd36b553a505dd2e622740.png)

**Step 2:** Check if the target machine is reachable:

**Command:**

```
ping -c 5 victim-1
```

![Content Image](https://assets.ine.com/lab/learningpath/e73f8f6fa6d30b5e10528a30c425b93b3b101dc2a856160a1a92ea676d4c45f1.png)

The target is reachable.

**Step 3:** Open the Metasploit framework console.

**Command:**

```
msfconsole -q
```

![Content Image](https://assets.ine.com/lab/learningpath/105ff30c4657d11ac1e3008c4935912c5d319c1ddbb28ac1dcbcdd1419a81982.png)

**Step 4:** Run the Metasploit auxiliary modules against the target one-by-one.

**Module 1:** auxiliary/scanner/http/http_version

**Commands:**

```
use auxiliary/scanner/http/http_version
set RHOSTS victim-1
run
```

![Content Image](https://assets.ine.com/lab/learningpath/d08db3bdc1bfab75eb6413fdc6a8f3740cbbf7a670813e5d91cf7e4bc5c44941.png)

**Module 2:** auxiliary/scanner/http/robots_txt

**Commands:**

```
use auxiliary/scanner/http/robots_txt
set RHOSTS victim-1
run
```

![Content Image](https://assets.ine.com/lab/learningpath/ff5d4835fbd8966e9f0db6d50c4433288c23de2ae61173b5a3400adff33545fc.png)

**Module 3:** auxiliary/scanner/http/http_header

**Commands:**

```
use auxiliary/scanner/http/http_header
set RHOSTS victim-1
run
```

![Content Image](https://assets.ine.com/lab/learningpath/d0066730416e74a2714e1468b1ae6ca3b58c633caf4052ffb296c3662de10dc5.png)

**Commands:**

```
use auxiliary/scanner/http/http_header
set RHOSTS victim-1
set TARGETURI /secure
run
```

![Content Image](https://assets.ine.com/lab/learningpath/ccdc51308e0ace01fe5032feb0ee172c2d1e338b96437d434cce4709ae6363c5.png)

**Module 4:** auxiliary/scanner/http/brute_dirs

**Commands:**

```
use auxiliary/scanner/http/brute_dirs
set RHOSTS victim-1
run
```

![Content Image](https://assets.ine.com/lab/learningpath/518b152b9cdb5daaf78e56ff25b10f1fd358054c2e4ad25217f6955b8a866a26.png)

**Module 5:** auxiliary/scanner/http/dir_scanner

**Commands:**

```
use auxiliary/scanner/http/dir_scanner
set RHOSTS victim-1
set DICTIONARY /usr/share/metasploit-framework/data/wordlists/directory.txt
run
```

![Content Image](https://assets.ine.com/lab/learningpath/c317c006f97616435993e2aeda558013d5f9479d8bb9a49492a5c9ee40713d73.png)

**Module 6:** auxiliary/scanner/http/dir_listing

**Commands:**

```
use auxiliary/scanner/http/dir_listing
set RHOSTS victim-1
set PATH /data
run
```

![Content Image](https://assets.ine.com/lab/learningpath/6a1103a62e9e5737080109552d8142f20e468f6478f3b6f8447ea2a3573958e8.png)

**Module 7:** auxiliary/scanner/http/files_dir

**Command:**

```
use auxiliary/scanner/http/files_dir
set RHOSTS victim-1
set VERBOSE false
run
```

![Content Image](https://assets.ine.com/lab/learningpath/c650c928d2a095b20c5d378804b89eae4bd473aec377c9352710c79ea16603d9.png)

![Content Image](https://assets.ine.com/lab/learningpath/77fd30d41d908cd0ef0d4088a84a6679041ee6e2c7ccc6d775bd19a216c86b78.png)

**Module 8:** auxiliary/scanner/http/http_put

**Commands:**

```
use auxiliary/scanner/http/http_put
set RHOSTS victim-1
set PATH /data
set FILENAME test.txt
set FILEDATA "Welcome To AttackDefense"
run
```

![Content Image](https://assets.ine.com/lab/learningpath/9746dc4d0a923fea584a486dcd80dd44aff0aa1b9541ba768344dab24980035b.png)

We can observe that we have successfully written a file on the target server. If the file is already exists it will overwrite it. Let’s use wget and download the test.txt file and verify it.

**Commands:**

```
wget http://victim-1:80/data/test.txt 
cat test.txt
```

![Content Image](https://assets.ine.com/lab/learningpath/596f21ad6fab8c7fbdf24a0ceddab0933e2113190750d904a479bc959e493f3c.png)

We can download the text.txt file and we can see it’s content i.e "Welcome To AttackDefense"

Now, let’s use DELETE method and delete the text.file

**Commands:**

```
use auxiliary/scanner/http/http_put
set RHOSTS victim-1
set PATH /data
set FILENAME test.txt
set ACTION DELETE
run
```

![Content Image](https://assets.ine.com/lab/learningpath/a562fd9cc71d5ff19b941e5a34122df2b5df629cb26f70fdbb193a4582c382e5.png)

Let’s try to download the same file from the same path. This time we should receive 404 error. i.e file not found. Because we have deleted it.

**Command:**

```
wget http://victim-1:80/data/test.txt 
```

![Content Image](https://assets.ine.com/lab/learningpath/d242d6b32467ad39d96c9a9e6b7e66855a885f1e3e49f57d87b862a6d4191983.png)

**Module 9:** auxiliary/scanner/http/http_login

**Commands:**

```
use auxiliary/scanner/http/http_login
set RHOSTS victim-1
set AUTH_URI /secure/
set VERBOSE false
run
```

![Content Image](https://assets.ine.com/lab/learningpath/4fd42ea1a79698903db679df2690aee81583e131b227ceddea173e4b5bb710a6.png)

**Module 10:** auxiliary/scanner/http/apache_userdir_enum

**Commands:**

```
use auxiliary/scanner/http/apache_userdir_enum
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set RHOSTS victim-1
set VERBOSE false
run
```

![Content Image](https://assets.ine.com/lab/learningpath/94ab0cee40cf15eb8bf87c2340a2945fb322da7dd0f7d7ef29fcbbf87631b565.png)

# Conclusion

In this lab, we learned about Apache enumeration using the Metasploit framework modules.

# References

1. [Apache](https://httpd.apache.org/)
2. Metasploit Modules:
3. [http_version](https://www.rapid7.com/db/modules/auxiliary/scanner/http/http_version)
4. [http_header](https://www.rapid7.com/db/modules/auxiliary/scanner/http/http_header)
5. [robots_txt](https://www.rapid7.com/db/modules/auxiliary/scanner/http/robots_txt)
6. [brute_dirs](https://www.rapid7.com/db/modules/auxiliary/scanner/http/brute_dirs)
7. [dir_scanner](https://www.rapid7.com/db/modules/auxiliary/scanner/http/dir_scanner)
8. [dir_listing](https://www.rapid7.com/db/modules/auxiliary/scanner/http/dir_listing)
9. [files_dir](https://www.rapid7.com/db/modules/auxiliary/scanner/http/files_dir)
10. [http_put](https://www.rapid7.com/db/modules/auxiliary/scanner/http/http_put)
11. [http_login](https://www.rapid7.com/db/modules/auxiliary/scanner/http/http_login)
12. [apache_userdir_enum](https://www.rapid7.com/db/modules/auxiliary/scanner/http/apache_userdir_enum)