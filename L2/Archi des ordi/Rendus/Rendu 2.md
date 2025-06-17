```
        .pos 0              # Début du programme
init:   irmovl stack,%esp   # Initialisation de la pile  
        mrmovl a, %eax      # initialisation variable a, divisé
        mrmovl b, %ebx      # initialisation variable b, diviseur
        pushl %ebx          # Paramètre a de la fonction (divisé)
        pushl %eax          # Paramettre b de la fonction (diviseur)
        call div            # Appel de la fonction divisé
        rmmovl %ecx, q      # Sauvegarde du quotien
        rmmovl %eax, r      # Sauvegarde du reste
        iaddl 8, %esp       # On efface les valeures a et b empilé
        halt                # Arrête le programme
        
        
# On calcule dans un premier temps m et c
div:        
        mrmovl 4(%esp), %eax    # récupéraition de la variable a de la fonction (divisé)
        mrmovl 4(%esp), %ebx    # récupéraition de la variable b de la fonction (diviseur)
        pushl %edi              # Sauvegarde des registre du caller
        pushl %ebx              # Sauvegarde des registre du caller
        
        irmovl 1, %edx          # initialisation de la variable c, accumulateur puissances de 2
        xorl %edi, %edi         # initialisation d'un registre temporaire
boucle: rrmovl %eax, %edi       # sauvegarde de a pour les calculs suivants
        subl %ebx , %edi        # si a < b*c
        jl end                  # alors on a trouvé m, on passe a la suite
        addl %ebx, %ebx         # sinon b*2
        addl %edx, %edx         # et c*2
        jmp boucle              # On poursuit la boucle jusqu'a 2^(n+1)
        
# On a n, la + grande puissance de 2 tel que (2^n)*b < a
end:    isarl 1, %edx           # On réduit la variable c (accumulateur) à 2^n en divisant par 2
        isarl 1, %ebx           # De même pour la variable m.
        xorl %ecx, %ecx         # Initialisation de la variable q (quotien)
div_bcl:rrmovl %eax, %edi       # sauvegarde de a pour les calculs suivants
        subl %ebx, %edi         # si (a < m) on effectue pas la soustraction
        jl else                 # Sino
        rrmovl %edi, %eax       # a = a - (2^n)*b
        addl %edx, %ecx         # On additionne c au quotien
else:   isubl 0, %edx           # On vérifie si c = 0
        je end_f                # Si c'est le cas, on a terminé, 
        isarl 1, %edx           # On divise (2^n)*b / 2 = (2^(n-1))*b
        isarl 1, %ebx           # On divise 2^n par 2 = 2^(n-1)
        jmp div_bcl             # On boucle
        
end_f:  popl %ebx               # Retablissement des registres du caller
        popl %edi               # Retablissement des registres du caller
        ret                     # on retourne r, ici dans %eax et q dans %ecx 
        
        
        
# NOTE: Attention à caller & callee
    

        .pos 0x100          # Décalage pour les entrées mémoires
a:      .long 359           # Variable a, d'entrée
b:      .long 17            # Variable b, d'entrée
q:      .long 0             # Variable Quotien, de sortie
r:      .long 0             # Variable Reste, de sortie

        .pos 0x200
stack:
```

Louis DENYS
22304612
