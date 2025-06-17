## Exercice 1
### Q1
```
prog:   .pos 0
    irmovl 43, %eax
    irmovl 26, %ecx
    addl %eax, %edx
    addl %ecx, %edx
    halt
```

### Q2
le comportement diffère car le `and` compare les bits sans faire de retenue.

### Q3
De même pour `xorl`, la comparaison bit par bit renvoie **49**.

## Exercice 2
### Q1
```
#SF
prog:   .pos 0
        isubl 1, %eax
        halt
```

```
#ZF
prog:   .pos 0
        irmovl 0xFFFFFFFF, %eax
        iaddl 1, %eax
        halt
```

```
#OF
prog:   .pos 0
        irmovl 0x80000000, %eax
        isubl 1, %eax
        halt
```

### Q2
```
#SF
prog:   .pos 0
        irmovl 1, %eax
        isubl 2, %eax
        halt
```

```
#ZF
prog:   .pos 0
        irmovl 1, %eax
        isubl 1, %eax
        halt
```

```
#OF
prog:   .pos 0
        irmovl 1, %eax
        isubl 0x80000000, %eax
        halt
```

### Q3
Effectuer un `xorl` d'un registre sur lui même remet sa valeur à 0, activant donc un `ZeroFlag`.
```
prog:   .pos 0
        irmovl 1234,%eax
        xorl %eax, %eax
        halt
```

### Q4
Faire un `andl` d'un registre sur lui même renvoie le registre initial
```
prog:   .pos 0
        irmovl 1234,%eax
        andl %eax, %eax
        halt
```

> [!question]
> Quel est l’intérêt d'un `xorl` ou d'un `andl` d'un registre sur lui même

## Exercice 3
```
prog:   .pos 0
        mrmovl a, %eax
        mrmovl b,%ecx
        mrmovl c, %edx
        subl %eax, %ecx
        jle un
        halt
un:     irmovl 1, %edx
        halt
        
stack:  .pos 100
        .align 4
a:      .long 2
b:      .long 3
c:      .long 0
```

```
prog:   .pos 0
        mrmovl a, %eax
        mrmovl b,%ecx
        mrmovl c, %edx
        subl %eax, %ecx
        jle un
        irmovl 2, %edx # else
        halt
un:     irmovl 1, %edx
        halt
        
stack:  .pos 100
        .align 4
a:      .long 2
b:      .long 3
c:      .long 0

```


### Bonus
Simple calculatrice, écrit A+B ou A-B dans %eax
```
prog:   .pos 0
        mrmovl a, %eax
        mrmovl b,%ecx
        mrmovl c, %edx
        ixorl 0, %edx
        je addi
            mrmovl c, %edx
            ixorl 1, %edx
            je sous
    sous:   subl %ecx, %eax
            rmmovl %eax, a
            halt
addi:   addl %ecx, %eax
        rmmovl %eax, a
        halt
        
stack:  .pos 100
        .align 4
a:      .long 2
b:      .long 3
c:      .long 0 # if 0-> a+b, if 1 -> a-b

```