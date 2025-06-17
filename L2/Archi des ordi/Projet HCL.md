Projet réalisé en collaboration:
- EL-MOUTAKI Fatine
- DENYS Louis


### Exercice 1
#### Question 1
On peut ré-écrire `RMMOVL` et `MRMOVL` en utilisant l'`icode` et l' `ifun` comme indiqué dans l'énoncé.
On commence par modifier le set d'instruction en utilisant le même icode pour que `MRMOVL` & `RMMOVL` ai le même `icode`.

Ainsi on peut remplacer lorsqu'un seul des deux est présent dans une instruction, ai sein du code HCL par:
```python
MRMOVL => ((icode == RMMOVL) && (ifun == 1))
RMMOVL => ((icode == RMMOVL) && (ifun == 0))
```
C'est le cas dans les exemples suivants:
```python

## Instruction de base
int dstM = [
    icode in {POPL, RMMOVL}  : rA;
    1 : RNONE;  # Don't need register for writing
];

## Remplacement par la formule
int dstM = [
    icode == POPL || ((icode == RMMOVL) && (ifun == 1))  : rA;
    1 : RNONE;  # Don't need register for writing
];

```

Et dans le cas où les deux sont présent, on ne conservera que la présence de `RMMOVL` par exemple pour 
```python
bool need_regids =
    icode in { RRMOVL, OPL, IOPL, PUSHL, POPL, IRMOVL, RMMOVL };
```


#### Question 2
On modifie la constante `RMMOVL` par `MMMOVL` au début du code HCL:
```
intsig RMMOVL                   'instructionSet.get("rmmovl").icode'

## Par:
intsig MMMOVL                   'instructionSet.get("rmmovl").icode'
```
On remplace ensuite toutes les occurrences de `RMMOVL` par `MMMOVL`.

On test nos modification avec le programme suivant
```Python
## On test avec:
        .pos 0
init:   mrmovl x, %eax    # Copie de x -> %eax
        rmmovl %eax, y    # Copie de %eax -> y

        .pos 100
x:      .long 15          # Initialisation variable mem x
y:      .long 0           # Initialisation variable mem y
```

### Exercice 2
#### Question 1
On crée une nouvelle instruction dans le jeu: `JREG icode=5 ifun=0` 
#### Question 2
On commence par ajouter `JREG` dans les instruction du HCL en définissant une nouvelle constante à son nom 
```python
intsig MMMOVL                   'instructionSet.get("rmmovl").icode'
```

On l'ajoute également dans `need_regids` et `instr_valid`, afin que l'instruction soit reconnue lors de l'execution d'un programme `y86`
```python
bool need_regids =
    icode in { RRMOVL, OPL, IOPL, PUSHL, POPL, IRMOVL, MMMOVL, JREG };

## [...]

bool instr_valid =
    icode in { NOP, HALT, RRMOVL, IRMOVL, MMMOVL,
               OPL, IOPL, JXX, CALL, RET, PUSHL, POPL, JREG } ;

```

On définit ensuite la source A, qui sera donc le registre contenant l'adresse du saut logique à effectuer.
```python
int srcA = [
    icode in { RRMOVL, MMMOVL, OPL, PUSHL, JREG } : rA;
    icode in { POPL, RET } : RESP;
    1 : RNONE;  # Don't need register for reading
];
```

On fait ensuite rentrer la valeur dans l' `UAL`, sans la modifier:
```python
## Select input A to ALU
int aluA = [
    icode in { RRMOVL, OPL, JREG } : valA;
    ## [...]
];

## Select input B to ALU
int aluB = [
    icode in { MMMOVL, OPL, IOPL, CALL, PUSHL, RET, POPL } : valB;
    icode in { RRMOVL, IRMOVL, JREG } : 0;
    # Other instructions don't need ALU
];
```
Pour cela on effectue une addition entre la valeur de branchement et `0`.

