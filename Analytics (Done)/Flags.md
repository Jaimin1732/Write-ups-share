### User.txt
```
metalytics@analytics:~$ whoami && hostname && ifconfig && cat user.txt
metalytics
analytics
docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:edff:fea1:b7ae  prefixlen 64  scopeid 0x20<link>
        ether 02:42:ed:a1:b7:ae  txqueuelen 0  (Ethernet)
        RX packets 42909  bytes 209835446 (209.8 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 20206  bytes 1430761 (1.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.129.149.35  netmask 255.255.0.0  broadcast 10.129.255.255
        inet6 dead:beef::250:56ff:feb0:b4ed  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::250:56ff:feb0:b4ed  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:b0:b4:ed  txqueuelen 1000  (Ethernet)
        RX packets 525861  bytes 59383128 (59.3 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 486060  bytes 337496141 (337.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 65577  bytes 15347519 (15.3 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 65577  bytes 15347519 (15.3 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth09cdfad: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::f0ae:f2ff:feff:ce1a  prefixlen 64  scopeid 0x20<link>
        ether f2:ae:f2:ff:ce:1a  txqueuelen 0  (Ethernet)
        RX packets 42909  bytes 210436172 (210.4 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 20249  bytes 1433867 (1.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

1233a15dd2b70ffba308f86c475d388e
```

![[Pasted image 20231010155540.png]]


### proof.txt

```
root@analytics:/tmp# whoami && hostname && ifconfig && cat /root/root.txt
root
analytics
docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:50ff:feff:a15e  prefixlen 64  scopeid 0x20<link>
        ether 02:42:50:ff:a1:5e  txqueuelen 0  (Ethernet)
        RX packets 4  bytes 136 (136.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 9  bytes 758 (758.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.10.11.233  netmask 255.255.254.0  broadcast 10.10.11.255
        inet6 fe80::250:56ff:feb9:8052  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:b9:80:52  txqueuelen 1000  (Ethernet)
        RX packets 1742  bytes 145667 (145.6 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1694  bytes 185055 (185.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 236  bytes 17421 (17.4 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 236  bytes 17421 (17.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth7eef40e: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::1001:51ff:fee1:2b2d  prefixlen 64  scopeid 0x20<link>
        ether 12:01:51:e1:2b:2d  txqueuelen 0  (Ethernet)
        RX packets 4  bytes 192 (192.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 18  bytes 1484 (1.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

de49e20187b2ece330457c539684582d

```

![[Pasted image 20231011205320.png]]

