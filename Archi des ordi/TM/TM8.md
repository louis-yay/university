### Exercice 3
#### Question 1
Pour RMMOVL, PUSHL, CALL, MRMOVL, on précise directement l'adresse dans l'instruction
```
call fun
```
et donc on a directement l'adresse du saut à effectuer, dans `valE`
POPL et RET vont utiliser la valeur stocké dans le registre `%esp`, qui est dans la banque de registre, on utilise donc `valA`

#### Question 2
`bool set_cc = icode in { OPL, IOPL };` 

#### Question 3
```
int srcA = [
    icode in { RRMOVL, RMMOVL, OPL, PUSHL } : rA;
    icode in { POPL, RET } : RESP;
    1 : RNONE;  # Don't need register for reading
];
```
IOPL n'a pas de registre de source, car on utilisera la valeur immediate

```
int aluA = [
    icode in { RRMOVL, OPL } : valA;
    icode in { IRMOVL, RMMOVL, MRMOVL, IOPL } : valC;
```
IOPL va mettre en entrée de l'ALU une valeur immediate contrairement à OPL  


### Exercice 4
#### Question 1
`decl` prend le format suivant: `d11f`

#### Question 2
Lors de l'execution, le processeur renvoie une erreur dans `STAT` et `ERR`.

#### Question 3
On ajoute les lignes suivantes:
```
intsig DECL                     'instructionSet.get("decl").icode'
```
au début et puis:
```
## Is instruction valid?
bool instr_valid =
    icode in { NOP, HALT, RRMOVL, IRMOVL, RMMOVL, MRMOVL,
               OPL, IOPL, JXX, CALL, RET, PUSHL, POPL, DECL } ;

```



```
[...]
intsig DECL                     'instructionSet.get("decl").icode'
[...]

## Fetch stage #####################################################

## Does fetched instruction require a register numbers byte?
bool need_regids =
    icode in { RRMOVL, OPL, IOPL, PUSHL, POPL, IRMOVL, RMMOVL, MRMOVL, DECL };

## Does fetched instruction require a constant word?
bool need_valC =
    icode in { IRMOVL, RMMOVL, MRMOVL, JXX, CALL, IOPL };

## Is instruction valid?
bool instr_valid =
    icode in { NOP, HALT, RRMOVL, IRMOVL, RMMOVL, MRMOVL,
               OPL, IOPL, JXX, CALL, RET, PUSHL, POPL, DECL } ;

## Decode stage ####################################################

## What register should be used as the A source?
int srcA = [
    icode in { RRMOVL, RMMOVL, OPL, PUSHL, DECL } : rA;
    icode in { POPL, RET } : RESP;
    1 : RNONE;  # Don't need register for reading
];

## What register should be used as the B source?
int srcB = [
    icode in { OPL, IOPL, RMMOVL, MRMOVL } : rB;
    icode in { PUSHL, POPL, CALL, RET } : RESP;
    1 : RNONE;  # Don't need register for reading
];

## What register should be used as the E destination?
int dstE = [
    icode in { RRMOVL, IRMOVL, OPL, IOPL } : rB;
    icode in { DECL } : rA;
    icode in { PUSHL, POPL, CALL, RET } : RESP;
    1 : RNONE;  # Don't need register for writing
];

## What register should be used as the M destination?
int dstM = [
    icode in { MRMOVL, POPL } : rA;
    1 : RNONE;  # Don't need register for writing
];

## Execute stage ###################################################

## Select input A to ALU
int aluA = [
    icode in { RRMOVL, OPL, DECL } : valA;
    icode in { IRMOVL, RMMOVL, MRMOVL, IOPL } : valC;
    icode in { CALL, PUSHL } : -4;
    icode in { RET, POPL } : 4;
    # Other instructions don't need ALU
];

## Select input B to ALU
int aluB = [
    icode in { RMMOVL, MRMOVL, OPL, IOPL, CALL, PUSHL, RET, POPL } : valB;
    icode in { DECL } : -1;
    icode in { RRMOVL, IRMOVL } : 0;
    # Other instructions don't need ALU
];

## Set ALU function
int alufun = [
    icode in { OPL, IOPL } : ifun;
    1 : ALUADD;  # Perform additions by default
];

## Should condition codes be updated?
bool set_cc = icode in { OPL, IOPL , DECL};

bool is_bch = icode in { JXX };

## Memory stage ####################################################

## Set read control signal
bool mem_read =
    icode in { MRMOVL, POPL, RET };

## Set write control signal
bool mem_write =
    icode in { RMMOVL, PUSHL, CALL };

## Select memory address
int mem_addr = [
    icode in { RMMOVL, PUSHL, CALL, MRMOVL } : valE;
    icode in { POPL, RET } : valA;
    # Other instructions don't need address
];

## Select memory input data
int mem_data = [
    # Value from register
    icode in { RMMOVL, PUSHL } : valA;
    # Return PC address
    icode == CALL : valP;
    # Default: Don't write anything
];

## Program Counter update ##########################################
[...]

```

### Exercice 5
