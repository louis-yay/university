# 1. Interface Réseaux et adresse IP

## Question 1

Avec la commande `ifconfig` on observe sur la partie `eth0`:

- `inet 172.21.241.111` → Adresse IPV4
- `inet6 fe80::215:5dff:fea9:f075` → Adresse IPV6
- `netmask 255.255.240.0` → Taille maximal du masque réseau
- `mtu 1420` → Maximum Transition Unit, taille maximal des paquets transmis

Et de même sur la partie `lo` :

- `inet 127.0.0.1` → Adresse IPV4
- `inet6 ::1` → Adresse IPV6
- `netmask 255.0.0.0` → Taille maximal du masque réseau
- `mtu 65536` → Maximum Transmission Unit, taille maximal des paquets transmis.

Avec la commande `ip addr ls` on a:

```Bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group 
default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet 10.255.255.254/32 brd 10.255.255.254 scope global lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1420 qdisc mq state UP group 
default qlen 1000
    link/ether 00:15:5d:a9:f0:75 brd ff:ff:ff:ff:ff:ff
    inet 172.21.241.111/20 brd 172.21.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::215:5dff:fea9:f075/64 scope link
       valid_lft forever preferred_lft forever
```

  

## Question 2

Avec la commande `ipcalc 172.21.241.111\24` on obtient les détails suivants:

```Bash
Address:   172.21.241.111       10101100.00010101.11110001. 01101111
Netmask:   255.255.255.0 = 24   11111111.11111111.11111111. 00000000
Wildcard:  0.0.0.255            00000000.00000000.00000000. 11111111
=>
Network:   172.21.241.0/24      10101100.00010101.11110001. 00000000
HostMin:   172.21.241.1         10101100.00010101.11110001. 00000001
HostMax:   172.21.241.254       10101100.00010101.11110001. 11111110
Broadcast: 172.21.241.255       10101100.00010101.11110001. 11111111
Hosts/Net: 254                   Class B, Private Internet
```

  

## Question 3

On observe en effet une première différence entre les adresses IPV4 et IPV6, entre l’interface `eth0` et la boucle local `lo` .  
De plus, après comparaison avec la machine voisine, on constate que les adresse IPV4 et IPV6 de la boucle local sont identique. Cette similarité est due au contexte local de l’interface, cette dernière ne communicant pas en dehors du “réseaux interne” de la machine, il n’y est pas nécessaire d’avoir des adresse IP différentes.  

  

## Question 4

Le MTU de l’interface `eth0` (1500) est normalisé pour les échanges entres machines. Pour garantir des échanges correcte entre les différentes machines, la taille des paquets se doit d’être normaliser et relativement “courte”.  
Ce qui n’est pas le cas sur la boucle local  
`lo` qui gère en interne les communication entre les composants même de l’ordinateur et qui donc, peu avoir une taille maximal des paquets largement supérieur à celle de la partie `eth0` .

  

## Question 5

```Makefile
PING 10.0.7.11 (10.0.7.11) 56(84) bytes of data.
64 bytes from 10.0.7.11: icmp_seq=1 ttl=64 time=0.179 ms
64 bytes from 10.0.7.11: icmp_seq=2 ttl=64 time=0.123 ms
64 bytes from 10.0.7.11: icmp_seq=3 ttl=64 time=0.136 ms
64 bytes from 10.0.7.11: icmp_seq=4 ttl=64 time=0.130 ms
```

---

  

# 2. Netcat & Netstat

## Question 1

On exécute sur un premier terminal: `nc -l -p 12345`  
Après ouverture d’un second terminal, on exécute la commande suivante:  
`ss -tuan`  
On obtient alors la liste suivante:  

```Bash
Netid          State           Recv-Q          Send-Q          Local Address:Port  ...  
udp            UNCONN          0               0               127.0.0.53%lo:53     
udp            UNCONN          0               0              10.255.255.254:53     
udp            UNCONN          0               0                   127.0.0.1:323    
udp            UNCONN          0               0                       [::1]:323    
tcp            LISTEN          0               1000           10.255.255.254:53     
tcp            LISTEN          0               4096            127.0.0.53%lo:53     
tcp            LISTEN          0               1                   0.0.0.0:12345  
```

On a bien notre serveur avec son adresse locale et son port: `0.0.0.0:12345`

  

## Question 2

On execute donc la commande `netstat -p -tuan` et on récupère l’output suivant:

```Bash
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      ...
tcp        0      0 10.255.255.254:53       0.0.0.0:*               LISTEN      
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      
tcp        0      0 0.0.0.0:12345           0.0.0.0:*               LISTEN      
udp        0      0 127.0.0.53:53           0.0.0.0:*                           
udp        0      0 10.255.255.254:53       0.0.0.0:*                           
udp        0      0 127.0.0.1:323           0.0.0.0:*                           
udp6       0      0 ::1:323                 :::*                                
```

  

