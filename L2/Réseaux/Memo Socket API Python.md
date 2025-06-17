## Setup
```python
import socket
```

Création d'une socket:
```python
server = socket.socket(socket.AF_INET6, socket.SOCK_STREAM)
# AF_INET6 Famille de socket (ici IPV6)
# SOCK_STREAM Argument générique, échange de données
```

### Serveur
On doit ensuite bind notre socket avec une adresse et un port.
```python
ADDR = "localhost"
PORT = 7777
server.bind((ADDR, PORT))
```

Le serveur pourra ensuite attendre l'arrivé de nouvelle connexion sur le port d'écoute
```python
while True:
	sock, addr = server.listen() # On block sur cette ligne

```
*la méthode listen va blocker le programme jusqu'à ce qu'une nouvelle connexion arrive, lorsque c'est le cas, elle récupère l'adresse et une nouvelle socket de connexion.*

#### Processing request
Une fois qu'un lien est établie entre le serveur et le client, on peut recevoir et traité les données reçu par ce dernier.
```python
while True:
	data = sock.recv(1500) # On bloque sur cette ligne
	print(data.decode('utf-8'))
```

#### Déconnection
Il est conseillé d'implémenté une méthode de déconnexion "correcte" afin de limite les bugs lors de reconnexion.
```python
sock.close() # On ferme la connexion
```

### Client
De la même manière pour un client on initialise une socket et on fait une **connexion** vers une adresse et un port.
```python
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
ADDR = "localhost"
PORT = 7777
client.connect((ADDR, PORT))
```

#### Sending request
Le client peu ensuite envoyer des données à travers la nouvelle socket juste ouverte entre lui et le serveur. 
```python
msg = "hello world"
client.send(msg.encode())
```
> [!important]
> Les données doivent être encodé en binaire: méthode `.encode()`

Le client peut aussi recevoir des données depuis le serveur, il n'y a pas de différence fondamentale entre les deux au niveau de l'échange des données.