# Évaluation Croisée du Projet

**Projet Évalué** : `[B16]`

## Consignes

Pour chaque question, on attend une réponse courte (oui/non/partiellement) qui
doit être suivie d'une argumentation précise et concise. L’argumentation est
essentielle et doit remplir 2 objectifs : justifier votre réponse courte et
proposer des pistes d’améliorations du code.

Par exemple :

* Question - Le code est-il correctement indenté ?
* Réponse - Partiellement. La fonction truc bidule() du fichier n’est pas
  correctement indentée. Le fichier truc.c n’est pas indenté de la même manière
  que les autres fichiers.

## Questionnaire à remplir

**Question 01** - L’archive du dépôt Git est-elle propre ? (Présence d'un
fichier `README.md` bien écrit, présence des fichiers `.clang-format`,
`.gitignore`, ... Absence de fichiers superflux, temporaires, ou d'exécutables,
...)
```
Partiellement. Présence d'un fichier README.md complété avec avec les noms d'auteur et instruction de compilations, repertoire sans fichier de compilation.
Présence de fichier superflux:
- wassim_haddadou_commit1.txt
- leaticie.txt
- Anis_Kasdi_commit1.txt
- sample.c
- test_queue.c
- game_test_ext.c 
- game_moves.h
```

**Question 02** - La compilation se fait-elle correctement ? (Les exécutables et
la bibliothèque sont générés sans erreurs ni warnings.)

```
Oui, le programme compile sans erreurs ni warning.
[100%] Linking C executable game_test_is_wrapping
```

**Question 03** - Est-ce que le programme `game_text` s'exécute sans erreurs, ni
bugs ? Arrivez-vous à gagner le jeu par défaut ?

```
Partiellement, le jeu s'execute sans bug, il est possible de finir le jeu
Il manque cependant la commande r, permetant de shuffle le jeu.
   0 1 2 3 4 
   -----------
0 |┌ < > ┐ v |
1 |├ ┬ ┬ ┴ ┤ |
2 |^ ^ ├ < | |
3 |> ┬ ┴ ┐ | |
4 |> ┴ < ^ ^ |
   -----------
Félicitations, vous avez gagné !
```

**Question 04** - Est-ce que les tests fournis s'exécutent sans fautes ?
Pouvez-vous ainsi identifier des bugs non corrigés ? Justifiez quelle fonction
ou ligne de code est responsable.

```
Partiellement, un seul test ne passe pas, test_game_default
97% tests passed, 1 tests failed out of 31

Total Test time (real) =   0.03 sec

The following tests FAILED:
          5 - test_ankasdi_game_default (Failed)

La fonction de test game_default appelle une autre fonction, game_check de nature:
game_get_piece_shape(g, 0, 0) == 3 &&
game_get_piece_orientation(g, 0, 0) == WEST &&
game_get_piece_shape(g, 0, 1) == 1 &&
[...]
game_get_piece_shape(g, 4, 4) == 1 &&
game_get_piece_orientation(g, 4, 4) == SOUTH;

Le nombre de test et le manque de clareté dans cette fonction est certainement la source de l'erreur.

```

**Question 05** - Quelle est la couverture du code ? Y a-t-il selon vous
suffisamment de tests, y compris pour les extensions du jeu (v2) ?

```
Le coverage totale est de 63%
        Covered LOC:         1039
        Not covered LOC:     599
        Total LOC:           1638
        Percentage Coverage: 63.43%
Les tests ne passent pas dans les conditions avec une game warping. Exemple:
if (g->wrapping) {
	hh2 = game_has_half_edge(g, g->nb_rows - 1, j, SOUTH); // jamais testé
}

Dans les tests de game_test_lakacimi.c, certaine fonctions ne sont jamais utilisé: (get_random_dir_tab, get_random_shape_tab)
De même dans game_test_ext, test_undo_redo_some, test_undo_redo_all, test_undo_one test_copy_ext test_equal_ext et check_game_ext. Ne sont jamais appelé.
Il n'y a pas non plus de test sur le game_test

Les tests sont globalement correctes en dehors du coverage des tests, la plus part des lignes non évalué sont majoritairement issues de vérifications de pointeurs NULL et donc jamais executé.
```

**Question 06** - Est-ce que les tests fournis s'exécutent sans fuites mémoire ?
Justifiez quelle fonction ou ligne de code est responsable.

```
Les tests génères plusieurs fuites de mémoires:
Memory Leak - 9
Uninitialized Memory Conditional - 36
Uninitialized Memory Read - 5
Il y a 13 tests qui sont responsables des fuites de mémoires.
Les fonction concernée sont:
3/31 MemCheck: #3: test_ankasdi_get_adjacent_square ............     Defects: 5
4/31 MemCheck: #4: test_ankasdi_game_is_well_paired ............     Defects: 18
6/31 MemCheck: #6: test_ankasdi_game_default_solution ..........     Defects: 4
7/31 MemCheck: #7: test_ankasdi_test_game_check_edge ...........     Defects: 1
8/31 MemCheck: #8: test_ankasdi_test_game_has_half_edge ........     Defects: 1
13/31 MemCheck: #13: test_game_equal .............................   Defects: 3
14/31 MemCheck: #14: test_game_copy ..............................   Defects: 1
15/31 MemCheck: #15: test_game_new ...............................   Defects: 3
18/31 MemCheck: #18: test_whaddadou_game_orientation .............   Defects: 1
20/31 MemCheck: #20: test_whaddadou_game_won .....................   Defects: 4
24/31 MemCheck: #24: test_whaddadou_game_play_move ...............   Defects: 1
30/31 MemCheck: #30: test_game_undo ..............................   Defects: 3
31/31 MemCheck: #31: test_game_redo ..............................   Defects: 5

Par exemple pour #8: test_game_has_half_edge, une game est crée à partir de game_new_empty() mais n'est jamais libéré, il suffit d'ajouter un game_delete à la fin de la fonction pour libérer l'espace occupé.
Autre exemple dans test_game_new,
On constate une erreur: Conditional jump or move depends on uninitialised value(s)
Cette dernière provient de la création d'un nouveau jeu avec une grille vide game g = game_new(NULL, NULL);
La fonction equal va tenter de vérifier des données sur une grille non_initialisé.
Cette erreur peut se régler en initialisant la grille avant d'appeler la fonction equal.
```

