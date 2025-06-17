---
Complété: true
Type: CM
Materials:
  - https://vanillacademy.com/chapitre/7
Vu en TD: true
---
## Exercice: Conception BDD

On souhaite une base de donnés pour gérer les UE, les groupes et les étudiants.

- Une UE a un code unique, un nom court et un nom long.
- Un groupe a un nom, un étudiant à un nom et un prénom
- Une UE à plusieurs groupes et un groupe à plusieurs UE

![[Untitled 17.png|Untitled 17.png]]

  

# Cours

_On veut trouver des donnés, si on trouve, c’est vrai !_

### Opérations mono table (algèbre relationnel)

- Projection: on projette des colonnes
    
    ![[Untitled 1 10.png|Untitled 1 10.png]]
    
- La selection: on sélectionne des lignes (formule logique) qui exprime le filtre.
    
    ![[Untitled 2 10.png|Untitled 2 10.png]]
    

## Opérations double-tables

- Le produit cartésien: R1xR2 → R3
    
    ![[Untitled 3 9.png|Untitled 3 9.png]]
    
      
    
      
    
    Résumé:
    
    ![[IMG_0676.jpeg]]
    

  

## Fonctions pour calculer sur le résultat

- Count(R)
- Order(R)
- Limit(R)
- Avg(R)

  

## Requête imbriqué

_Exemple_: ==Une première requête== trouve la valeur maximum **MAX()** et qu'==une deuxième requête== cherche la ligne qui contient cette valeur maximum.

```SQL
SELECT * FROM auteurs JOIN images ON auteurs.id = images.auteur 
WHERE likes = (SELECT MAX(likes) FROM images);
```

  

Il est aussi possible d’utiliser des ==quantificateurs== au sein d’une requête SQL.

- Le ==quantificateur== ==**ALL**== vérifie qu'une condition est vraie pour toutes les lignes d'une table
- Le ==quantificateur== ==**ANY**== vérifie qu'une condition est vraie pour au moins une ligne d'une table.

```SQL
SELECT * FROM auteurs WHERE id = ALL (SELECT auteur FROM images);
```

[[Tableau de bord étudiant/L1/Bases de données et prog web/Note de cours/8 - Liens entre serveur et BDD]]

## Exercices:

![[Untitled 4 6.png|Untitled 4 6.png]]

![[Untitled 5 4.png|Untitled 5 4.png]]