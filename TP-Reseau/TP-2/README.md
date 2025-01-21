# TP2 : Environnement virtuel

## I. Topologie réseau

### Compte-rendu    

### ☀️ Sur node1.lan1.tp2

- afficher ses cartes réseau

```
[aymeric@node1 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:5e:52:d2 brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.11/24 brd 10.1.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe5e:52d2/64 scope link
       valid_lft forever preferred_lft forever
```
- afficher sa table de routage
```
[aymeric@node1 ~]$ ip r s
default via 10.1.1.254 dev enp0s3 proto static metric 100
10.1.1.0/24 dev enp0s3 proto kernel scope link src 10.1.1.11 metric 100
```
- prouvez qu'il peut joindre node2.lan2.tp2

```
[aymeric@node1 ~]$ ping 10.1.2.12
PING 10.1.2.12 (10.1.2.12) 56(84) bytes of data.
64 bytes from 10.1.2.12: icmp_seq=1 ttl=63 time=2.31 ms
64 bytes from 10.1.2.12: icmp_seq=2 ttl=63 time=2.29 ms
64 bytes from 10.1.2.12: icmp_seq=3 ttl=63 time=5.78 ms
64 bytes from 10.1.2.12: icmp_seq=4 ttl=63 time=1.95 ms
^C
--- 10.1.2.12 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 1.946/3.081/5.780/1.564 ms
```
- prouvez avec un traceroute que le paquet passe bien par router.tp2
```
[aymeric@node1 ~]$ traceroute 10.1.2.12
traceroute to 10.1.2.12 (10.1.2.12), 30 hops max, 60 byte packets
 1  _gateway (10.1.1.254)  1.929 ms  2.119 ms  1.713 ms
 2  10.1.2.12 (10.1.2.12)  8.944 ms !X  9.852 ms !X  9.024 ms !X
```
## II. Interlude accès internet

### ☀️ Sur router.tp2

- prouvez que vous avez un accès internet (ping d'une IP publique)

```
[aymeric@routeur ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=113 time=16.7 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=113 time=18.1 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 16.684/17.415/18.146/0.731 ms
```

- prouvez que vous pouvez résoudre des noms publics (ping d'un nom de domaine public)

```
[aymeric@routeur ~]$ ping google.com
PING google.com (216.58.214.78) 56(84) bytes of data.
64 bytes from fra15s10-in-f78.1e100.net (216.58.214.78): icmp_seq=1 ttl=116 time=18.2 ms
64 bytes from fra15s10-in-f78.1e100.net (216.58.214.78): icmp_seq=2 ttl=116 time=18.3 ms
^C
--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 18.212/18.271/18.330/0.059 ms
```

### ☀️ Accès internet LAN1 et LAN2

- il peut ping une IP publique

```
[aymeric@node2 ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=112 time=28.2 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=112 time=23.7 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=112 time=19.9 ms
^C
--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 19.854/23.917/28.209/3.414 ms
```
- il peut ping un nom de domaine public

```
[aymeric@node2 ~]$ ping google.com
PING google.com (216.58.214.78) 56(84) bytes of data.
64 bytes from fra15s10-in-f78.1e100.net (216.58.214.78): icmp_seq=1 ttl=115 time=19.6 ms
64 bytes from par10s39-in-f14.1e100.net (216.58.214.78): icmp_seq=2 ttl=115 time=22.1 ms
64 bytes from par10s39-in-f14.1e100.net (216.58.214.78): icmp_seq=3 ttl=115 time=22.2 ms
^C
--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2006ms
rtt min/avg/max/mdev = 19.631/21.315/22.243/1.192 ms
```

## III. Services réseau

### ☀️ Sur node1.lan1.tp2


```
[aymeric@node1 ~]$ curl site_nul.tp2
<h1>Je te remplit</h1>
```

## 2. Or is it ? Bonus : DHCP

```
[aymeric@node2 ~]$ sudo dnf -y install dhcp-server
[aymeric@node2 ~]$ sudo nano /etc/dhcp/dhcpd.conf

option domain-name     "dns_nul";
option domain-name-servers     1.1.1.1;
default-lease-time 600;
max-lease-time 7200;
authoritative;

subnet 10.1.1.0 netmask 255.255.255.0 {
    range dynamic-bootp 10.1.1.100 10.1.1.200;
    option broadcast-address 10.1.1.255;
    option routers 10.1.1.254;
}

[aymeric@dhcp ~]$ sudo systemctl enable --now dhcpd
[aymeric@dhcp ~]$ sudo firewall-cmd --add-service=dhcp
success
[aymeric@dhcp ~]$ sudo firewall-cmd --runtime-to-permanent
success

[aymeric@node1 ~]$ sudo cat /etc/sysconfig/network-scripts/ifcfg-enp0s3
DEVICE=enp0s3

BOOTPROTO=dhcp
ONBOOT=yes

[aymeric@node1 ~]$ sudo dhclient enp0s3
[aymeric@node1 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:5e:52:d2 brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.100/24 brd 10.1.1.255 scope global dynamic noprefixroute enp0s3
       valid_lft 463sec preferred_lft 463sec
    inet 10.1.1.101/24 brd 10.1.1.255 scope global secondary dynamic enp0s3
       valid_lft 659sec preferred_lft 659sec
    inet6 fe80::a00:27ff:fe5e:52d2/64 scope link
       valid_lft forever preferred_lft forever
```
