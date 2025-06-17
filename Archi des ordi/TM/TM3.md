## Exercice 1
### Question 1
`a b - b c - c d - & +`

### Question 2
```
#   a b - b c - c d - & +
        .pos 0
        # a - b
init:   irmovl stack, %esp
        mrmovl a, %eax
        mrmovl b, %ecx
        subl %eax, %ecx
        pushl %ecx # a-b
        
        mrmovl b, %eax
        mrmovl c, %ecx
        subl %eax, %ecx
        pushl %ecx #(b-c)
        
        mrmovl c, %eax
        mrmovl d, %ecx
        subl %eax, %ecx # (c-d) -> %ecx
        
        popl %eax
        andl %eax, %ecx # (b-c)&(c-d) -> %ecx
        popl %eax # a-b
        addl %eax, %ecx # (a-b) + ((b-c)&(c-d)) -> %ecx
        
        
        halt

        .pos 0x200
stack:
a:      .long 1
b:      .long 2
c:      .long 3
d:      .long 4

```

## Exercice 2
```
        .pos 0
init:   irmovl stack, %esp
        irmovl 7, %eax
        pushl %eax
        irmovl 5, %eax
        pushl %eax
        call sub
        iaddl 8, %esp
        halt
        
sub:    mrmovl 4(%esp), %ecx # on lit i
        mrmovl 8(%esp), %eax # on lit j
        subl %ecx, %eax # i-j -> %eax
        ret


        .pos 0x200
stack:
```

## Exercice 4
```
        .pos 0
init:   irmovl stack, %esp
        irmovl 1234, %ebx
        pushl %ebx  # saving %ebx -> stack
        irmovl 7, %eax
        pushl %eax
        irmovl 5, %eax
        pushl %eax
        call sub
        iaddl 8, %esp
        popl %ebx  
        halt
        
sub:    mrmovl 4(%esp), %ebx # on lit i
        mrmovl 8(%esp), %eax # on lit j
        subl %ebx, %eax # i-j -> %eax
        ret


        .pos 0x200
stack:
```

## Exercice 5
Ici on utilise %eax et %ebx dans le prog principale, on va donc sauvegarder les valeurs avant d'effectuer des opérations, en respectant les conventions de responsabilités
```
        .pos 0
init:   irmovl stack, %esp
        irmovl 1234, %ebx
        irmovl 567, %eax
        pushl %eax # saving %eax -> stack
        irmovl 7, %eax
        pushl %eax
        irmovl 5, %eax
        pushl %eax
        call sub
        iaddl 8, %esp
        popl %eax  
        halt
        
sub:    pushl %ebx  # saving %ebx -> stack
        mrmovl 4(%esp), %ebx # on lit i
        mrmovl 8(%esp), %eax # on lit j
        subl %ebx, %eax # i-j -> %eax
        rmmovl %eax, result
        popl %ebx
        ret


        .pos 0x100
result: .long 0

        .pos 0x200
stack:

```

```
        .pos 0
init:   irmovl stack, %esp
        irmovl 3, %eax
        pushl %eax
        call g
        halt
        
sub:    mrmovl 4(%esp), %ecx # on lit i
        mrmovl 8(%esp), %eax # on lit j
        subl %ecx, %eax # i-j -> %eax
        ret
        
g:   mrmovl 4(%esp), %eax
     irmovl 7, %ecx
     pushl %ecx # 7 -> stack
     pushl %eax # i -> stack
     call sub # i-7 -> %eax
     rrmovl %eax, %edx # i-7 -> %edx
     iaddl 8, %esp 
     mrmovl 4(%esp), %eax 
     irmovl 1, %ecx
     pushl %ecx # 1 -> stack
     pushl %eax # i -> stack

     


        .pos 0x200
stack:  
```