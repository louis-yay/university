# Exercices TD
### Exercice 1
```ocaml
let rec btree_mult2_leaves t =
match t with
| Empty -> t
| Node(v, Empty, Empty) -> Node((2*v), Empty, Empty)
| Node(v,g,d) -> Node(v,btree_mult2_leaves(g), btree_mult2_leaves(d));;
```

### Exercice 2
```ocaml
let rec btree_expand_leaves t =
match t with
| Empty -> t
| Node(v, Empty, Empty) -> Node(v, Node((2*v), Empty, Empty), Node(((2*v)+1), Empty, Empty))
| Node(v,g,d) -> Node(v, (btree_expand_leaves g), (btree_expand_leaves d));;
```

### Exercice 3
```ocaml
let rec btree_map f t =
match t with
| Empty -> t
| Node(v, Empty, Empty) -> Node((f v), Empty, Empty)
| Node(v,g,d) -> Node(v, (btree_map f g), (btree_map f d));;
```

# Exercices TM
### Exercice 1
```ocaml
let btree_perfect h =
let rec aux n h =
	if h = -1 then Empty
	else Node(n, (aux (2*n) (h-1)), (aux ((2*n)+1) (h-1))) in
(aux 1 h);;
```

#### Insertion d'élément
```ocaml
let rec bst_insert t c = 
match t with
| Empty -> Node(c, Empty, Empty)
| Node(v,g,d) -> 
	if v>c then Node(v, (bst_insert g c), d)
	else if v<c then Node(v, g, (bst_insert d c))
	else t;;
	
```

```ocaml
let rec bst_from_list l = 
match l with 
| [] -> Empty
| head::tail -> (bst_insert head (bst_from_list tail))
```