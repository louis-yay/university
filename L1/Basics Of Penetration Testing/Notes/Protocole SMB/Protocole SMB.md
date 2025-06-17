---
Reviewed: false
---
![[Untitled 3.png|Untitled 3.png]]

Durant un ==scan==, on peu remarquer une ouverture du port 445 TPC réserver pour le [==SMB protocole==](https://fr.wikipedia.org/wiki/Server_Message_Block).  
Généralement, le protocole SMB tourne sur le  
_Application Layer_ ou sur le _Presentation Layer._

Cependant, ce protocole est majoritairement ==utilisé avec d’autres protocoles== comme NetBIOS par dessus TCP/IP. Nous pourrons donc les voir sur des ports différent durant le scan.

  

En utilisant le protocole SMB, une application peu ==acceder aux fichier== d’un serveur, ou à d’autres ressources comme des imprimantes. Il peu également communiquer avec ==n’importe quel programme== prévus pour recevoir le protocole SMB.

![[Untitled 1.png]]

Un _SMB-enabled_ stockage sur le réseau est appelé un ==_share._== ==Ils peuvent être accessible depuis n’importe quel client avec les accès suffisant, permetant ainsi la modification, l’ajout et la supression de fichier. Un niveau de sécurité est donc nécessaire ici.== ==_On fera face à des mots de passe / vérification d’accès._==

Il est cependant possible qu’un administrateur fasse une erreur de sécurité, autorisant les connection _guest_ ou _anonymous_.

  

Pour communiquer avec le protocole smb, il est necessaire d’utiliser un script appelé _smbclient_, s’il n’est pas installé sur la machine, on procède à son installation:

```Bash
sudo apt-get install smbclient
```

Le Smbclient va essayer de connecter l’hôte distant et va vérifier si des authentification sont necessaire, puis procéder à celles-ci si besoin.

![[Untitled 2 2.png|Untitled 2 2.png]]

Ne connaissant aucun password ni authentification, on essayera des connections ‘par défaut’ dans le cas où le client est mal configuré:

- Guest authetification
- Anonymous authentification

```Bash
smbClient -L {target_IP}
[-L|--list=HOST] : Selecting the targeted host for the connection request.
```

![[Untitled 3 2.png|Untitled 3 2.png]]

- Admin$ : Administrateur, peut accéder les différent volumes du disque
- C$ : Accès au disque C
- IPC$ : Protocole de communication interne
- Workshares: Custom share.

  

On essaye donc de se connecter à travers les différents portails:

```Bash
smbclient \\\\{target_ip}\\ADMIN$
smbclient \\\\{target_ip}\\C$
smbclient \\\\{target_ip}\\WorkShares
```

Il est probable que certain accès soient refusé, par mesure de sécurité. On essaye donc le suivant etc.. Jusqu’à potentiellement tombé sur un accès ouvert.

![[Untitled 4.png]]

![[Untitled 5.png]]

Une fois connecté, on peu utiliser la commande help pour voir ce qu’il est possible de faire. On peu ainsi naviguer au sein du disque à la recherche du drapeau !