---
Complété: true
Type: CM
Materials:
  - https://www.notion.sovanillacademy.com/chapitre/3
Vu en TD: true
---
  

## Qu’est ce que CSS ?

Lors du passage de l’arbre construit depuis le fichier HTML vers l’image, on se pose la question du visuel, comment positionner et afficher ?

- Quels sont les propriété graphique (Couleurs, Fonts, Espaces, etc…) ?
- ==Propriété graphique = nom de la propriété et sa valeur==
    - Environs ==200 propriétés graphiques== différentes ! (+/-)
    - Cependant comme le navigateur se demande pour chaque propriété, que faire ? Il y a donc une valeur par défaut pour chacune de ses propriété graphiques, qui peut dépendre de la balise

![[Untitled 11.png|Untitled 11.png]]

CSS sert donc à ==changer les valeurs des propriété graphique.== Celles-ci pré-existe au CSS !!

## Où trouver les nouvelles valeurs des prop graphique ?

1. Dans le html avec la balise `<style>` qui permet de taper du code CSS
2. On utilise la balise ==`<link ref=”stylesheet” href=”style.url”>`== qui va dire au navigateur de ==charger une feuille de style==

On cherche à lier le HTML avec les nouvelles valeurs !

  

## Comment donner les nouvelles valeurs ?

```CSS
nom_de_la_prop : la_nouvelle_valeur;
color: "red"; /*Il y a souvent plusieurs type de valeurs possible pour une même propriété*/
```

  

## Sur quel(s) noeud(s) affecte-t-on ces nouvelles valeurs ?

![[Untitled 1 6.png|Untitled 1 6.png]]

### La règle CSS c’est:

- La ou les nouvelles valeurs
- Une expression qui permet de cibler un ou plusieurs noeud

  

## L’expression qui permet de cibler un ou plusieurs noeuds:

1. En précisant le nom de la balise (toute les balises qui ont ce nom sont ciblé par le changement de la valeur de la propriété graphique)
    
    ```CSS
    h1 { /*Expression, toutes les balises h1 seront impacté*/
    	color: "red";
    }
    ```
    
2. En précisant une class CSS.  
    En HTML, toutes les balises peuvent avoir une propriété nommée  
    **class****.** La valeur est donnée par le développeur.  
    Ainsi cette classe peut être utilisé comme expression d’une règle CSS  
    
    ```HTML
    <h1 class="important">Titre 1</h1>
    <p class = "important">Paragraphe</p>
    ```
    
    ```CSS
    .important {
    	color: "red";
    }
    ```
    
3. En donnant l’id du noeud HTML. En HTML, on peut donner des ID à un noeud _<h1 id=”titre1”>_ ==UN ID DOIT ETRE UNIQUE !==
    
    ```CSS
    \#titre1 {
    	color: "red";
    }
    ```
    

  

**Expressions**

- De base:
    - Balise H1{- - -}
    - Class .important{- - -}
    - id \#monId{- - -}
- Composition:
    - ex: H1.important{- - -} Va toucher toute les balises h1 qui ont la classe important
    - ex: H1>H2 va toucher toutes les balises H2 qui ont pour parent directe une balise H1
    - etc… Il y a 50 façon de composer les balises

## Conflicts

```CSS
H1{
	color:"red";
}

.important{
	color:"yellow";
}
```

```HTML
<H1 class = "important"></H1>
```

Le navigateur vas, lors du passage de HTML vers l’arbre, récupérer les 2 règles (CSS)

- Le navigateur attend les règles pour commencer le painting

![[Untitled 2 6.png|Untitled 2 6.png]]

On parcours l’arbre en largeur et pour chaque nœud on se demande s’il y a une ou des règles qui cible le nœud ?  
Il cherche dans le CSS les valeurs puis  
==s’il y a matching alors il applique au nœud==.  
Ensuite, il passe au nœud enfant et il commence par  
==COPIER les règles du NOEUD parent==. Puis cherche les règles et écrase les valeurs par de nouvelles si necessaire (d’où le cascading du CSS).

### Comment gérer les conflits ?

S’il y a conflit entre deux expressions, on établis des règles de priorité de la façon suivante:

- On établis un tuple de 3 valeurs: (0,0,0)
- Pour chaque valeur on incrémentera sous certaines conditions:
    - (1 si ID; 0 sinon, 1 si class, 0 sinon, nb_de_balise)
- On vérifie ensuite quel expression à la plus haute valeur à la position 0 du tuple, si il y a égalité on passe à la deuxième position, puis à la troisième si besoin.
    - Ainsi il y a un ordre de priorité !
    - ==ID > CLASS > BALISE==

[[Tableau de bord étudiant/L1/Bases de données et prog web/Note de cours/4 - Serveur statique et dynamique]]

# Exercices:

![[Untitled 3 5.png|Untitled 3 5.png]]

![[Untitled 4 3.png|Untitled 4 3.png]]

1. On utilise l’élément not() pour sélectionner toutes les images sauf le logo

```CSS
img :not(\#logo){}
```

  

![[Untitled 5 2.png|Untitled 5 2.png]]

1. ~
    
    ```CSS
    body{
    	background-color: black;
    }
    ```
    
2. ~
    
    ```CSS
    title{
    	color:red;
    }
    ```
    
3. On peu aussi laisser img{} par défaut car le navigateur va donne la priorité à l’ID logo plutôt qu’à la balise image:
    
    ```CSS
    img :not(\#logo){
    	display:block;
    }
    ```
    
4. ~
    
    ```CSS
    \#logo{
    	display:inline;
    }
    ```
    

  

![[Untitled 6 2.png|Untitled 6 2.png]]

1. Le conflit va s’appliquer sur l’élément `<span class="belles-couleurs">images<span>` qui fait à la fois partie de la classe ==`.belles_couleurs`== et est enfant de la balise ==`<p>`==
2. Il va y avoir une priorité sur la classe par rapport au changement depuis la balise. Ainsi la couleur de l’élément `<span class="belles-couleurs">images<span>` sera rouge !