---
Complété: true
Type: CM
Materials:
  - https://vanillacademy.com/chapitre/2
Vu en TD: true
---
### Quel est la différence entre une page web et une page html ?

- Une page web est affiché par un navigateur
- Une page web est constitué de une ou plusieurs ressources fournis par un ou plusieurs serveur web.
- Toute page web, doit, absolument, avoir une ressource qui est un document HTML.
- Le html précises les autres ressources necessaire à l’afichage de la page.

  

![[Untitled 12.png|Untitled 12.png]]

La ressource html est la première ressource que le navigateur va demander puis charger. Ainsi l’url d’un site web va renvoyer la page html (première ressource), et comme 1 URL = 1 ressource, l’url du site renvoie à la ressource HTML (puis vers d’autres ressources).

Le navigateur lis le html et regarde s’il demande d’autres url, et ainsi, il fera les requêtes necessaires. Le HTML est donc la ressource principale, celle qui connait les autres ressources.

  

### Le HTML

- Liste les URL des autres ressources necessaire à l’affichage
- Explique comment l’affichage doit se faire (painting)
- C’est du texte !
- Il précise la construction de la page sous la forme d’un arbre (texte structuré) et de cet arbre, je construit la page.

![[Untitled 1 7.png|Untitled 1 7.png]]

### A. Du texte à l’arbre:

1. Prologue: <!DOCTYPE html>
2. Les balises (marqueurs de structuration) <body><\body>. Quand l’algo voit une balise ouverte, il crée un noeud dans l’arbre, il descend, puis remonte quand il voit une balise fermé.
3. Nettoyage du texte libre (espace, retour chariot etc.. supprimé !)

_Exemple:_

```HTML
<html>
	<body>
		<img src="URL">
			Hello
			
			Comment     vas  tu ?
		<img src="URL">
	<\body>
<\html>
```

![[Untitled 2 7.png|Untitled 2 7.png]]

### B. De l’arbre à l’image:

- 1 noeud = 1 boite carré (ou rectangulaire) (a partir de body)
- Règle 1: Lorsqu’on a un noeud parent, on aura une boite (fils) dans une autre boite (parent).
- Règle 2: (Frère et soeur), On suppose un même parent pour 2 noeuds, on aura un affichage côte à côte ou l’un au dessus de l’autre en fonction des balises.

![[Untitled 3 6.png|Untitled 3 6.png]]

![[Untitled 4 4.png|Untitled 4 4.png]]

![[Untitled 5 3.png|Untitled 5 3.png]]

### Les balises de base sont utilisées pour réaliser la structure d'une page web :

- `<html>` : La balise `<html>` est la balise racine de l'arbre des balises dans un code HTML.
- `<head>` : La balise `<head>` doit être sous la balise `<html>`. Elle contient les informations générales sur la page web ainsi que les liens vers les ressources la composant.
- `<title>` : La balise `<title>` définit le titre de la page web.
- `<body>` : La balise `<body>` doit être sous la balise `<html>`. Elle définit le corps de la page web (ce qui est affiché par le navigateur).

  

### Les balises de texte suivantes sont utilisées pour définir le contenu textuel d'une page web :

- `<h1> ... <h6>` : Les balises `<h1> ... <h6>` définissent des niveaux de titres de texte (`<h1>` étant le niveau le plus haut).
- `<p>` : La balise `<p>` définit un paragraphe de texte.
- `<br>` : La balise `<br>` ajoute un saut de ligne.
- `<hr>` : La balise `<hr>` ajoute une ligne grise pour délimiter deux sections dans la page.
- `<ul>` : La balise `<ul>` définit une liste non énumérée (unordered list).
- `<ol>` : La balise `<ol>` définit une liste énumérée (ordered list).
- `<li>` : La balise `<li>` définit un élément dans une liste (item list), que la liste soit énumérée ou non énumérée.

  

### Les balises de média sont utilisées pour intégrer des images, des films ou des pistes sonores :

