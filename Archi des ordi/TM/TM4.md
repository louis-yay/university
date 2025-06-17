```
cat monautreprog.S
		#include "head.S"

		main:
		// Initialiser la pile
			movl $(stack + STACK_SIZE),%esp
			movl %esp,%ebp

		// On affiche deux caracteres
			movl $0xb8000,%ebx // Ecran -> ebx
			movb charA,%al  // lire la variable charA
			movl $10, %ecx 	// Nombre de boucles
boucle:		movb %al,(%ebx) // rmmovl %al -> screen
			subl $1, %ecx
			addl $2,%ebx    // sauter 2 octets: un pour le caractere, l autre pour la couleur
			addb $1,%al     // on incremente
			movl %ecx, %edx
			cmpl $0, %edx
			je fin
			jmp boucle


			// recuperer un caractere tape au clavier
fin:		// call getchar

		// On s arrete
			hlt

			.size main,.-main



		// Juste un exemple de variable
		charA:
			.byte 48

		.comm stack,STACK_SIZE

```

