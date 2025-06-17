---
Reviewed: false
---
Les ==bases de données== sont des collections organisé d’information qui peuvent être facilement accessible, managé et mise à jours.

Il existe différents type de bases de données et une de ces dernières est Redis, qui est une base de données dites “en-mémoire” qui va exploiter la RAM du système au lieu du disque dur, permettant ainsi une rapidité bien plus intéressante. _Ce système est majoritairement utilisé pour les information nécessitant un accès rapide (prix sur une page de vente par exemple_

> Le plus gros des données sera stocké sur des système de DB plus classique comme MySQL ou MongoDB.

  

En utilisant la procédure de connection classique, on peu observer après le nMap un port ouvert et réservé pour redis:

![[Untitled 7.png|Untitled 7.png]]

  

### Qu’est ce que Redis ?

Redis (**RE**mote **DI**ctionary **S**erver) est un système avanvcé opensource de NoSQL. Les données sont stocké sous forme de clé-valeur, comme un un dictionnaire. *Redis fait des backup sur le disque dure pour prévenir des erreurs / pertes de données.

**Information complémentaires:**

- Redis tourne côté serveur, le serveur écoute les connections depuis les clients, depuis un programme ou à travers un interface de commande (CMD)
- L’interface de commande en ligne (CLI) est un outil puissant pour avoir un accès complet aux données de Redis et à ses fonctionnalités
- Les données sont stocké initialement sur la RAM puis copié en backup sur le disque dur.

  

On peu maintenant installé l’outil Redis en procédant de la manière habituelle:

```Bash
sudo apt infoinstall redis-tools
```

  

Une fois le service redis-cli, on peu rechercher la liste des commandes:

```Bash
redis-cli --help
```

  

Dans notre cas on va chercher à se connecter à notre cible, on utilisera donc:

```Bash
redis-cli -h {target_IP}
```

  

Une fois correctement connecté, on peu énuméré les informations de notre cible en utilisant la commande info:

![[Untitled 1 3.png|Untitled 1 3.png]]

On peu noté en dessous de \#Keyspace qu’il existe une base de données en mémoire, à l’index 0. On procède donc à sa séléction:

```Bash
select 0
```

On peu ensuite accéder à l’enssemble des clées:

```Bash
keys *
```

Et une fois la clé “flag” repéré, il ne reste plus qu’à y accéder:

```Bash
get flag
```

![[Untitled 2 4.png|Untitled 2 4.png]]