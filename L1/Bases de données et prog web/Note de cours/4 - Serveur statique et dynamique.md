---
Complété: true
Type: CM
Materials:
  - https://vanillacademy.com/chapitre/4
Vu en TD: true
---
## Pour construire un site web on peu utilise la recette suivantes:

_Chapitre 3 → 4_

1. **Combien de ressources ?**
    1. Nb d’images, de pages webs, feuilles css, etc…
2. **URL ?** On se demande quel est l’url pour chaque ressources.
    1. On peut construire un tableau répertoriant URL ↔ Ressource
    2. Pour l’instant on préfèrera ajouter un / au début de nos url
3. Server.js
    1. On fait 1 if par ressource (sauf le dernier)
    2. `if (req.url == URL) { res.end(fs.readfile(file)) } else if{—-}`
4. Coder HTML
    
    1. `<a href=”URL”>` URL ONLY !!! (url du tableau)
    
      
    

---

On reprend l’étape 2 du protocole et on se demande:

|   |   |
|---|---|
|**URL**|**Peut changer ?**|
|/image1|X Non|
|/mur|V (En fonction du nombre d’images dans le répertoire)|
|/page_image1|X (Non mais il y en a bcp)|

Cas: Il y a beaucoup de ressources qui ne changent pas (images par exemple)

On ne veut pas Ctrl+C et Ctrl + V tout le code du server.js

_Toute la partie url, image1, image2, image3 etc…_

Web Statique: Quand on a plusieurs ressources qui ne changent pas, qui sont donc des fichiers alors on établit une convention entre leur url et leur fichiers.

  

On établit une convention pour pouvoir compacter

_écrire web statique_

  

Web dynamique: Les ressources changent en fonction des circonstances.

⇒ ==Construction du HTML (texte) à chaque requête.==

```JavaScript
if (req.url == '/mur'){
	let html = "<!DOCTYPE html>";
	html += "<html><body>";
	html += "<h1>Mon Mur</h1>";
	let images = fs.readDirSync('./static/images') // L'enssembles des images
	for (let i = 0; i<images.lenght; i++){
		html += '<img src="/static/images/' + images[i] + '"/>';
	}
		html += "</body></html>";
		res.end(html);
}
```

Soit le code HTML suivant généré par le serveur:

```HTML
<!DOCTYPE html>
<html><body>
<h1>Mon Mur</h1>
<img src="/static/image1.jpg/">
<img src="...">
... On continue pour toutes les images
</body></html>
```

  

Pour construire en web dynamique les pages-images (1, 2, etc…) on établit la convention suivante:

L’URL finis par le numéro de la page. _/pages-images/1_

On peut ainsi construire le code JS en utilisant split() (Voir TD4)

```JavaScript
if (req.url.startsWith('/calendrier/')) {
        let urlParts = req.url.split('/');
```

[[Tableau de bord étudiant/L1/Bases de données et prog web/Note de cours/5 - HTTP]]

## Exercices:

![[Untitled 10.png|Untitled 10.png]]