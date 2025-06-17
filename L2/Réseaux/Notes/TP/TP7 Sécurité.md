## Certificat X509
- le certificat de l’autorité de certification (ou CA) : *ca.pem* 
	- Certificat publique de l'autorité de certification.
	- Présent sur les machines clientes, il permet de vérifier l'authenticité de la signature du certificat du serveur 
- la clé privé de l’autorité de certification : *ca.key*
	- Clé privé de l'autorité, cette dernière à servit à signer le certificat de l'autorité de certification ainsi que le certificat du serveur
	- Cette clé reste caché au sein de l'autorité et nous ne l'utiliserons pas ici.
- le certificat du serveur : *server.pem*
	- Certificat d'authentification du serveur signé par l’autorité de certification et envoyé au client
- la clé privé du serveur : *server.key*
	- La clé privé du serveur, cette clé à également signé le certificat du serveur
	- Cette clé reste privé, côté serveur.

### Inspection des certificats
```c
certtool --certificate-info --infile ca.perm
certtool --certificate-info --infile server.perm
```

On synthétise les informations contenue dans le certificat du serveur
1. Version du certificat: 3
	- Version actuelle de la norme X.509
2. Numéro de série du certificat: 1eb74c7337400d26d85ec1bc592a468308bb9700
	- Numéro unique d'identification du certificat, émit par l'autorité de certification (CA)
3. Nom du propriétaire du certificat: 127.0.0.1
	- Nom du serveur qui heberge le certificat (ici son adresse ip)
4. Nom de l'autorité de certification: CA
	- Nom de l'autorité de certification qui à signé le certificat
5. Date d'expiration du certificat: 31 Aout 2025
	- Date d'expiration du certificat, le certificat n'est plus utilisable au delà de cette date
6. Key Id: 7f01fab60f74003147ea9b23ff4d341f0e1b7d4b
	- Numéro d'identification de la clé de certification
7. Signature: (...)
	- Signature de l'autorité de certification (CA)
8. Fingerprint: 
	1. sha1: 4bf706d9d65c8a57a449d20c261e00ef6af8a57d
	2. sha256: 3d492034098eb162696663ac76a48b006c6437537c718a9332d6e87e2896e353
	- Checksum du certificat en fonction de différents algo (sha1 et sha256)
9. Valeur de la clé publique: JBraOMQgir19WvT+aLykTIqrzKDyOCCHsW1wygs56uU=
	- Clé publique du serveur 

#### Inspection du certificat www.perdu.com
```c
certtool --certificate-info --infile perdu
```

On constate les informations suivantes sur son certificat
```c
X.509 Certificate Information:
	Version: 3
	Serial Number (hex): 342b4db016dc3ae31395769c23a6436d
	Issuer: CN=WE1,O=Google Trust Services,C=US
	Validity:
		Not Before: Wed Oct 23 10:27:15 UTC 2024
		Not After: Tue Jan 21 10:27:14 UTC 2025
	Subject: CN=perdu.com
	Subject Public Key Algorithm: EC/ECDSA
	Algorithm Security Level: High (256 bits)

[...]
Fingerprint:
		sha1:30b522a8f1dbb6824bd0427fc98244147fa47ad9
		sha256:3c0223bc7801f8d8862613b7f9366b1f9a09ecb6c189f468872e0e2e4fe2798e
	Public Key ID:
		sha1:32f7396feeeee2c7f1edaec63d76f7dddae63c2e
		sha256:849be42f3bc57b3b4056d4e1ffcdb2e9200ebe79b8e8de24e53fea89f448ac47

```
On constate que l'autorité de certification de perdu.com est Google Trust Services. On reconnait également le nom de domaine, on a accès au finger print etc...
- Note: la version de X509 est bien la même.


### Test des certificats
On utilise GNS TLS pour simuler une connexion http entre deux machines:
```c
$ gnutls-serv --http --x509keyfile=server.key --x509certfile=server.pem --port=1234
HTTP Server listening on IPv4 0.0.0.0 port 1234...done
HTTP Server listening on IPv6 :: port 1234...done

```

On se connecte ensuite avec un autre terminal pour simuler un client:
```c
$ gnutls-cli --x509cafile ca.pem -p 1234 127.0.0.1
Processed 1 CA certificate(s).
Resolving '127.0.0.1:1234'...
Connecting to '127.0.0.1:1234'...
- Successfully sent 0 certificate(s) to server.
- Server has requested a certificate.
- Certificate type: X.509
- Got a certificate list of 1 certificates.
[...]
- Status: The certificate is trusted. 
- Description: (TLS1.3-X.509)-(ECDHE-SECP256R1)-(RSA-PSS-RSAE-SHA256)-(AES-256-GCM)
- Session ID: 53:7D:EE:03:E3:00:6C:DC:A7:96:25:FA:32:AE:85:70:4D:8F:2C:19:2E:C5:DF:09:B6:6E:B1:34:C5:97:6F:99
- Options:
- Handshake was completed

- Handshake was completed

```

On peut ensuite faire un `GET / HTTP/1.0` pour récupérer une page `html`:

![[Tableau de bord étudiant/L2/Réseaux/Notes/TP/GNU_TLS_infopage.png]]

On essaie ensuite avec notre navigateur web:
![[Tableau de bord étudiant/L2/Réseaux/Notes/TP/test_localhost_naivgateur.png]]
On constate un avertissement, la connexion n'est pas sécurisé.
- Cet avertissement est due à l'origine du certificat, ce dernier étant auto-généré, il ne correspond pas aux attentes de notre navigateur web, qui s'attend à un veritable certificat vérifié par une autorité d'authentification
- Accepter un certificat non reconnue dirige vers un server qui n'a pas été vérifié par une autorité de confiance, le site internet en l’occurrence peut-être frauduleux et représenter une menace pour le client.

### Programmation Socket SSL (Py)
##### Client
```python
#!/usr/bin/python3
import socket
import ssl
  

HOST = "127.0.0.1"
PORT = 7777
BUFSIZE = 1024

# SSL
context = ssl.create_default_context(ssl.Purpose.SERVER_AUTH)
context.load_verify_locations('ca.pem')

# Generate SSL socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sslsock = context.wrap_socket(s, server_side=False, server_hostname=HOST) # bind sock to ssl context

# Main programm
sslsock.connect((HOST, PORT))
msg = b"Hello World!"
sslsock.sendall(msg)
answer = sslsock.recv(BUFSIZE)
print(answer.decode())
sslsock.close()
```

##### Server
```python
#!/usr/bin/python3
import socket
import ssl
  
HOST = '127.0.0.1'
PORT = 7777
BUFSIZE = 1024

# SSL
context = ssl.create_default_context(ssl.Purpose.CLIENT_AUTH)
context.load_cert_chain('server.pem', 'server.key')

# echo server
def echo(sc):
	while True:
		data = sc.recv(BUFSIZE)
			if data == b'' or data == b'\n':
				break
		print(data.decode())
		sc.sendall(data)

  
  

# main program
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
sslsock = context.wrap_socket(s, server_side=True) # bind sock to ssl context
sslsock.bind((HOST, PORT))
sslsock.listen()

while True:
	sc, addr = sslsock.accept()
	echo(sc)
	sc.close()

s.close()
```

Dans ces deux cas, on va créer un contexte d'execution ssl en s’appuyant sur nos certificats, on relie ensuite nos socket aux deux contexte et on peut ensuite échanger de manière sécurisé comme pour les sockets classiques.