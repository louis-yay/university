---
Complété: true
Type: CM
Materials:
  - https://vanillacademy.com/chapitre/5
Vu en TD: true
---
HTTP est le protocole du web (communication serveur ↔ client)

- Client / serveur ⇒ Message du client vers me serveur (navigateur ⇒ serveur)
- Requête / Réponse ⇒ Le client pose une requête et le serveur y répond (ex: demande de ressources, réponse: ressource)

![[Untitled 18.png|Untitled 18.png]]

  

## Requête

Commande → 1 ligne `retour chariot`

Les entêtes → (X lignes) `Double retour chariot`

Le corps →

## Réponse

Commande → 1 ligne `retour chariot`

Les entêtes → (X lignes) `Double retour chariot`

Le corps →

  

Exemples:

GET url_complete HTTP/1.1

Entêtes: Accept: text/html (on accepte du format texte ou sous format html)

HTTP/1.1 OK 200 (status code)

ZIP

Cookie

(corp)

<html> …

## Zoom sur la requête (commande)

Les intentions HTTP:

GET → Donne moi la ressource dont l’url est ———

DEL → Supprime ———

PUT → Dépose, pose

Il en existe d’autres, cependant depuis les années 2000, les commandes DEL et PUT ne sont plus (ou presque) accepter par les serveurs.

- ==On== ==peu utiliser l’outil postman pour envoyer des requêtes==

  

## GET VS POST

## GET

L’intention est d’obtenir une ressource

Ici on a pas de corp (on cherche juste à récupérer une info)

==Si on fait un get avec des datas, il faut les mètre dans l’url. (limité en taille et en encodement)==

![[Untitled 1 11.png|Untitled 1 11.png]]

### POST

L’intention c’est de modifier en ajoutant de l’info à une ressource

Ici le corp n’est pas vide (la plus part du temps)

![[Untitled 2 11.png|Untitled 2 11.png]]

## Comment demander au navigateur d’envoyer des requêtes ?

- Barre de navigation → GET
- Lien des ressources dans le doc HTML (image, css, …) → GET
- Lien hypertext
    
	 ```js
    if (x.StartWith('http') {
    	url = x; }
    else if (x.startWith('/'){
    	url = document.url.protocole + port + x } // Document = Chemin dans lequel on se trouve
    else {
    	doc.url.suppAfterLastSlash + x
    }
    ```
    
      
    

## La balise FORM

```JavaScript
<form action='url' method='GET/POST'>
	<input name='tel' type='number' type='submit'>
<form>
```

  

  

### Comment sont encodés les données d’un formulaire dans la req HTTP ?

- En utilisant la méthode GET, on passe les informations du formulaire dans l’url.
- On a donc une url de la forme: `http—?nom=val&nom2=val2`
- Et en utilisant la méthode POST on lit le corp de la requête pour récupérer les informations.

```JavaScript
else if (req.method === "POST" && req.url === "/description-image") {
        let donnees;;
        req.on("data", (dataChunk) => {
            donnees += dataChunk.toString();
        });
        req.on("end", () => {
            const paramValeur = donnees.split("&");
            const imageNumber = paramValeur[0].split("=")[1];
            const description = paramValeur[1].split("=")[1];
            descriptions[imageNumber] = description;
            res.statusCode = 200;
            res.end(descriptions.toString());
        });
```

  

[[Tableau de bord étudiant/L1/Bases de données et prog web/Note de cours/6 - Modèle Relationnel]]