Enfin on modifie le paramètre de mise à jour du compteur ordinal en utilisant `valE`, la sortie de l'UAL, hors comme nous n'avons pas modifié sa valeur, `valE = valA` ici qui correspond à l'adresse stocké dans le registre `rA`
```python
## Compute address of next instruction to be fetched
int new_pc = [
    # [...]
    # Jump on register: Use output of UAL
    icode == JREG : valE;
    # [...]
];
```

Jeu de test:
```python
        .pos 0
init:   mrmovl x, %eax          # Intialisation x -> %eax
        irmovl post, %ebx       # Addr de branchement -> %ebx
        rmmovl %ebx, z          # Addr de branchement -> z (zone mémoire)
        addl %eax, %eax         # Opération UAL quelconque
        jmem z
        isubl 10, %eax          # Opération UAL quelconque
post:   rmmovl %eax, y          # Sauvegarde des données
        halt
        
        
        .pos 100
x:      .long 15                # variable mémoire x, initialisé à 15
y:      .long 0                 # Variable mémoire y, initialisé à 0
z:      .long 0                 # Variable mémoire z, initialisé à 0

```

#### Question 3
On met également à jour les indicateurs d'états, on comprendra donc un branchement si l'instruction est JREG.
```python
bool is_bch = icode in { JXX, JREG } || ((icode == MMMOVL) && (ifun == 2));
```

Pour le reste il n'y a peu de modification, l'instruction va reprendre les principes de `MRMOVL` en passant la mémoire lu comme prochaine adresse de branchement au lieu de le garder dans un registre.
Pour cela on modifie le système de mise à jour du compteur ordinal:
```python
    # Completion of RET instruction: Use value retrieved from stack
    icode == RET || ((icode == MMMOVL) && (ifun == 2)): valM;
```
Ici `((icode == MMMOVL) && (ifun == 2))` correspond à l'encodage de l'instruction `JREG`

#### Question 4
Fonction switch et jeu de test, `CF FICHIER .json DU SIMULATEUR`

### Exercice 3
#### Question 1
On ajout l'instruction loop: `loop icode=13 ifun=0 args= valC`
Et on écrit un court programme de test:
```python
         .pos 0
        xorl %eax, %eax      # Initialisation d'une variable %eax
        irmovl 10, %ecx      # Initialisation d'n compteur
boucle: iaddl 2, %eax        # Instruction de boucle, on ajoute 2 à %eax
        loop boucle          # On boucle vers ligne 4
        halt                 # Fin du programme 
        
```
Ce petit programme execute 10 addition successive de 10 à la variable `%eax`, qui va donc prendre les valeurs: `2, 4, 6, ..., 20` .

#### Question 2
On ajout dans un premier temps la définition de `loop` et de `%ecx` dans le code HCL:
```python
## Loop:
intsig LOOP                     'instructionSet.get("loop").icode'

## ECX:
intsig RECX                     'registers.ecx'         # Loop index
```

On ajoute ensuite `LOOP` dans les vérification booléenne de départ:
```python
## Does fetched instruction require a constant word?
bool need_valC =
    icode in { IRMOVL, MMMOVL, JXX, CALL, IOPL, LOOP };

## Is instruction valid?
bool instr_valid =
    icode in { NOP, HALT, RRMOVL, IRMOVL, MMMOVL,
               OPL, IOPL, JXX, CALL, RET, PUSHL, POPL, JREG, LOOP } ;
```
Loop utilise `valC` comme adresse de branchement, s'il y a.

