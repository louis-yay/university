---
Année:
  - "2023"
---
# Basiques:

Pour pouvoir accéder à une machine, il est necessaire de se trouver sur le même réseaux qu’elle. De ce fait, on utilise ici un outil appelé ==**OpenVPN**== qui permet d’utiliser directement un vpn depuis un terminal linux:

```Bash
sudo openvpn {filename}.ovpn
[...]
Initialization Sequence Completed
```

Une fois le vpn connecté, on vérifié la stabilité de la connection avec la cible à l’aide de la commande _==ping==_:

```Bash
ping {target_ip}
```

  

L’étape suivante est de repéré les **ports** de la cibles qui sont ouvert, c’est à dire accessible pour nous. De cette manière, on utilise la commande _==nmap==_ avec le flag _==-sV==_ qui permet de déterminer le nom et la description des services identifiés:

```Bash
nmap -sV {target_ip}
OU
sudo nmap -sV {target_ip}
```

  

On repère ensuite les ports libre d’accès et ceux à quoi ils sont destiné, ainsi on se basant sur les protocoles prévus à cet effet on peu tenter de s’introduire dans la machine.  
  
  
==Exemple d’application:== [==Meow==](https://app.hackthebox.com/starting-point)

---

[[Tableau de bord étudiant/L1/Basics Of Penetration Testing/Notes/Notes]]

  

[[Tableau de bord étudiant/Cours/L1/Basics Of Penetration Testing/Readings/Readings|Readings]]

[[Tableau de bord étudiant/Cours/L1/Basics Of Penetration Testing/Questions|Questions]]

[[Tableau de bord étudiant/Cours/L1/Basics Of Penetration Testing/Syllabus]]

---

  

==↓ Add a database filter so the board only shows to-do’s relevant to this course.==