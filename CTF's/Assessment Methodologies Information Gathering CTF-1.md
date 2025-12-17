TARGET: http://target.ine.local

**Flag 1:** This tells search engines what to and what not to avoid.

means robots.txt

http://target.ine.local/robots.txt

```
User-agent: *
Disallow: /wp-admin/
Allow: /wp-admin/admin-ajax.php

FLAG1{c68988b7251f42459ce9bea370d0b98f}
```


____________

**Flag 2:** What website is running on the target, and what is its version?

i used "whatweb" Tool

```
whatweb http://target.ine.local

http://target.ine.local [200 OK] Apache[2.4.41], Country[RUSSIAN FEDERATION][RU], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[192.124.197.3], MetaGenerator[WordPress 6.5.3 - FL@G2{704d19c49c00424aa2ec9aedb15324a1}], Script[importmap,module], Title[INE], UncommonHeaders[link], WordPre
```

____________

**Flag 3:** Directory browsing might reveal where files are stored.

means use directory spraying

so use "dirb"

```
└─# dirb http://target.ine.local                                                                                                                                                           

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Fri Sep 19 16:48:11 2025
URL_BASE: http://target.ine.local/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://target.ine.local/ ----
+ http://target.ine.local/index.php (CODE:301|SIZE:0)                                                                                                                                     
+ http://target.ine.local/robots.txt (CODE:200|SIZE:108)                                                                                                                                  
+ http://target.ine.local/server-status (CODE:403|SIZE:281)                                                                                                                               
==> DIRECTORY: http://target.ine.local/wp-admin/                                                                                                                                          
==> DIRECTORY: http://target.ine.local/wp-content/                                                                                                                                        
==> DIRECTORY: http://target.ine.local/wp-includes/                                                                                                                                       
+ http://target.ine.local/xmlrpc.php (CODE:405|SIZE:42)                                                                                                                                   
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-admin/ ----
+ http://target.ine.local/wp-admin/admin.php (CODE:302|SIZE:0)                                                                                                                            
==> DIRECTORY: http://target.ine.local/wp-admin/css/                                                                                                                                      
==> DIRECTORY: http://target.ine.local/wp-admin/images/                                                                                                                                   
==> DIRECTORY: http://target.ine.local/wp-admin/includes/                                                                                                                                 
+ http://target.ine.local/wp-admin/index.php (CODE:302|SIZE:0)                                                                                                                            
==> DIRECTORY: http://target.ine.local/wp-admin/js/                                                                                                                                       
==> DIRECTORY: http://target.ine.local/wp-admin/maint/                                                                                                                                    
==> DIRECTORY: http://target.ine.local/wp-admin/network/                                                                                                                                  
==> DIRECTORY: http://target.ine.local/wp-admin/user/                                                                                                                                     
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-content/ ----
+ http://target.ine.local/wp-content/index.php (CODE:200|SIZE:0)                                                                                                                          
==> DIRECTORY: http://target.ine.local/wp-content/plugins/                                                                                                                                
==> DIRECTORY: http://target.ine.local/wp-content/themes/                                                                                                                                 
==> DIRECTORY: http://target.ine.local/wp-content/uploads/                                                                                                                                
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-includes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-admin/css/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-admin/images/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-admin/includes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-admin/js/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-admin/maint/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-admin/network/ ----
+ http://target.ine.local/wp-admin/network/admin.php (CODE:302|SIZE:0)                                                                                                                    
+ http://target.ine.local/wp-admin/network/index.php (CODE:302|SIZE:0)                                                                                                                    
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-admin/user/ ----
+ http://target.ine.local/wp-admin/user/admin.php (CODE:302|SIZE:0)                                                                                                                       
+ http://target.ine.local/wp-admin/user/index.php (CODE:302|SIZE:0)                                                                                                                       
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-content/plugins/ ----
+ http://target.ine.local/wp-content/plugins/index.php (CODE:200|SIZE:0)                                                                                                                  
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-content/themes/ ----
+ http://target.ine.local/wp-content/themes/index.php (CODE:200|SIZE:0)                                                                                                                   
                                                                                                                                                                                          
---- Entering directory: http://target.ine.local/wp-content/uploads/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
-----------------
END_TIME: Fri Sep 19 16:48:22 2025
DOWNLOADED: 32284 - FOUND: 13

```

we found 
```
 http://target.ine.local/wp-content/uploads
```

output:
```
Index of /wp-content/uploads
[ICO]	Name	Last modified	Size	Description
[PARENTDIR]	Parent Directory	 	- 	 
[DIR]	2024/	2024-05-27 08:46 	- 	 
[DIR]	2025/	2025-09-19 10:52 	- 	 
[TXT]	flag.txt	2025-09-19 10:49 	40 	 
```

```
FLAG3{15bf0a581cbb4977ab71138db2f6c655}
```

_________

**Flag 4:** An overlooked backup file in the webroot can be problematic if it reveals sensitive configuration details.

means find .bak find in directory fuzzing or spraying

so use "dirb" tools

```
─# dirb http://target.ine.local -w /usr/share/dirb/wordlists/big.txt -X .bak

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Fri Sep 19 16:59:47 2025
URL_BASE: http://target.ine.local/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
OPTION: Not Stopping on warning messages
EXTENSIONS_LIST: (.bak) | (.bak) [NUM = 1]

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://target.ine.local/ ----
+ http://target.ine.local/wp-config.bak (CODE:200|SIZE:3438)                                                                                                                              
                                                                                                                                                                                          
-----------------
END_TIME: Fri Sep 19 16:59:49 2025
DOWNLOADED: 4612 - FOUND: 1

```

use "curl"

```
curl http://target.ine.local/wp-config.bak
```

or

use brwoser to download wp-config.bak 

```
FLAG4{3da1d17550844c40a81674af1202dd9b} 
```

_________

**Flag 5:** Certain files may reveal something interesting when mirrored.

means we have to downlaod  offline brwoers of "http://target.ine.local"

then read all files 

```
─# cd target.ine.local/                                                                                                                                                       

┌──(root㉿INE)-[~/target/target.ine.local]
└─# ls                                                                                                                                                                                     
index905b.html  indexcff1.html  index.html  index.php  wp-admin  wp-content  wp-includes  wp-login56d7.html  wp-loginc2b6.html  wp-login.html  xmlrpc0db0.php

┌──(root㉿INE)-[~/target/target.ine.local]
└─# cat xmlrpc0db0.php | grep flag                                                                                                                                                         

┌──(root㉿INE)-[~/target/target.ine.local]
└─# cat xmlrpc0db0.php | grep "FLAG"
                        <api name="FLAG5{38e7ee65c3bc4f9ab59382bb514e60f3}" blogID="1" preferred="false" apiLink="http://target.ine.local/xmlrpc.php" />

```

```
"FLAG5{38e7ee65c3bc4f9ab59382bb514e60f3}
```

