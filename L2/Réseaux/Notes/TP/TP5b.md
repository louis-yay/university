Trouver le numéro de port de echo:
```bash
grep echo etc/services
```
- Le numéro de port de echo est donc le `7` pour TCP et UDP

### Encodage
Pour transmettre des données en utilisant une socket, nous devons transtypé les chaines de caractères en tableau d'octer:
```python
s = "hého"  
c = s.encode("utf-8")  
# Et inversement, quand on a lu un tableau d’octets depuis le réseau et que l’on veut  
# l’afficher :  
c = b"h\xc3\xa9ho"  
s = c.decode("utf-8")
```

### Plus petit serveur TCP

```python
import socket
import sys
s = socket.socket(socket.AF_INET6, socket.SOCK_STREAM)
s.bind(("", 7777))
s.listen(1)

running = True

while running:

	# On accepte les connexion entrante
	sock, addr = s.accept() #On reçoit une nouvelle socket et une adresse, connexion du client
	connected = True
	while connected:
		data = sock.recv(1500) #De cette socket on reçoit des données
		
		if(len(data) == 0):
			connected = False
			print(data.decode("utf-8"))
```
On peut ensuite établir une connexion TCP avec l'outil `netstat`:
- `nc localhost 7777`
- *Il est également possible de voir que le port 7777 est ouvert avec `netstat -tuap`*
