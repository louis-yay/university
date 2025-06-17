---
Complété: false
Type: Lecture
Materials:
  - https://vanillacademy.com/chapitre/11
Vu en TD: false
---
## Introduction

Lors de l’écriture de page d’une ==page HTML== on se retrouve face à plusieurs problèmes. Tout d’abord, l’écriture de code HTML au sein du serveur rend difficile la lecture et l’écriture du code.  
Ainsi, on peu penser décaler la génération des pages HTML au sein de  
==modules== que l’on importerais au sein du serveur. Cependant, la méthode la plus simple et la plus utilisé aujourd’hui est le ==templating==.

On écrit des pages html, en `.html` , en spécifiant des paramètres de génération dynamique, et, on peu ensuite envoyer ces pages généré dynamiquement au navigateur qui va automatiquement se charger du rendu de la page.

Exemple:

```JavaScript
<!DOCTYPE html>
<html lang="fr">
    <head>
        <title>Mur d'images</title>
    </head>
    <body> 
        <h1>Mur</h1>
        <% for (let f of sFiles) { %>
            <img src="/public/<%=f%>">
        <% } %>
    </body>
</html>
```