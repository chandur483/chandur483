A web server is software that is used to serve website data on the web.  
+ Web servers utilize HTTP (Hypertext Transfer Protocol) to facilitate the communication between clients and the web server. 
+ HTTP is an application layer protocol that utilizes TCP port 80 for communication. + We can utilize auxiliary modules to enumerate the web server version, HTTP headers, brute-force directories and much more. 
+ Examples of popular web servers are; Apache, Nginx and Microsoft IIS

**we enumerate web using auxiliarys scaanner in metasploit-framework 

**first thing to do

## check postgresql stauts

```
service postgresql start
```

## then start metasploit
```
msfconsole
```

## create a workspace
```
workspace -a new
```

## set RHOSTS gobally
```
setg RHOSTS <target ip>
```

## now search for auxiliary scanner for http
```
search type:auxiliary nmae:http
```

check http version 

```
use auxiliary/scanner/http/http_version
```

check http header

```
use auxiliary/scanner/http/http_header
```

check robots.txt

```
use auxiliary/scanner/http/robots_txt
```

**then use *curl* tools to see directory

```
use auxiliary/scanner/http/dir_scaanner
```
this command enum directorys persent in wed

now check for diff file in that directorys.
```
use auxiliary/scanner/http/files_dir
```

## some directory needs authentication to access we need to brute force

```
use auxiliary/scanner/http/http_login
```

if success OK 
or
if not then find a valid username

i this case current web service uses apache
```
use auxiliary/scanner/http/apache_userdir_enum
```
