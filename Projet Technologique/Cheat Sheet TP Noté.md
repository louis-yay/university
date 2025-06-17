### C
- **Type opaque** -> Type dont la nature est caché, dont on ne peut pas accéder aux composantes internes.

### Formatage
`clang-format-14 -i *.c *.h` -> Formate tout les fichier en `.c` et `.h` suivant les règles d'un fichier `.clang-format`.

### GCC
- `-c` -> Compile depuis un `fichier.c` -> `fichier.o`
- `-o` -> Compile `fichier.o` -> `fichier` (executable)
- `-Wall` -> Affiche tout les warning de compilation
- `-Ox` -> Optimisation à la compilation 
	- `0` -> Peu d'optimisation
	- `3` -> Beaucoup d'optimisation


### SDL
- `(0,0)` -> Coin supérieur gauche de l'image


### Latex

**Tableau**
```Latex
\begin{tabular}{l|c|r}
	\hline
	info1 & info2 \\
	\hline 
	chimie1 & chimie2 \\
\end{tabular}
```

**Ref biblio**
```Latex
\cite{RefID}
```

Et dans le biblib:
```biblio
@misc{ RefID,
	author = {Auteur Ref},
	title = {Titre de la ref jpp},
	year = {2025},
	url = {https://pourquoi/pas/une/url},
	note = "[En ligne; Page disponible le 10-avril-2025]"
}
```

**Images**
```Latex
\usepackage{graphicx}

\includegraphics[height=7.5cm]{bodyschem.png}
```

**Ref & labels**
```Latex
\label{label1}

\ref{label1} -> donne la position dans la table des matières de la ref
\pageref{label1} -> donne la page de la ref
```

### CMake
`Make test` -> Run l'ensemble des tests dispo
`Make ExperimentalMemCheck` -> Vérifie les fuites de mémoires
`Make ExperimentalCoverage` -> Calcule le coverage général des tests du projet
	- Necessite `--coverage` dans les options de compilation
	- Vue facile avec l'extention VSC gcov Viewver `CTRL+SHIFT+P -> gcov viewer show`
