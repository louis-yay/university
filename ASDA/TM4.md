### Exercice 1
#### Q2
```ocaml
let rec bst_max t =
match t with 
| Empty -> failwith "Empty tree"
| Node(v,_,Empty) -> v
| Node(v,_,d) -> (bst_max d);;
```

### Exercice 2
#### Q3
```ocaml
let rec bst_pop_max t =
match t with
| Empty -> failwith "Empty tree"
| Node(v, g, Empty) -> (g,v)
| Node(v,g,d) -> let tree, max = (bst_pop_max d) in (Node(v, g, tree), max);;
```

#### Q4
```ocaml
	let rec bst_remove t c =
	match t with
	| Empty -> t
	| Node(v,g,d) -> 
		if v = c then 
			if g = Empty then d
			else let treel, maxl = (bst_pop_max g) in
				Node(maxl, treel, d)
		else if c < v then Node(v, (bst_remove g c), d)
		else Node(v, g, (bst_remove d c))
```

`| Node(v, g, d) when v = c -> let (Node(nv, ng, nd), m) = (bst_pop_max t) in Node(m, ng, nd)` permet de décomposer le résultat de `bst_pop_max`

### Exercice 3
#### Q3

```ocaml
let rec is_bst t =
	let rec aux t = 
		match t with
			| Empty -> failwith "Empty tree "
			| Node(v, Empty, Empty) -> true, x, x
			| Node(v,Empty,d) -> let bst_d, min_d, max_d = (aux d) in (v < min_d && bst_d), v, max_d
			| Node(v, g, Empty) ->  let bst_g, min_g, max_g = (aux g) in (v < max_g && bst_g), min_g, v
			| Node(v, g, d) -> let bst_g, min_g, max_g = (aux g) and bst_d, min_d, max_d = (aux d) in bst_g && bst_d && max_g < v && min_d > v, min_g, max_d
	in
	t = Empty || (let bst,min,max = (aux t) in bst);;
```


### Exercice 4
#todo ex 4 
```ocaml
let rec bst_range_search t min max =
match t with
| Empty -> []
| Node(v, g, d) -> if v < min then (bst_range_search d min max)
	else if v > max then (bst_range_search g min max)
	else (bst_range_search g min max)@[v]@(bst_range_search d min max)

```
