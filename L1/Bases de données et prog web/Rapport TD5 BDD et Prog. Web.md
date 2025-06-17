## Procédé

J’ai construit mon site web en suivant les conseils du cours présent sur le site web ainsi que les conseils donnés lors des TD.

  

**CLEAN CODE**  
  
Ainsi, le projet à été mené afin d’être le plus cohérent avec la notion de  
_Clean_ _Code_ (Doc.[https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29](https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29))

  

**RESSOURCES**  
  
Aussi, le serveur est construit en grande majorité de manière  
dynamique afin de répondre aux exigence technique des différents TD. Les seules ressources encore en statique sont, la page d’index, la page d’erreur 404 ainsi que la feuille de style et les différentes images.  
On pourrait passer les 2 dernières pages HTML en dynamique et intégrer le CSS au sein même des pages dynamiques, mais ça ne serait pas très pertinent au vu que ces dernières ne sont pas ou peu amené à changer.  

  

**ARCHITECTURE**  
  
En ce qui concerne  
l’architecture du site, j’ai opté pour une séparation des images par rapport au reste des ressources statiques. Ainsi on peu observer le dossier `images` et sous sous dossier `small`, qui contiennent respectivement les images de taille normal et leurs équivalent en petit.

  

**GESTION DES REQUÊTES HTTP  
  
  
**Comme indiqué dans le dernier TD, le serveur gère les requête `POST`.  
Il en récupère les données dans le corp de celles-ci.  
Dans le cadre de l’ajout de commentaires sous les images, le serveur vas accéder au commentaire et l’ajouter à la liste des commentaires, déjà déclaré en tant que variable globale du serveur.  
Les autres méthodes tombe dans la suite de  
`if else`, vérifiant si la demande est pour une ressource statique, s’il s’agit d’une image ou d’une page dynamique.  
Si aucune page n’est trouvé, on renvoie la page 404 et le code d’erreur correspondant.  

  

**PORT**  
Enfin, le serveur tourne en local sur le port 8080.  
  
_Adresse du site en local:_ `[http://localhost:8080](http://localhost:8080/static/index.html)`

  

### Tableau des ressources

|   |   |   |   |
|---|---|---|---|
|Ressource|Dynamique / Statique|URL|Description|
|index.html|Statique|http://localhost:8080/static/index.html|Page d’index|
|style.css|Statique|http://—-/static/style.css|Feuille de style CSS|
|images.html|Dynamique|http://—-/images.html|Le mur d’images|
|pages-image/X|Dynamique|http://—-/pages-images/X|Page images pour chacune des images|
|imageX.jpg|Statique|http:/—-/static/images/imageX.jpg|Image X|
|imageX_small.jpg|Statique|http://—-/static/images/small/imageX_small.jpg|Petite version de l’image X|
|logo.png|Statique|http://—-/static/logo.png|Logo du site, sur la page index|
|404.html|Statique|http://—-/static/404.html|Page d’erreur 404|

  

_Louis DENYS_