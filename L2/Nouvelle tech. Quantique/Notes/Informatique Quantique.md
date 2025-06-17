# Portes logiques
## Portes logiques classique

### Not, And, Xor, Cnot
| Entrée | Sortie | Cnot |
| ------ | ------ | ---- |
| 00     | 00     |      |
| 01     | 01     |      |
| 10     | 11     |      |
| 11     | 10     |      |
Si le premier bit est a 1, on inverse le second, sinon, on ne fait rien.

## Portes logiques quantiques
Les portes logiques quantiques utilisent des qubits

### Portes à 1 Qubit
#### Portes X, Y, Z ou portes de Pauli
**Porte X / Not**
- $|0>$ ---> $|1>$  et $|1>$ ---> $|0$  
- Pour un qubit quelconque: $|\Psi> = a|0> + b|1>$  ---> $X|\Psi> = b|0> + a|1>$ 

**Porte Y**
- $|0>$ ---> $i|1>$
- |1> ---> $-i|0>$

**Porte Z**
- $|0>$ ---> $|0>$
- $|1>$ ---> $-|1>$

#### Porte H de Hadamard
- $|0>$ ---> $\frac{1}{\sqrt{2}}(|0> + |1>)$ et $|0>$ ---> $\frac{1}{\sqrt{2}}(|0> + |1>)$ 
- Pour un qubit qlcq: $|\Psi> = a|0> + b|1>$ ---> $H|\Psi> = \frac{a}{\sqrt{2}}(|0> + |1>) + \frac{b}{\sqrt{2}}(|0> - |1>)$   
	- Et par linéarité: $\frac{a+b}{\sqrt{2}}|0> + \frac{a-b}{\sqrt{2}}|1>$ 

### Portes à 2 qubits
#### Porte CNot et Swap
La porte CNot fonctionne comme en logique classique
La porte Swap inverse les états de deux quibits
![[Tableau de bord étudiant/L2/Nouvelle tech. Quantique/Annexe/Annexe_1.png]]

### Porte de toffoli (ou CCNot) à 3 qubits
La porte prend en paramètre 3 qubits, et inverse uniquement l'état du troisième si les deux premiers qubits sont évalué à |1>
En informatique classique on aurait donc:

| Entrée | Sortie |
| ------ | ------ |
| 000    | 000    |
| 001    | 001    |
| 010    | 010    |
| 011    | 011    |
| 100    | 100    |
| 101    | 101    |
| 110    | 111    |
| 111    | 110    |
C'est une porte CNot avec une double vérification.