- `<img (src width height)>` : La balise `<img>` affiche une image. L'URL de l'image doit être donnée dans l'attribut **src** de la balise. Les attributs **width** et **height** servent à modifier la taille de l'affichage.
- `<audio (controls)>` : La balise `<audio>` est utilisée pour écouter un son. L'attribut **controls** est optionnel. Lorsqu'il est utilisé le navigateur affiche les boutons de contrôle pour écouter le son (play, stop, etc.). La balise `<audio>` doit contenir une balise `<source (src)>` pour cibler le contenu sonore. La balise `<source>` permet de préciser l'URL du contenu sonore dans l'attribut **src**.
- `<video (width height controls)>` : La balise `<video>` est utilisée pour afficher une vidéo. Cette balise doit contenir une balise `<source (src)>` pour cibler le contenu vidéo.

  

### Les balises de structuration permettent de modifier la structure de l'arbre HTML et ainsi de faciliter la mise en forme de la page web:

- `<div>` : La balise `<div>` définit une partie de la page web. Cette balise est souvent employée pour structurer la page web en différentes parties.
- `<span>` : La balise `<span>` définit une sous partie d'une suite d'éléments affichés en ligne. Cette balise est souvent employée pour structurer les lignes de la page web.

  

### Enfin les balises de lien permettent de relier le code HTML avec d'autre ressources qui composent la page web :

- `<a (href)>` : La balise `<a>` définit un lien vers une page web dont l'URL est donné dans l'attribut **href**.
- `<style>` : La balise `<style>` définit un style CSS à appliquer sur la page web.
- `<link (href rel)>` : La balise `<link>` référence une ressource dont l'URL est donnée dans l'attribut **href** et dont la nature de la relation de cette ressource avec la page web est précisée par l'attribut **rel**. Nous expliquons comment cette balise est utilisée pour référencer des styles CSS dans le [Chapitre 3](https://vanillacademy.com/chapitre/3) .
- `<script (src)>` : La balise `<script>` définit un script (JavaScript). Il est aussi possible de référencer un script externe en précisant son URL dans l'attribut **src**.

  

[[Tableau de bord étudiant/L1/Bases de données et prog web/Note de cours/3 - CSS]]

## Exercices et cours complet:

> [!info] Vanilla Academy - Chapitre 2  
> La vocation d'une page web est d'être affichée dans un navigateur web afin que les utilisateurs puissent la visualiser et interagir avec elle.  
> [https://vanillacademy.com/chapitre/2](https://vanillacademy.com/chapitre/2)  

![[Untitled 6 3.png|Untitled 6 3.png]]

### Exercice 1:

![[Untitled 7 2.png|Untitled 7 2.png]]

![[Untitled 8 2.png|Untitled 8 2.png]]

![[Untitled 9 2.png|Untitled 9 2.png]]

### Exercice 2:

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>Mes Images</title>
    </head>
    <body>
        <h1>Mes images</h1>
        <p>Voici mes images</p>
        <div>
            <img src='logo.jpg'>
        </div>
        <hr/>
        <div>
            <a href='images'>
                <img src='image1_small.jpg'>
                <img src='image2_small.jpg'>
                <img src='image3_small.jpg'>
            </a>
        </div>
        <img src='image1'/>
        <img src='image2'/>
        <img src='image3'/>
        <img src='image4'/>
        <img src='image5'/>
    </body>
</html>
```

![[Untitled 10 2.png|Untitled 10 2.png]]

![[Untitled 11 2.png|Untitled 11 2.png]]

![[Untitled 12 2.png|Untitled 12 2.png]]

Si on retire les div, au sien du DOM, le logo sera enfant du body, et la balise a deviendra aussi enfant de body.  
Sur l’affichage le logo ne changera pas de position, cependant toutes les images (les 8) seront toutes aligné due à la balise <a> qui est inline.  

![[Untitled 13.png]]

![[Untitled 14.png]]