**Question 07** - La bibliothèque fournie peut-elle remplacer votre propre
bibliothèque sans générer de problèmes ? Vérifiez avec `game_text`.

```
En remplaçant ma bibliothèque par celle du groupe B16, je peux executer game_text sans aucun problème. Le jeu est jouable sans dificultés.
```

**Question 08** - Est-ce que la bibliothèque fournie arrive à passer vos tests ?
Pouvez-vous ainsi detécter de nouveaux bugs ou fuites mémoire dans cette
bibliothèque ? Justifiez quelle fonction ou ligne de code est responsable.

```
Les tests passent tous, et on détecte moins de fuites mémoires lors de l'execution avec nous tests.
100% tests passed, 0 tests failed out of 33

Total Test time (real) =  10.88 sec
-- Processing memory checking output:
8/33 MemCheck: #8: test_ldenys_get_piece_orientation ..............   Defects: 1
30/33 MemCheck: #30: test_epavarykhin_new_ext .......................   Defects: 1
Cependant ce sont des erreurs que l'on retrouve également en compilant notre propre projet, des bugs qui sont donc lier à l'implémentation de nos tests
```

**Question 09** - A l'inverse, est-ce que les différents exécutables fournis
(`game_text` et tests) fonctionnent avec votre propre bibliothèque ? Justifiez
tout problème découvert, y compris dans votre propre projet.

```
Make test renvoie la même erreur qu'avec la bibliohtèque d'origine:
5 - test_ankasdi_game_default (Failed)

En ce qui concerne le fuites de mémoires:
3/31 MemCheck: #3: test_ankasdi_get_adjacent_square ............   Defects: 6
8/31 MemCheck: #8: test_ankasdi_test_game_has_half_edge ........   Defects: 1
Memory checking results:
Memory Leak - 1
Uninitialized Memory Read - 6

On constate qu'il y a moins de fuites mémoires, on a des fuites de mémoires venant de plusieurs fonction appelé par test_get_adjacent_square, ces erreurs n'aparaissant pas lors de l'autre croisement, on peut en déduire que l'implémentation du test de get_adjacent_square pose problème.
```

**Question 10** - Est-ce que le code est propre et facilement lisible ?
(Indentation cohérente, nommage cohérent des variables et des fonctions, pas de
constantes magiques, fonctions de longueur raisonnable, pas de code mort ou de
duplication inutile, ...)

```
Le code est globalement bien indenté.
La notation de la structure game manque de clareté:
struct game_s {
	direction **d;
	shape **s;
	bool wrapping;
	uint nb_rows;
	uint nb_cols;
	queue *history;
	queue *redo_history;
};
Les appelations 's' et 'd' crée de la confusion dans le code car pas suffisement explicite.

On constate des block lourds de code dans game_default_solution() et default_check() qui définissent la game par defaut, ce code peut être factorisé sous forme de tableau comme fait dans game_default().
On constate également de la duplication de code dans les fonctions game_check_edge(), game_new_ext() (pourrait être améliorer en remplaçant la double grille de game par une seul grille de pièce qui rassemble les direction et les shapes).

Globalement le code est assez lisible. Seul problème sur la clareté, certaine fonction dont la définition se trouve dans le fichier game.h se retrouvent dans game_ext.c, et vice versa avec game_aux également. Il serait plus cohérent de remettre les fonctions à leurs bonne place.
```

**Question 11** - Le code est-il suffisament commenté ?

```
Partiellement, le code est très bien commenté pour la plus part des fonctions, ajoutant des informations complémentaire à la bonne relecture du code, cependant certaine fonction n'ont pas de commentaire.
Une voix d'amélioration serait commenter de manière uniforme sur toutes les fonctions.

```

**Question 12** - Est-ce que le code est complet et fonctionnel ? Dire si la V1
est correctement implémentée et lister les extensions de la V2 qui sont
correctement implémentées.

```
Le code de la V1 est presque complet, il manque dans game_text la possibilité de shuffle le jeu alors que cette fonctionalité est implémenté et fonctionnelle.
De plus, la fonction is_connected, ne prend pas en compte le wrapping, et ne semble pas non plus être implémenté correctement. La fonction voyage de pièce en pièce en partant d'une pièce initial vers une autre pièce si la pièce initiale à une sortie dans la direction en question, sans prendre en compte s'il y a un MISMATCH entre les deux pièces.
```

**Question 13** - Donnez un appréciation générale. Voyez-vous d'autres pistes
d'amélioration ?

```
Le code est globalement bon, la présence de gros block de codes faits taches sur le reste.
La présences de fichiers complémentaire et inutile pour certain en plus l'absence de partition des fichiers rend le code dificile à parcourir, on se perd facilement dans l'arborécence.
Re-aranger votre projet en suprimant les fichiers inutile, netoyant le code comme dit précedement, ainsi que réfléchir à une organisation en dossier du projet peut réellement améliorer la qualité de votre travail.
```

---