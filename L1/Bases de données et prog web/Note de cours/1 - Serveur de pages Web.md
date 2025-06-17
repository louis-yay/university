---
Complété: true
Type: CM
Materials:
  - https://vanillacademy.com/chapitre/1
Vu en TD: true
---
Le web est ensemble des ==pages web== que l’on peut afficher à partir des ==ressources== des différents ==serveurs==.

- **Qu'est ce qu'une** ==**page web**== **?**
    - Une page web est constituée de plusieurs ressources.
    - Une page web est affichée dans un navigateur web (elle existe dans celui-ci)
- **Qu’est ce qu’une** ==**ressource**== **?**
    - HTML, CSS, JS, Images, Vidéos, etc…
        - Le navigateur assemble les ressources envoyé par le serveur

  

Ainsi, le navigateur demande au serveur des ressources qu’il fournit afin que ce premier puisse les assembler et construire la ou les pages webs.

![[Untitled 15.png|Untitled 15.png]]

---

## Plus petit serveur Web:

On peu appeler un ‘plus petit serveur’ un serveur qui fournit une unique ressource quel que soit la demande faire par le ou les navigateurs.  
Il faut donc sur notre dit serveur:  

- Un fichier html: _index.html_
- Et le code du serveur.

  

On utilisera ici:

```HTML
<html>
    <body>
        <p>
            C'est la seule page que j'ai.
        </p>
    </body>
</html>
```

Et pour le code du serveur, un fichier server.js:

```JavaScript
const http = require("http"); 
const server = http.createServer(); 
const fs = require("fs");

server.on("request", (request, response) => {
  const indexPage = fs.readFileSync("./index.html", "utf-8"); 
  response.end(indexPage); 
});

const port = 8080;
server.listen(port, () => {
  console.log("Server running");
});
```

_CF fichier pour le code commenté_

  

## URL (**Uniform Resource Locator)**

Le navigateur ne pouvant demandé qu’une seul ressourcé par requête, les ressources associé à une pages web doivent toutes avoir une unique url menant jusqu’à elle.

A chaque ressources correspond une URL et réciproquement.

![[Untitled 1 8.png|Untitled 1 8.png]]

![[Untitled 2 8.png|Untitled 2 8.png]]

Aussi, une fois que l’on à spécifié au serveur la manière dont on écrit les URL, il est possible de, au sein d’une page html, crée des liens qui interconnecterons ces pages entres elles. De cette manière on peut utiliser la balise suivante:

```HTML
<a href="/b">Lien vers la page index.html !<\a>
```

On spécifie le liens d’accès _”/b”_, et, comme spécifier dans le code d’écriture du serveur, on accèdera à la page en question !  
  

_server.js_

```JavaScript
[...]
server.on("request", (request, response) => {
  console.log(request.url)

  if (request.url === '/a') {
  const indexPage = fs.readFileSync("./index.html", "utf-8"); 
  response.end(indexPage);
  } else if (request.url === '/b') {
    const indexPage = fs.readFileSync("./hello.html", "utf-8"); 
    response.end(indexPage); 
  } else {
    response.statusCode = 404;
    response.end("Error");
  }

});

[...]
```

_CF fichier pour code complet et documenté_

![[Untitled 3 7.png|Untitled 3 7.png]]

[[Tableau de bord étudiant/L1/Bases de données et prog web/Note de cours/2 - HTML]]