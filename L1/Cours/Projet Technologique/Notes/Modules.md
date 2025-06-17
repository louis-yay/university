---
Reference:
  - "[[03-cours-module.pdf]]"
Date: 2024-09-17
Reviewed: true
---
## Principe

En général, un module est définie de la manière suivant:

- `<name>` le nom du module
- `<name>.h` le fichier header avec déclaration des fonctions
- `<name>.c` le fichier de définition des fonctions déclaré dans le header, ainsi que les fonctions auxiliaires privé à se module.

  

## Distribution

Lorsqu’on veut distribuer un module sous la forme d’un bibliothèque réutilisable

- `<name>.a` le fichier de la bibliothèque en binaire
- `<name>.h` le fichier qui contient les déclarations des fonctions
    - `\#include "<name>.h"`