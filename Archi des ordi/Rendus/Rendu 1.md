```y86
        .pos 0              # Début du programme
        mrmovl a, %eax      # initialisation variable a
        mrmovl b, %ebx      # initialisation variable b
        irmovl 1, %edx      # initialisation accumulateur puissance de 2 
        xorl %edi, %edi     # initialisation registre temporaire
        
boucle: rrmovl %eax, %edi   # sauvegarde de a pour les calculs suivants
        subl %ebx , %edi    # si a < b*c
        jl end              # alors on termine 
        addl %ebx, %ebx     # sinon b*2
        addl %edx, %edx     # et c*2
        jmp boucle          # On poursuit la boucle jusqu'a 2^(n+1)

end:    isarl 1, %edx       # On réduit l'accumulateur à 2^n en divisant par 2
        rmmovl  %edx, c     # On renvoie c, la puissance de 2 correspondante
        halt                # Arrête le programme

        .pos 0x100          # Décalage pour les entrées mémoires
a:      .long 72            # Variable a, d'entrée
b:      .long 9             # Variable b, d'entrée
c:      .long 0             # Variable c, de sortie
```

Louis DENYS
22304612
