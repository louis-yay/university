---
Complété: true
Type: CM
Materials:
  - https://vanillacademy.com/chapitre/10
Vu en TD: true
---
Lors des échanges entre le navigateur et le serveur, on se demande comment savoir ==de qui vient l’information==. Soit, le schémas suivant:

![[Untitled 9.png|Untitled 9.png]]

Comment faire pour savoir si il s'agit de bob ? de nav 1 ?

- `Http` est client**/**serveur, request**/**response, il est ==stateless== (ne conserve pas les états)
    - Pour compenser, on peu essayer d'utiliser le fingerprinting:

![[Untitled 1 5.png|Untitled 1 5.png]]

Cependant on constate qu’il faudrait envoyer à travers une nouvelles requête et donc, impossible d’associer utilisateur ↔ requête.

Solution: Envoyer un numéro d'identifiant dans l'url.

## Pour maintenir une session entre un nav et un serveur:

Le serveur va envoyer un numéro d'identifiant dans l'url pour ==CHAQUE== ==REQUÊTE== afin que le serveur puisse identifié de qui vient l’information.  
  
**Option 1:**

- On passe le numéro dans l'URL (plus complexe car il faut modifier tout le système d'URL du serveur)

**Option 2:**

- On passe le numéro dans le `header` de la `requête http`
    - Si la première requête n'a pas de numéro, on en génère un, puis on demande au navigateur d'envoyer un numéro pour toute les requêtes qui suivront

## Comment demander au navigateur de renvoyer un numéro pour toutes les requêtes ?

On utilisera le `header` avec les **cookies**

==Def== **==cookie==**==: Un cookie est un petit fichier **stocké** par le navigateur à la demande du serveur (lien entre le cookie et l'adresse du serveur==

Les infos contenues dans le cookies (à la demande du serveur) sont renvoyé dans toutes les requêtes vers le serveur.  
  

![[Untitled 2 5.png|Untitled 2 5.png]]

1. Avant d'envoyer la req, je vérifie s'il y a un cookie pour le serveur  
    -> Si cookie: mettre les  
    ==données dans le header==
2. Le serveur peut demander la création d'un cookie et mettre des données dedans.  
      
    _Dans les headers de la rep:_ `SET-COOKIE cle=url`

  

Cependant, créer des sessions utilisateur de cette manière comporte des risques, impossible de savoir s’il s’agit bien de la même personne ! Etant donné que le serveur ne se base que sur les données des cookies. Il faut que ce mode de connection soit temporaire et inclure dans le serveur une méthode de connection, qui donc, donne accès à une session.

![[Untitled 3 4.png|Untitled 3 4.png]]

Dans le formulaire `/signup`, on récupère à l'aide d'un formulaire.

Pour des raisons de sécurité, on stocke uniquement un hash du password dans la BDD

On convertis donc les données du formulaire en hash et sont stocké dans la base de donnée

- ==(Pour plus de sécurité, on ajoute du 'sel' au mdp avant de le passer en hash ET on stocke le sel dans la BDD)==  
      
    

## Résumé:

Session = numéro pour que le serveur reconnaisse les requêtes qui viennent du même navigateur

- Ce numéro est intégré dans toutes les requêtes
- Il est stocké dans un cookie au sein d'un navigateur, ce dernier est mis en place à la demande du serveur

Compte utilisateur

- Login / password stocké dans le base de donnée sous la forme: login / hash(password + sel)
- On établis le lien entre l'ID de l'utilisateur et le numéro de la session