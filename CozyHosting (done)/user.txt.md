```
josh@cozyhosting:~$ cat user.txt
03d213d526fe72664d4e21d95987172c
```

![[Pasted image 20230905221227.png]]


# Root
![[Pasted image 20230905222046.png]]

```
root@cozyhosting:~# cat root.txt && whoami && ifconfig
5cb4228dbb05b7e8e5079c18c44f0528
root
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.129.105.21  netmask 255.255.0.0  broadcast 10.129.255.255
        inet6 dead:beef::250:56ff:feb0:cc92  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::250:56ff:feb0:cc92  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:b0:cc:92  txqueuelen 1000  (Ethernet)
        RX packets 115199  bytes 12335209 (12.3 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 105056  bytes 142465823 (142.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 236395  bytes 100951441 (100.9 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 236395  bytes 100951441 (100.9 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

root@cozyhosting:~#
```