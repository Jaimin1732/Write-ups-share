### Site 
After getting nmap scan, I decided to check out the website and I found that the website has a login page, so I went on there and added "data.analytical.htb" to the /etc/hosts file alongside "analytical.htb".

![[Pasted image 20231010153515.png]]

```
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/Analytics]
└─# cat /etc/hosts                                     
127.0.0.1	localhost
127.0.1.1	kali
10.129.149.35	analytical.htb	data.analytical.htb

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

```

![[Pasted image 20231010153722.png]]

### POC
After finding the metabase login page, I decided to google "metabase exploit" and I found this site, Which shows me how to get initial access.
**site**: https://blog.assetnote.io/2023/07/22/pre-auth-rce-metabase/
![[Pasted image 20231010154123.png]]