On ajout ensuite en entrée de l'unité arithmétique et logique;
```python
int aluA = [
    icode in { RRMOVL, OPL, JREG, LOOP } : valA;
    icode in { IRMOVL, MMMOVL, IOPL } : valC;
    icode in { CALL, PUSHL } : -4;
    icode in { RET, POPL } : 4;
    # Other instructions don't need ALU
];
```
On prend `%ecx` comme entrée `A` pour l'UAL, et on prendre `-1` comme entrée B. De cette manière, l'UAL va décrémenter de 1 la valeur de `%ecx` pour faire un simili de boucle `for`.
```python
int aluB = [
    (icode == MMMOVL) && ((ifun == 0) || ifun == 1) : valB; # RMOVL & MRMOVL
    icode in {OPL, IOPL, CALL, PUSHL, RET, POPL, MMMOVL } : valB;
    icode in { RRMOVL, IRMOVL, JREG } : 0;
    icode == LOOP: -1;
    # Other instructions don't need ALU
];
```

On doit ensuite sauvegarder la nouvelle valeur de `%ecx`, étant donné que ce dernier sert de compteur, on sauvegarde la valeur sortant de l'UAL dans `%ecx`.
```python
int dstE = [
    icode in { RRMOVL, IRMOVL, OPL, IOPL } : rB;
    icode in { PUSHL, POPL, CALL, RET } : RESP;
    icode == LOOP: RECX;
    1 : RNONE;  # Don't need register for writing
];
```
Ensuite, `loop` provoque un branchement alors on ajout dans la condition de branchement `is_bch`:
```python
bool is_bch = icode in { JXX, JREG, LOOP } ;
```

On ajoute enfin, une condition supplémentaire à la mise à jour du `PC`:
```
    icode == LOOP && valE > 0 : valC;
    # Taken branch:  Use immediate value
```
Si l'instruction est une boucle dont le compteur est une valeur strictement positive, alors on branche à l'adresse indiqué, ici, `valC`.

### Exercice 4
#### Question 1
On ajoute 2 nouvelles instructions:
- `loope icode=13 ifun =3`
- `loopne icode=13 ifun = 4`
On se base sur les `ifun` des fonctions `je` et `jne` pour les répertorier sur `loope` et `loopne`.
On utilise le même `icode` que la fonction `loop`.

#### Question 2
Ici par besoin de mettre à jour les constantes du HCL, on re-utilisera `LOOP` en précisant l'ifun au besoin.

Il suffit très simplement ensuite d'ajouter la condition `Bch` qui va automatiquement verifier la flags et mettre à jour en fonction, en se basant sur le `ifun` de la fonction (tout comme les jumps)
```python
    # Loop: check if the index is 0
    icode == LOOP && valE > 0 && Bch : valC; 
```

### Question 3
On va créer une fonction strncpy qui va dans un premier temps charger les adresses source et destination.
```python
.pos 0
main:
        irmovl SRC, %esi # esi -> adr source
        irmovl DEST, %edi #edi -> adr destination
```
Avant de les placer dans des registres dans l'étiquette copy. On charge aussi n, le nombre de "mots" à copier.
```python
copy:
        mrmovl (%esi), %eax # on place la source dans eax pour effectuer la copie
        rmmovl %eax, (%edi) # copie dans la destination
        mrmovl n, %ecx # nombre de mots restants à copier
```
Puis on en vient au premier test pour savoir si tout a été copié :
```python
        # si eax est vide c'est que le mot a été copié
        andl %eax, %eax # flags mis à jour
        je fin # on quitte la boucle de copie avant que le nb max de mots soit atteint
```        
On avance dans le tableau et on met le compteur à jour pour possiblement sortir ensuite de la boucle :
```python
        iaddl 4, %esi # on avance de 4 bits
        iaddl 4, %edi
        
        isubl 1, %ecx # flags mis à jour et on soustrait 1 pour le caractère ayant été copié
        je fin # on quitte si le nb max de mots à copier a été atteint
```
Enfin on met les flags à jour pour boucler avec loopne.
```python
        andl %eax, %eax # flags mis à jour
        loopne copy # on boucle sur copie si le nombre de mots restants > 0 et si le mot n'a pas été entièrement copié
```
Etiquette de fin qui recharge dans la destination et le nombre de mots restants.
```python
fin:
    rmmovl %eax, DEST
    rmmovl %ecx, n
    halt
``` 