### Introduction

En analysant les retours des commandes `ifconfig` , `/sbin/route -n` ainsi que le fichier `resolv.conf` on retient:

- L’adresse IPV4: `10.0.2.15`
    - Son masque: `255.255.255.0`
    - L’adresse de gateway: `10.0.2.2`
    - Serveur DNS local: `10.0.2.3`

  

Avec l’outil wireshark on constate les détails des différentes trames (exemples ci dessous)

![[Capture_dcran_du_2024-09-25_11-13-36.png]]

  

### Analyse des trames wireshark

- On procède en envoyant la requête ARP à l’adresse de broadcast afin que l’ensemble des machines du réseau reçoivent la requête. De cette manière, on demande à l’ensemble des machines si une adresse IP (ici `10.0.2.3`) leur appartient. Et si c’est le cas, de renvoyer leurs adresses MAC à l’adresse expéditrice (ici `10.0.2.15`).
- Le protocole utilisé pour les échangent DNS est le protocole UDP (_User Daragram Protocol)_
    - En découpant la réponse du serveur DNS, on y trouve l’adresse IP la machine [www.google.com](http://www.google.com): `172.217.19.132`
- On cherche à savoir à quel machine appartient l’adresse `10.0.2.2` car il s’agit de l’adresse de gateway, adresse nécessaire pour toute communication entre 2 machine qui ne sont pas dans le même réseau.
    - On a l’adresse ethernet `52:55:0a:00:02:02` qui correspond bien à l’adresse qu’avait renvoyé le serveur DNS suite à la demande pour le gateway (`10.0.2.2`)
- Dans les requêtes ICMP on a les types:
    - `type: 8` pour le request _(echo (ping) request)_
    - `type: 0` pour le reply _(echo (ping) reply)_

  

### Une page Web: je suis perdu!

- Le port source (client) est `37090` et le port de destination est `80` (serveur). Le flag `SYN` correspond à une demande de synchronisation de l’utilisateur vers le serveur.
- Les trames qui correspondent aux échanges HTTP sont:
    - La trame 10 pour la requête `GET`
    - La trame 12 pour la réponse `200 OK`
- Les champs suivant correspondent:
    - `User-Agent` - information sur le client et son OS _(système d’exploitation)_
    - `Host` - URL du serveur durant l’échange
    - `Connection` - Information sur l’état que doit avoir la connexion une fois la requête terminé.
        - `close` pour fermer la connexion
        - `keep-alive` pour la garder ouverte
- On constate les champs suivants:
    - `Server: Apache` → Le logiciel serveur est Apache
    - `Content-Lengh: 204` → Longueur du contenue de la réponse est de 204 caractères.
    - `Content-Type: text/html` → Le serveur nous à renvoyé une page html

On reconstruit facilement la conversation en utilisant l’outil de suivit dans wireshark.

_TP fait en collaboration avec EL-MOUTAKI Fatine_