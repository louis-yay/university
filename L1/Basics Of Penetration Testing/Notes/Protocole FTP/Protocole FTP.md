---
Reviewed: false
---
[Le protocole FTP](https://youtu.be/tOj8MSEIbfA?feature=shared) (File Transfer Protocol) est un protocole d’échange de données entre un client (utilisateur) et un serveur web connecté sur l’internet. C’est un protocole qui ne sécurise pas les échanges de données, celles-ci sont transmises grâce au TCP/IP et ne sont pas crypté.

![[Untitled 6.png|Untitled 6.png]]

_Un outil bien connu utilisant ce protocole est_ [_Filezilla_](https://filezilla-project.org)

  

Un port exécutant un service actif est un espace réservé pour l'adresse IP de la cible afin de recevoir des demandes et d'envoyer des résultats. Cela signifie que le client doit impérativement avoir plusieurs ports actif s’il souhaite pouvoir effectuer différente tache simultanément. Voici un exemple:

![[Untitled 1 2.png|Untitled 1 2.png]]

  

Aussi, les données n’étant pas crypté, ainsi, n’importe qui interceptant la communication peu lire les données en clair. Ce type d’attaque est appelé: attaque de ==l’Homme du milieu.==

![[Untitled 2 3.png|Untitled 2 3.png]]

![[Untitled 3 3.png|Untitled 3 3.png]]

FTP non sécurisé

![[Untitled 4 2.png|Untitled 4 2.png]]

Utilisation de FTPS et [SSH](https://fr.wikipedia.org/wiki/Secure_Shell).

  

Pour installer / vérifier l’installation du protocole FTP:

```Bash
sudo apt install ftp -y
[...]
ftp is already the newest version (20210827-4build1).
```

Ensuite de ça on peu utiliser l’attribut ==-h== pour voir de quoi le protocole est capable:

```Bash
ftp -h
```

  

On peu ensuite se connecter au serveur avec la commande suivant:

```Bash
ftp 10.129.1.14
Connected to 10.129.1.14.
220 (vsFTPd 3.0.3)
Name (10.129.1.14:kira):
```

Ils nous ai alors proposé d’entrée un nom.  
Une mauvaise configuration du protocole ftp autoriser les utilisateur anonyme à se connecter en tapant le nom  
_anonymous_

```Bash
Connected to 10.129.1.14.
220 (vsFTPd 3.0.3)
Name (10.129.1.14:kira): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
```

  

Une fois connecté, on peu utiliser la commande help / -h / —help pour avoir plus de détails sur ce qu’il est possible de faire.  
Pour plus d’info sur une commande en particulier on peu faire:  

```Bash
man {CommandName}
```

  

Une fois dans le serveur, on peu effectuer la commande bien connu, ls, pour acceder à la liste des dossiers et fichiers.

```Bash
229 Entering Extended Passive Mode (|||29213|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
```

Le protocole ftp nous informe de l’avancement de la commande que nous venons de passer à l’hôte.

  
On repère donc le fichier flag.txt, que l’on peu télécharger:  

```Bash
get flag.txt
local : flag. txt remote: flag. txt
200 PORT command successful. Consider us ing PASV.
150 Opening BINARY mode data connection for flag. txt (32 bytes).
226 Transfer complete.
32 bytes received in secs (33.7838 kB/s)
```

Le fichier à bien été récupéré de la machine !