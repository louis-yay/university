---
Complété: true
Type: CM
Materials:
  - https://vanillacademy.com/chapitre/6
Vu en TD: true
---
Lors des 3 prochains chapitre, nous allons apprendre comment fonctionne une base de données, comment on la code etc…

![[Untitled 16.png|Untitled 16.png]]

## Structure des bases de données

On va avoir des données auxquels on va associé des structures:

- Commentaire ==⇒ Max 200==
- Text
- Orientation ==⇒ “Portrait”==
- Portrait paysage

A l’aide de la structuration de ces données, on peut interroger la base de données de manière logique. Par exemple: ==quels sont les images en portrait avec un commentaire qui dit: “bien” ?==

Ainsi, on construit une base de données de la manière suivante:

1. Quels est la connaissance que je souhaite obtenir ?
2. Je pose une structure de mes données
3. Je pose mes données

  

**La connaissance c’est (+/-) trouver des datas dans un type de structure:**

Dans la base de données relationnelle, on utilise des tableaux à 2 dimension.

Les cases du tableaux ne contiennent que des données atomiques, impossible à décomposé.

- Bool, entier, char, float

==Attention, en fonction des bases de données, on utilise pas forcément la même structure de données !!==

  

On veut ranger des données dans des tables.

- On va identifier les concepts exprimées dans les données ==(ex: images, auteurs, ..)==
- → L’idée est de rattacher des données cohérentes dans un concepts ⇒ ==Une table==
- Nommer les tables:
    
    ![[Untitled 1 9.png|Untitled 1 9.png]]
    
    - Chaque table possède des colonnes qui définissent le concept

  

### Table:

- Exprime un concept ⇒ Nom
- Plusieurs colonnes qui définissent le concept (type de base)
- Une ligne = une entité
    - Chaque ligne exprime quelque chose, il faut donc être capable ==d’identifier de manière unique== une ligne
    - Avoir deux fois la même ligne, c’est ==useless.==

Dans une table, il doit y avoir une colonne ou une combinaison, la clé, qui identifie une ligne de manière unique.

![[Untitled 2 9.png|Untitled 2 9.png]]

Plusieurs concepts. Chaque concept correspond à sa table (id)

### Il existe des relations entre les concepts

- Comment les structurer ?
- 2 Structures incontournable:
    1. 1 (truc) contient plusieurs (bidules)
        
        ![[Untitled 3 8.png|Untitled 3 8.png]]
        
        1. 1 Groupe contient plusieurs étudiant
        2. 1 photographe contient plusieurs photos
    2. Relation de partage
        
        ![[Untitled 4 5.png|Untitled 4 5.png]]
        
        1. Etu ↔ UE
        2. On passe par une table intermédiaire qui fait le lien entre Etu et UE

(S’entrainer sur la conception, savoir s’il faut contenir, ou partager, entre les tables)

  

### Formalisation:

> D'un point de vue formel, une base de données stocke les données sous forme de relations. Le terme relation est utilisé ici dans son sens mathématique : étant donné des domaines **D1, D2, ..., Dn**, **R** est une relation sur ces **n** domaines si elle est un ensemble fini de nuplets appartenant au produit cartésien des domaines (**D1 X D2 ... X Dn**). Une relation est donc un sous ensemble fini du produit cartésien des domaines.
> 
> Par exemple, les données contenues dans la table des images sont définies formellement par le produit cartésien : **(id X nom X date)**. Un élément de la relation est le nuplet **(1, 'Charles Baudelaire', 1863)**.

Un schémas relationnelle précise le nom de la relation ainsi que le nom des types de ses domaines (attributs). Comme par exemple: **$auteur(id, prenom(char(30)), nom(char(30)))$**﻿**.**

  

Une clé primaire est un attribut ou une combinaison d’attributs qui satisfait:

- ==Une contrainte d'unicité== chaque valeur clé désigne de manière unique un nuplet de la relation.
- ==Une contrainte de minimalité== : si une clé est composée d'un ensemble d'attributs, cette combinaison doit être minimale (aucun attribut ne peut en être retiré sans violer la règle d'unicité)
- ==Lorsqu'une relation== possède deux ou plusieurs clés primaires non redondantes, l'une d'entre elles est choisie arbitrairement et est appelée clé primaire de cette relation

  

L'optimisation d'une base de données vise à respecter des contraintes précisées dans ce qu'on appelle des ==formes normales==.

- Une relation est dite de première forme normale, si elle possède au moins ==une clé primaire== et si toutes les valeurs sont élémentaires.
- Une relation est dite de deuxième forme normale si elle est de première forme normale, et chaque attribut n'étant pas une clé primaire est en ==dépendance fonctionnelle== avec la clé primaire.
- Une relation est dite de troisième forme normale si elle est en deuxième forme normale et si tout attribut n'appartenant pas à une clé n'a pas de dépendance fonctionnelle avec un autre attribut n'appartenant pas à une clé.

[[Tableau de bord étudiant/L1/Bases de données et prog web/Note de cours/7 - Les requêtes des BDD]]