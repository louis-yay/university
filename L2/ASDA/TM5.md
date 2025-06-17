### Exercice 3
#### Q1
```ocaml
let get_height t = 
match t with
	| Empty -> -1
	| Node((_,h),_,_) -> h;;
```

#### Q2
```ocaml
let tag_node c t1 t2 = Node((c, (max (get_height t1) (get_height t2)) +1 ), t1, t2);;
```

#### Q3
```ocaml
let rec tag_btree t =
match t with
| Empty -> Empty
| Node(v, Empty, Empty) -> Node((v,0), Empty, Empty)
| Node(v, g, d) -> (tag_node v (tag_btree g) (tag_btree d))
```


### Excerice 4
#### Q1
```ocaml
let rec is_avl t = 
match t with 
| Empty -> true
| Node(v, Empty, Empty) -> true
| Node(v, g, d) ->  -1 <= (get_height g) - (get_height d)  && (get_height g) - (get_height d) <= 1 && (is_avl g) && (is_avl d);;
```

### Exercice 7
#### Q1
```ocaml
let l_rotate t =
match t with
| Empty -> failwith "Tree not suitable for left rotation"
| Node(a,t1,Node(c, t2, t3)) -> Node(c,Node(a,t1,t2), t3)
| Node(_,_,_) -> failwith "Tree not suitable for left rotation";;
```
#question comment faire match l'arbre sur sur la condition `Node(a,t1,Node(c, t2, t3))` (connaitre) le cas avec rotation gauche