En se connectant, on peut échanger des données entre les deux terminaux !

![[image 10.png|image 10.png]]

![[image 1 7.png|image 1 7.png]]

## Question 3-4

En remplaçant l’IP lors de la connexion par celle de la machine de ma voisine, je peux me connecter et échanger des messages de la même manière que montré précédemment.

  

---

# 3. Protocole ARP

## Question 1

En utilisant la commande `arp`, j’obtient la liste suivante:

```Bash
Adresse           TypeMap AdresseMat          Indicateurs      Iface
10.0.7.254        ether   58:20:b1:b1:23:00   C                eth0
10.0.7.11         ether   d8:9e:f3:10:93:a0   C                eth0
```

## Question 2

Je lance un ping sur une machine au hasard au cremi (ici la machine dont l’IP est `10.0.7.3` )  
La liste est donc mise à jour:  

```bash
Adresse           TypeMap AdresseMat          Indicateurs      Iface
10.0.7.254        ether   58:20:b1:b1:23:00   C                eth0
10.0.7.11         ether   d8:9e:f3:10:93:a0   C                eth0
10.0.7.3          ether   d8:9e:f3:10:2a:f0   C                eth0
```

## Question 3

Avec `ip neigh ls` on a donc une table plus complète comprenant les adresses IPV4 et IPV6 des machine précédemment prise en compte.

```bash
10.0.7.254 dev eth0 lladdr 58:20:b1:b1:23:00 REACHABLE
10.0.7.11 dev eth0 lladdr d8:9e:f3:10:93:a0 STALE
10.0.7.3 dev eth0 lladdr d8:9e:f3:10:2a:f0 STALE
fe80::5a20:b1ff:feb1:2300 dev eth0 lladdr 58:20:b1:b1:23:00 router STALE
2001:660:6101:800:7::ffff dev eth0 lladdr 58:20:b1:b1:23:00 router STALE
```

## Question 4

Après ping des adresses `8.8.8.8` et `10.252.0.4` , ces dernières ne s’affichent pas sur la table ARP car ne font pas partie du réseaux local (LAN) sur lequel notre machine est connecté.

---

# 4. Résolution de noms (DNS)

## Question 1

Lors de la lecture du fichier `resolv.conf` on a l’adresse IPV4 suivante;

- `nameserver 127.0.0.53`

On constate que le masque de l’adresse IP est le même que le masque de la boucle local de notre machine (`127.0.0.1`). Cette adresse IP correspond à l’adresse IP du serveur DNS que notre machine va devoir interroger.

