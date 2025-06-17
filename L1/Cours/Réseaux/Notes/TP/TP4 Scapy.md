### Démarrage du réseau virtuel

Depuis mon ordinateur distant je me connecte à un poste du cremi via SSH et j’effectue la commande suivante:  
  

```C
$ /net/ens/qemunet/qemunet.sh -b -d tmux -s /net/ens/qemunet/demo/gw.tgz
$ tmux a 
```

- On met ensuite en écoute la machine immortal avec `tcpdump -i eth0`
- Puis on observe les services disponibles sur la machine opeth avec `netstat -tupl`

```Bash
root@opeth:~# netstat -tupl
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address         Foreign Address  
tcp        0      0 0.0.0.0:ssh           0.0.0.0:*        
tcp        0      0 0.0.0.0:telnet        0.0.0.0:*        
tcp        0      0 0.0.0.0:echo          0.0.0.0:*        
tcp        0      0 0.0.0.0:daytime       0.0.0.0:*        
tcp6       0      0 [::]:http             [::]:*        
tcp6       0      0 [::]:ssh              [::]:*         
udp        0      0 0.0.0.0:echo          0.0.0.0:*
udp        0      0 0.0.0.0:daytime       0.0.0.0:*
```

  

### Prise en main de scapy

On test scapy3 sur la machine:

```Bash
>>> x = IP()
>>> x.show()
###[ IP ]###
  version= 4
  ihl= None
  tos= 0x0
  len= None
  id= 1
  flags=
  frag= 0
  ttl= 64
  proto= hopopt
  chksum= None
  src= 127.0.0.1
  dst= 127.0.0.1
  \options\
```

On procède de la même manière mais en écrivant le code dans un fichier `scipt.py` . On obtient les mêmes résultats.

```Bash
#!/ usr /bin /env python3
import sys
from scapy . all import *
x = IP ()
x. show ()
```

  

### Ping

On construit notre paquet IP et on l’encapsule dans un paquet ICMP

- `ping = IP(dst ='192.168.0.2')/ ICMP(type ='echo-request')`

A partir de la on peu envoyer le paquet et stocké la réponse dans la variable `pong` :

- `pong = sr1( ping )`

  

### ARP

Le protocole ARP demande à l’ensemble de machine à travers le broadcast si une certaine adresse IP leurs correspond _Who has_.  
Si la machine en question à la bonne adresse IP, cette dernière renvoie son adresse mac, utile pour les communication Ethernet de bas niveau.  

On écrit le script suivant:

```Python
#!/usr/bin/env python3
import sys
from scapy.all import *

etrame = Ether(dst='FF:FF:FF:FF:FF:FF')
ARP = ARP(pdst='147.210.0.1')
tram = etram/ARP
req_res = srp1(trame, timeout=1)

if req_res:
        print(f"Response from {req_res.psrc}")
        print("MAC : {req_res.hwsrc}")
else:
        print("No response recieved.")
```

  

### Services UDP : Daytime et Echo

En faisant pas à pas le script `daytime.py` on récupère la date en brut:

- `b'Wed Oct 2 17:43:39 2024\r\n'`

Pour le service du port Echo, le padding correspond à la mémoire ajouté à la fin du message afin que ce dernier fasse une taille normalisé.

  

### Syn Scan

Soit la fonction suivante qui permet de scanner les ports de 1 à 100 et d’afficher les listes des ports avec leurs états de réponse à la requête TCP pour`SYN`.

```Python
from sys import *
from scapy.all import *

def scan(addr):
  ipPqt = IP(dst=addr)
  ports = []
  for i in range(1, 100):
  res = sr1(ipPqt/TCP(sport = 12345, dport = i, flags='S'))
    ports.append(res.payload.flags.S)
  return ports

data = scan('192.168.0.2')
for i in range(100):
  print(f"Port {i} -> {data[i]}")
```

  

### Traceroute

```Python
#!/usr/bin/env python3
import sys
from scapy.all import *

def traceroute(addr, hop_max=32):
  for i in range(1, hop_max+1):
    ipPqt = IP(src='147.210.0.2', dst=addr, ttl=i) / ICMP()
    res = sr1(ipPqt, timeout=1) \#On set un timeout d'attente de la réponse.
		
    if res == None:
      print(f"No response from Hop N°{i}")
    else:
      if(res[IP].src == addr):
	      print(f"{addr} reached at Hop N°{i}")
	      break \#On quitte la boucle car la destination est atteinte.
	    else:
	      print(f"Hop N°{i} from {res[IP].src}")
	return
```

  

### Connexion TCP

```Python
#!/usr/bin/env python3
import sys
from scapy.all import *

def TCP_connexion(addr, port=7):
  ipPqt = IP(dst=addr)
  tcpPqt =  TCP(sport=12345, dport=7, flags = "S")
  res = sr1(ipPqt/tcpPqt)
  if(res.payload.flags.SA):
    tcpPqt =  TCP(sport=12345, dport=7, flags = "A")
    res = sr1(ipPqt/tcpPqt)
    print("connection established")
    return
	
TCP_connexion('192.168.0.3')
```