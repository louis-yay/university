---
Reference:
  - https://moodle.u-bordeaux.fr/pluginfile.php/922898/mod_resource/content/7/02-cours-compil.pdf
  - "[[Makefile.txt]]"
Date: 2024-09-06
Reviewed: true
---
### Compilation fichier unique

Lors de la compilation d’un programme en C, on “fusionne” plusieurs fichiers afin de créer un exécutable.

![[image 9.png|image 9.png]]

- _hello.c_: fichier source avec la fonction main
- _stdio.h_: fichier entête (header) → déclaration de la fonction printf
- _libc.a_ : bibliothèque C → incluant le code objet de la fonction printf
- _a.out_ : fichier exécutable, produit par le compilateur gcc  
      
    

En réalité, le compilateur gcc est encore plus complexe que ça et cache encore plusieurs étapes:

![[image 1 6.png|image 1 6.png]]

- phase de prepocessing `cpp, gcc -E, génère des .i`
- phase de compilation en langage assembleur `cc, gcc -S, génère des .s`
- phase assembleur `as, gcc -c, génère une fichier objet .o`
- phase d’édition de lien `ld, gcc -o, génère un exécutable`

==Exemple:  
  
==On suppose un fichier hello.c utilisant la fonction `printf`  
On veut générer un fichier exécutable  
`hello`  
  
  
**Compilation  
  
**`**gcc -std=c99 -c hello.c**` **→** Compilation, génère hello.o  
  
  
**Génération de l’éxécutable**  
  
`gcc hello.o -o hello` → Edition des liens (génère hello)

![[image 2 3.png|image 2 3.png]]

  

  

### Pour une compilation de fichiers séparés:

On transformera dans un premier temps l’ensemble des fichiers au format assembleur:  
  
`gcc -std=c99 -c foo.c bar.c foobar.c` → Compilation des *.c au format .o  
  
On génère ensuite l’exécutable à partir des précédents fichiers:  
  
`gcc foo.o bar.o foobar.o -o foobar` → édition de lien

![[image 3 3.png|image 3 3.png]]

On ajoute dans les fichiers *.h la déclaration des fonctions. On définit ensuite ces fonctions dans leurs fichier .c respectifs. On ajoutera \#include “fichier.h”

  

### Bibliothèque

Lorsque notre code fait appelle à des bibliothèque, il est nécessaire de le spécifier lors de la compilation:

- **Utilisation d’un bibliothèque statique standard (libm.a → option -lm)**
    - `gcc foo.o bar.o foobar.o -o foobar -lm` → bar.c utilise `sqrt`
- **Génération d’une bibliothèque statique**
    - `**gcc -c std=c99 foo.c bar.c**` **→** génération de foo.o et bar.o
    - `ar rcs libfb.a foo.o bar.o` → génération de libfb.a
- **Utilisation d’une bibliothèque statique “maison” (libfb.a → option -lfb)**
    - `gcc -std=c99 -c foobar.c` → génération de foobar.o
    - `gcc foobar.o libfa.a -o foobar` → génération de l’exécutable
- Ou de manière plus élégante avec -lfb (et -L.)
    - `gcc foobar.o -o foobar -lfb -L.` # génération de l’exécutable  
          
        

  

**Résumé des options du compilateur gcc:**

- `-c` : option pour compiler des fichiers source (.c) en fichier objet (.o)
- `-std=c99` : spécifie le standard que le code source satisfait
- `-o <fichier>` : génère l’exécutable dans <fichier> au lieu de a.out
- `-Wall` : (Warning ALL) affiche tous les message d’avertissement sur votre  
    code  
    
- `-l<nom>` : inclut la bibliothèque lib<nom>.a lors de la génération de  
    l’exécutable (la bibliothèque libc.a est implicitement inclut)  
    
- `-L <rep>` : ajoute <rep> à la liste des répertoires consultés pour trouver les fichiers .a des bibliothèques

  

---

## Makefile

Pour simplifier la compilation, on utilise un fichier `Makefile` associé à l’outil `make`

```Makefile
cible1: dep1 dep2 dep3
	commande ...
	
dep 1: autre_dep1 autre_dep2 ...
	autre_commande ...
```

  

Exemple:

```Makefile
ALL : hello

hello : hello.o
	gcc hello.o -o hello
hello.o : hello.c
	gcc -c hello.c
	
clean:
	rm -f hello.o hello
```

![[image 4 3.png|image 4 3.png]]

  

### Builtin & compléments

Pour simplifier le fichier makefile, on peut préciser des variables builtin afin de ne plus spécifier que les dépendances:

```
Makefile
CC= gcc   # Compilateur 
CFLAGS= -std=c99 -Wall -O2  # Options
ALL : hello
hello : hello.o
hello.o : hello.c
```

On peut aussi faire appel à des compléments qui permet de généraliser l’écriture des commandes à exécuté avec les makefile:

- La variable `$@` représente la cible courante
- La variable `$^` représente la liste des dépendances
- La variable `$<` représente la première dépendance

Exemple:

```Makefile
convert: convert.o tsp.o memoire.o matrice2d.o
	$(CC) $(CFLAGS) $^ -o $@ $(LDLIBS)
```

### Netoyage

```Makefile
.PHONY : clean
clean :
rm -f *.o *.a foobar
```