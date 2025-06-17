---
Complété: true
Type: CM
Materials:
  - https://vanillacademy
Vu en TD: true
---
> Les navigateurs peuvent exécuter du code Javascript dans une page web et ainsi supporter des interactions riches et complexes.

Ainsi, on peu, dans les outils du développeur, executer du code `JS`

  

## Les objets Window et Document

Au sein de l’environement JS, associé à une page web, on retrouve plein de différents **objets** avec des **propriétés.** Deux objets principaux sont `Window` (fenêtre utilisateur) et `document` (arbre DOM).  
Voici quelques unes des propriété de window:  

```JavaScript
window.innerHeight // Hauteur de la fenêtre
window.innerWidth // Largeur de la fenêtre
window.location // URL de la page
window.navigator // Informations sur le navigateur
window.screen // Informations sur l'écran
window.document // Document HTML de la page
```

  
On peu accéder aux différents éléments du document en utilisant la/les méthodes query sur les sélecteurs CSS ou les balises:  

```JavaScript
document.getElementById("images") // Accès à la balise <div id="images">
document.getElementsByClassName("commentaire") // Accès aux balise <div class="commentaire">
document.querySelector("\#images") // Accès à la balise <div id="images">
document.querySelectorAll(".commentaire") // Accès aux balise <div class="commentaire">
document.getElementById("images").querySelectorAll("img") // Accès aux balise <img> à partir de la balise <div id="images">
document.getElementById("images").children[0] // Accès au premier fils à partir de de l'élément <div id="images">
```

Aussi, une fois qu’on à la main mise sur un élément, il est possible d’accéder et de modifier toutes ces propriétés. Par exemple pour les balises d’ID ==images:==

```JavaScript
let imagesDiv = document.getElementById("images");
let images = imagesDiv.querySelectorAll("img");
for (let i = 0; i < images.length; i++) {
    if (images[i].src) {
        images[i].src = '/public/images/image1_small.png';
    }
}
```

## Evénements

Une fois qu’on a sélectionné un objet, ou groupe d’objet, on peu ajouter un `EventListener` qui permet d’attendre des évènements sur cette objet et d’y associer une action. Par exemple:  
  

```JavaScript
let par = document.querySelectorAll('p');

// Puis:
for(let i =0; i<par.length; i++){
    par[i].addEventListener('click', (e)=>{
    par[i].textContent = 'cool';
  });
}
```

  

### Propagation des évènements

Les évènements JS déclenché par une interaction utilisateur vont remonter (bubbling) jusqu’à la balise la plus haute et s’executer.  
On peu cependant stopper cette propagation:  
  

```JavaScript
let form = document.getElementById('login-form');
let fname = document.getElementById('fname');
let lname = document.getElementById('lname');
form.addEventListener('submit', (e) => {
    if (!fname.value || !lname.value) {
        alert('please enter First and Last names')
        e.preventDefault();
    }
})
```

  

## Requête HTTP

On peu utiliser le JS pour envoyer des requête http, exemple pour récupérer des commentaires:  
  

```JavaScript
fetch('/commentaires')
.then((result) => {
    //récupérer les nouveaux commentaires
    //les intégrer dans le DOM
})
```

  

## Intégrer le code JS dans une page HTML

> L'intégration d'un code Javascript dans une page web se fait grâce à la balise `<script>`. Cette balise peut être placée n'importe où dans le document HTML. Le code Javascript peut être directement intégré dans la balise ou se trouver dans une ressource qui est référencée (propriété **src** de la balise).

```JavaScript
<html>
    <head>
        <script>
            console.log('Script1');
            let script1 = true;
        </script>
    </head>
    <body>
        <h1>Javascript</h1>
        <script>
            console.log('Script2');
            if (script1) {
                console.log('Script1 was ok');
            }
            let script2 = true;
        </script>
    </body>
    <script type="text/javascript" src='/public/script3.js'></script>
</html>
```

[[Tableau de bord étudiant/L1/Bases de données et prog web/Note de cours/10 - Session et compte utilisateur]]