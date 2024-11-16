# Explanation
After running nmap automator, I proceeded forward to run dirsearch and found a website at 'http://cozyhosting.htb/actuator'.  Once I visited the site I was shown all the sub folders list. I then checked the sessions subfolder and I was given a session cookie and its username. I then put that session cookie into a cookie editor and then refreshed the page, which then gave me access to the /admin page of the website. After getting access to the admin page, I started to test the field and I saw that the connection was timing out which indicated that I could possibly get command injection. 

## Dirsearch
```bash
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/CozyHosting]
└─# dirsearch -u http://cozyhosting.htb

  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10927

Output File: /root/.dirsearch/reports/cozyhosting.htb/_23-09-05_20-20-00.txt

Error Log: /root/.dirsearch/logs/errors-23-09-05_20-20-00.log

Target: http://cozyhosting.htb/

[20:20:00] Starting: 
[20:20:07] 200 -    0B  - /Citrix//AccessPlatform/auth/clientscripts/cookies.js
[20:20:09] 400 -  435B  - /\..\..\..\..\..\..\..\..\..\etc\passwd
[20:20:10] 400 -  435B  - /a%5c.aspx
[20:20:10] 200 -  634B  - /actuator
[20:20:10] 200 -    5KB - /actuator/env
[20:20:10] 200 -   15B  - /actuator/health
[20:20:10] 200 -  198B  - /actuator/sessions
[20:20:10] 200 -   10KB - /actuator/mappings
[20:20:10] 200 -  124KB - /actuator/beans
[20:20:11] 401 -   97B  - /admin
[20:20:26] 200 -    0B  - /engine/classes/swfupload//swfupload.swf
[20:20:26] 200 -    0B  - /engine/classes/swfupload//swfupload_f9.swf
[20:20:26] 500 -   73B  - /error
[20:20:26] 200 -    0B  - /examples/jsp/%252e%252e/%252e%252e/manager/html/
[20:20:26] 200 -    0B  - /extjs/resources//charts.swf
[20:20:29] 200 -    0B  - /html/js/misc/swfupload//swfupload.swf
[20:20:30] 200 -   12KB - /index
[20:20:32] 200 -    4KB - /login
[20:20:32] 200 -    0B  - /login.wdm%2e
[20:20:33] 204 -    0B  - /logout
[20:20:43] 400 -  435B  - /servlet/%C0%AE%C0%AE%C0%AF

Task Completed 
```

## Actuator Webpage
![[Pasted image 20230905202258.png]]

# Actuator Sessions file

```
C30D7A10EA7C839B40FAB44B0CE9E9F4	"kanderson"
```

# Cookie Editor
![[Pasted image 20230905205335.png]]

# /admin page
![[Pasted image 20230905205414.png]]

# Command Injection
![[Pasted image 20230905211947.png]]
