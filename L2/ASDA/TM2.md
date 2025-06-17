```ocaml
type 'a btree =
| Empty
| Node of 'a * 'a btree * 'a btree;;

let t = Node(7, Node(2, Node(5,Empty,Empty), Empty),
Node(1, Node(5,Empty,Empty), Node(3,Empty,Empty)));;

  
let btree_root t =
match t with 
| Empty -> failwith "l'arbre est vide"
| Node(x,l,r) -> x

  

let rec btree_mem = fun a t ->
match t with
| Empty -> false
| Node(v,g,d) -> if v = a then true else (btree_mem a g) || (btree_mem a d);;
  

let rec btree_nb_occ = fun a t ->
match t with
| Empty -> 0
| Node(v,g,d) -> if v = a then 1 + (btree_nb_occ a g) + (btree_nb_occ a d) else (btree_nb_occ a g) + (btree_nb_occ a d);;

  
let rec hauteur = fun t ->
match t with
| Empty -> -1
| Node(_,g,d) -> 1 + (max (hauteur g) (hauteur d));;
  

let rec taille = fun t ->
match t with
| Empty -> 0
| Node(_,g,d) -> 1 + (taille g) + (taille d);;

  
let rec nb_internal_node = fun t ->
match t with
| Empty -> 0
| Node(_,g,d) -> if g != Empty || d != Empty then 1 + (nb_internal_node g) + (nb_internal_node d) else (nb_internal_node g) + (nb_internal_node d);;


let rec nb_lead = fun t ->
match t with
| Empty -> 0
| Node(_, g, d) -> if g == Empty && d == Empty then 1 else (nb_internal_node g) + (nb_internal_node d);;
  
let arity = fun t ->
match t with
| Empty -> -1
| Node(_,g,d) when g = Empty && d = Empty -> 0
| Node(_,g,d) when g = Empty && d != Empty -> 1
| Node(_, g,d) when d = Empty && g != Empty -> 1
| Node(_,g,d) when d != Empty && g != Empty -> 2;;
  
  
let rec at_depth = fun t a ->
match t with
| Empty -> []
| Node(v, g, d) -> if a = 0 then [v] else (at_depth g (a-1)) @ (at_depth d (a-1));;
```