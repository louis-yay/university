---
Complété: true
Type: CM
Materials:
  - https://vanillacademy.com/chapitre/8
Vu en TD: true
---
Le protocole de communication avec la base de donnés dépend du vendeur de la BDD

[](https://www.notion.soundefined)

On a une architecture à 3 couches, interdiction de faire des échanges entre les couches 1 et 3.

Pour communiquer entre le serveur et la BDD il faut utiliser un driver propriétaire:

- pour une base de donnée (psql)
- Un langage de prog (js)
    - On utilise donc: PG.

_On peut utiliser docker pour faciliter la chose_

  

### Installer Pg

Pour utiliser pg, il faut l’importer dans le code source de notre serveur:

```Bash
npm init
npm install pg --save
```

On initialise un projet et on y installe le driver pg.

  

### Pg et JS

Pour commencer on initialiser une nouvelle instance client, et on se connecte à la base de données.

```JavaScript
const { Client } = require('pg');

const client = new Client({
    user: 'postgres', 
    password: 'root', 
    database: 'application_image',
    port : 5432 
});

client.connect()
.then(() => {
    console.log('Connected to database');
})
.catch((e) => {
    console.log('Error connecting to database');
    console.log(e);
}); 
```

  

On peu ensuite interroger notre base de données de la manière suivante:

```JavaScript
client.query('SELECT * FROM images')
.then((res) => {
    console.log(res.rows);
})
.catch((e) => {
    console.log('Error executing query');
    console.log(e);
}); 
```

  

La requête va renvoyer des données (rows) qui prendrons le format suivant:

```JavaScript
let res = {
    rows : [
        {
            id: 1,
            nom:"Portrait Bleu",
            fichier: "Image1.jpg"
        },
        {
            id: 2,
            nom:"Marcheur",
            fichier: "Image2.jpg"
        },
        {
            id: 3,
            nom:"Sol Rouge",
            fichier: "Image3.jpg"
        }
    ]
};
//On accède aux données de cette manière:
let nomFichierDeuxiemeImage = res.rows[1].fichier;
```

  

On peu aussi utiliser ces méthodes de manière synchrone:

```JavaScript
let res = await client.query('SELECT * FROM images') //La ligne suivante sera exécutée après la fin de l'exécution de la requête
console.log(res.rows);
```

Note: Il est également possible d’utiliser la méthode query pour envoyer des requête visant à ajouter / modifier des éléments dans la base de données !

  

**Connexion**  
Il est possible d’établir la connexion à la BDD à plusieurs moments dans notre code, cependant, on préfèrera l’utiliser au début du code du serveur, établissant une unique performance (+ rapide, - sûr)  

  

[[Tableau de bord étudiant/L1/Bases de données et prog web/Note de cours/9 - Gestion des évènements en JS]]