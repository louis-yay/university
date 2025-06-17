## Routage
On effectue la commande `/sbin/route -n` et on récupère la table suivante:
```c
Table de routage IP du noyau
Destination     Passerelle      Genmask         Indic Metric Ref    Use Iface
0.0.0.0         10.0.103.254    0.0.0.0         UG    0      0        0 eth0
10.0.103.0      0.0.0.0         255.255.255.0   U     0      0        0 eth0
```
- Mon voisin à bien la même adresse destination pour le réseau locale, `10.0.103.0`
- L'adresse de la passerelle pour la route par défaut est `10.0.103.254`

Avec `ip route ls` on a le resultat:
```c
default via 10.0.103.254 dev eth0 onlink 
10.0.103.0/24 dev eth0 proto kernel scope link src 10.0.103.3 
```
- Où `\24` correspond à la taille du masque, ici, de 24 bits.

Avec `ip -6 route ls` on obtient:
```c
2001:660:6101:800:103::/80 dev eth0 proto kernel metric 256 pref medium
fe80::/64 dev eth0 proto kernel metric 256 pref medium
default via fe80::5a20:b1ff:feb1:2300 dev eth0 proto ra metric 1024 expires 8821sec hoplimit 25 pref medium
```
- Avec le masque \80 on a la partie réseau: 
	- `2001:660:6101:800:103`, qui correspond à 20 chiffres hexadécimaux

On se connecte à distance avec SSH, et on active la virtual box:
```c
/net/ens/qemunet/qemunet.sh -b -d tmux -s /net/ens/qemunet/demo/gw1.tgz
tmux a
```



### Routage Basique
Soit le schémas suivant
```
   grave  
     |  
 ---------- 
	 | 
  immortal 
	 | 
-------------- 
   |       | 
 opeth    syl
```
Et les adresses IP:
- `grave eth0 147.210.0.2`
- `immortal eth0 147.210.0.1`
- `immortal eth1 192.168.0.1`
- `opeth eth0 192.168.0.2`
- `syl eth0 192.168.0.3`

*depuis opeth:*
```c
root@opeth:~# ping 192.168.0.3
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_seq=1 ttl=64 time=0.626 ms
64 bytes from 192.168.0.3: icmp_seq=2 ttl=64 time=0.266 ms
64 bytes from 192.168.0.3: icmp_seq=3 ttl=64 time=0.427 ms
```
Les machines peuvent communiquer entre elles au sein du réseau local

*depuis opeth*
```c
root@opeth:~# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
192.168.0.0     0.0.0.0         255.255.255.0   U     0      0        0 eth0
```

*depuis immortal*
```c
root@immortal:~# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
147.210.0.0     0.0.0.0         255.255.255.0   U     0      0        0 eth0
192.168.0.0     0.0.0.0         255.255.255.0   U     0      0        0 eth1
```

Pour permettre une communication entre les réseaux on configure dans un premier temps le mode routeur pour la machine immortal:
```c
echo 1 > /proc/sys/net/ipv4/ip_forward
```

On configure ensuite les passerelles par défauts des autres machines:
```c
// Grave:
route add default gw 147.210.0.1

// Opeth & Syl
route add default gw 192.168.0.1
```

Une fois la configuration faite, on peut ping depuis *opeth*
```c
root@opeth:~# ping 147.210.0.2
PING 147.210.0.2 (147.210.0.2) 56(84) bytes of data.
64 bytes from 147.210.0.2: icmp_seq=1 ttl=63 time=1.25 ms
64 bytes from 147.210.0.2: icmp_seq=2 ttl=63 time=0.725 ms
64 bytes from 147.210.0.2: icmp_seq=3 ttl=63 time=0.721 ms
```

Et depuis immortal on effectue un `tcpdump -i any`
```c
root@immortal:~# tcpdump -n -i any
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on any, link-type LINUX_SLL (Linux cooked v1), capture size 262144 bytes
10:45:17.000992 IP 192.168.0.2 > 147.210.0.2: ICMP echo request, id 567, seq 1, length 64
10:45:17.001014 IP 192.168.0.2 > 147.210.0.2: ICMP echo request, id 567, seq 1, length 64
10:45:17.001488 IP 147.210.0.2 > 192.168.0.2: ICMP echo reply, id 567, seq 1, length 64
10:45:17.001493 IP 147.210.0.2 > 192.168.0.2: ICMP echo reply, id 567, seq 1, length 64
```


### Routage avancé
Pour le routage plus avancé on configuration de la manière suivante:
147.210.12.1
*Opeth*
```c
route add -net 147.210.13.0/24 gw 147.210.12.2
route add -net 147.210.14.0/24 gw 147.210.12.2
route add -net 147.210.15.0/24 gw 147.210.12.2
```

Immortal
```c
echo 1 > /proc/sys/net/ipv4/ip_forward
route add -net 147.210.14.0/24 gw 147.210.13.2
route add -net 147.210.15.0/24 gw 147.210.13.2
```

*Grave*
```c
echo 1 > /proc/sys/net/ipv4/ip_forward
route add -net 147.210.12.0/24 gw 147.210.13.1
route add -net 147.210.15.0/24 gw 147.210.15.1
```

*Syl*
```c
echo 1 > /proc/sys/net/ipv4/ip_forward
route add -net 147.210.13.0/24 gw 147.210.14.1
route add -net 147.210.12.0/24 gw 147.210.14.1
```

*Nile*
```c
route add -net 147.210.12.0/24 gw 147.210.15.1
route add -net 147.210.13.0/24 gw 147.210.15.1
route add -net 147.210.14.0/24 gw 147.210.15.1
```

## Firewall
On lance la virtual Box
```c
/net/ens/qemunet/qemunet.sh -b -d tmux -s /net/ens/qemunet/demo/dmz.tgz
tmux a
```

Dans le réseau, DMZ et le réseau internet des employé ne sont pas dans les mêmes sous réseaux.

Une fois le firewall mis en place:
```c
iptables -P INPUT DROP 
iptables -P OUTPUT DROP 
iptables -P FORWARD DROP
```

On fait un ping de nile vers syl et on constate que les paquets ne sont pas transmis
```c
root@nile:~# ping 192.168.0.2
PING 192.168.0.2 (192.168.0.2) 56(84) bytes of data.
--- 192.168.0.2 ping statistics ---
4 packets transmitted, 0 received, 100% packet loss, time 3024ms
```

On configure le firewall
```c
// Ping vers internet non réciproque
iptables -A FORWARD -s 192.168.0.0/24 -p icmp -j ACCEPT
iptables -A FORWARD -s 192.168.1.0/24 -p icmp -j ACCEPT
iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT

// Accès au web
iptables -A FORWARD -s 192.168.0.0/24 -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -s 192.168.1.0/24 -p tcp --dport 80 -j ACCEPT

// Ssh de grave -> dt
iptables -A FORWARD -s 172.16.0.2 -d 192.168.0.3/24 -p tcp --dport 22 -j ACCEPT

// Connexion vers syl en port 80
iptables -A FORWARD -d 192.168.0.2/24 -p tcp --dport 80 -j ACCEPT
```