On constate également que l’adresse [http://www/](http://www/) renvoie directement sur la page d’accueil du CREMI.

  

## Question 2-3

En faisait un `host` du nom de domaine [yahoo.com](http://yahoo.com) on constate plusieurs adresses IPV4 et IPV6. Ces dernières correspondent aux différentes machines sur lesquels sont hébergé le site yahoo.com.

```bash
kira@MSI:/etc$ host yahoo.com
yahoo.com has address 98.137.11.164
yahoo.com has address 98.137.11.163
[...]
yahoo.com has IPv6 address 2001:4998:124:1507::f001
yahoo.com has IPv6 address 2001:4998:24:120d::1:1
[...]
yahoo.com mail is handled by 1 mta5.am0.yahoodns.net.
yahoo.com mail is handled by 1 mta7.am0.yahoodns.net.
yahoo.com mail is handled by 1 mta6.am0.yahoodns.net.
```

  

---

  

# Services au CREMI : LDAP & NFS

## Question 1

Les données utilisateurs sont stocké sur plusieurs serveurs du CREMI. C’est à ces derniers que se connecte la machine client afin d’y récupérer les données en question. La présence de plusieurs serveurs permet la récupération des données même si un ou plusieurs serveur du CREMI sont indisponible au moment de la connexion.

En utilisant `nslookup` on retourne les adresses IP des serveurs en question, on constate qu’ils font partie du même réseau `10.0.x.x` mais ne partagent pas le même masque que le client `10.0.220.x` contre `10.0.7.x`

```bash
Server:         127.0.0.53
Address:        127.0.0.53\#53

Name:   cresus.emi.u-bordeaux.fr
Address: 10.0.220.10
Name:   cresus.emi.u-bordeaux.fr
Address: 2001:660:6101:800:220::10
```

## Question 2

En utilisant grep on cherche les ports utilisé par ces services:  
  
`grep 'service' /etc/services`

- `LDAP` → `389/tcp` ou `389/udp`
- `http` → `80/tcp`
- `ssh` → `22/tcp`
- `x11` → `6000/tcp`

  

## Question 3

Avec `df ~` on a donc:  
  

```bash
Sys. de fichiers      blocs de 1K Utilisé Disponible Uti% Monté sur
unityaccount:/account     9437184 1216320    8220864  13% /autofs/unityaccount/cremi
```

---

# TP 02

Avec `ifconfig` on a la liste des interfaces suivantes:  
  

```Bash
### 1

eth0: flags=4098<BROADCAST,MULTICAST>  mtu 1500
        ether aa:aa:aa:aa:00:00  txqueuelen 1000  (Ethernet)
				...

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        ...
```

  

Pour l’adresse `IPV4` suivante, on a les caractéristiques suivantes:

- Adresse du réseau: `192.168.0.0`
- Masque du réseau: `255.255.255.0`
- Plage d’adresses: `192.168.0.0` → `192.168.0.255`

  

On configure ensuite les 4 machines en utilisant la commande `ifconfig` , on note les adresse IP suivantes pour chacune des machines

- `ifconfig eth0 192.168.0.x/24 up` où `x` varie en fonction de la machine que l’on configure.

  

Depuis la machine _imortal_ (`192.168.0.1` ) on ping l’adresse de _syl_ (`192.168.0.2`)

```bash
PING 192.168.0.2 (192.168.0.2) 56(84) bytes of data.
64 bytes from 192.168.0.2: icmp_seq=1 ttl=64 time=0.673 ms
64 bytes from 192.168.0.2: icmp_seq=2 ttl=64 time=0.362 ms
```

On utilise ici la commande `ping` qui utilise le protocole `ICMP` (_man ping)._

  

Depuis les mêmes machines, on effectue la commande `tcpdump -i eth0` depuis la machine _syl_ pour récouter les requête envoyer par la commande `ping` depuis la machine _imortal._ On obtient le résultat suivant dans la console de _syl_ preuve que le ping est bien reçu et retourné.  
  

```bash
[ 1402.117623] device eth0 entered promiscuous mode
tcpdump: verbose output suppressed, use -v or -vv for full protocol 
decode listening on eth0, link-type EN10MB (Ethernet), 
capture size 262144 bytes
11:31:54.736079 IP 192.168.0.1 > 192.168.0.2: ICMP echo request, ..
11:31:54.736112 IP 192.168.0.2 > 192.168.0.1: ICMP echo reply, ...
```

  
On effectue la commande  
`ping 192.168.0.5` , on constate que le ping pars mais ne reçois pas de réponse car la cible est inatteignable. On constate cependant que la machine _syl_ (configuré en écoute) voit tout de même ces pings passé.

  

On essaye maintenant un ping broadcasts `ping 192.168.0.255 -b` .

- On constate que les paquets partent à toutes les machines, mais ces dernières ne renvoie pas de réponses aux paquets envoyé.
    
    ```bash
    11:40:20.209041 IP 192.168.0.1 > 192.168.0.255:
    ICMP echo request, id 746, seq 1, length 64
    ```
    

On modifie ensuite la configuration des machines concernant la gestion des broadcasts avec la commande `sysctl net.ipv4.icmp_echo_ignore=0` et on refait un ping broadcast.

```bash
WARNING: pinging broadcast address
PING 192.168.0.255 (192.168.0.255) 56(84) bytes of data.
64 bytes from 192.168.0.2: icmp_seq=1 ttl=64 time=0.649 ms
64 bytes from 192.168.0.4: icmp_seq=1 ttl=64 time=0.765 ms (DUP!)
64 bytes from 192.168.0.3: icmp_seq=1 ttl=64 time=0.769 ms (DUP!)
```

On a bien une réponse des machines !

  

Après reboot de la machine _immortal_ on effectue la commande `ifconfig` on constate une relise à 0 de la configuration `eth0` .

On configure donc le fichier `/etc/network/interfaces` de la manière suivante:  
  

```bash
auto eh0
iface eth0 inet static
adress 192.168.0.1
netmask 255.255.255.0
```

Après un reboot de la machine, on retombe bien sur la configuration initial qu’on avait fait de la machine (IP: `198.168.0.1`)

  

### Bonus

Configuration de l’IPV6 avec `ifconfig` : `ifconfig eth0 add 2001:db8::/48 up`

Configuration de `/etc/network/interfaces` pour une IPV6:

```bash
auto eth0
iface eth0 inet6 static
        address 2001:db8::
        netmask 48
```

  

Configuration de l’adresse IPV4 avec la commande `ip` :

`ip addr add 192.168.0.1/24 dev eth0`