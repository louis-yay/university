```ocaml

(* Ne pas supprimer ni modifier la ligne suivante *)
open Type

(* follow_path *)

(* Ne pas supprimer ni modifier la ligne suivante *)
open Type

(* follow_path *)
let rec follow_path t l = match t, l with
| Empty, _ -> invalid_arg "empty tree"
| Node(v,_,_), [] -> v
| Node(v,g,d), head::tail when head = L -> (follow_path g tail)
| Node(v,g,d), head::tail when head = R -> (follow_path d tail)
| Node(_,_,_), head::tail -> invalid_arg "not a direction"

```


```ocaml
(*
    Écrire une fonction path_to_key qui prend en entrée un arbre binaire de recherche et une clé et renvoie une
    liste de directions indiquant le chemin jusqu’au nœud de l’arbre contenant cette clé
*)

(* Ne pas supprimer ni modifier la ligne suivante *)
open Type

(* Remplacer la fonction suivante sans changer son nom *)
let rec path_to_key t x = match t with
| Empty -> []
| Node(v, g, d) ->  if x = v then []
    else if x<v then [L]@(path_to_key g x)
    else [R]@(path_to_key d x);;


```


```ocaml
let rec btree_is_quasi_perfect t = 
    let rec aux t = 
    (*aux t renvoie: (is_quasiperfect t, isperfect t, height t*)
        match t with
            | Empty -> (true, true, -1)
            | Node(_, l, r) -> let (qpl, pl, hl) = aux l in
                               let (qpr, pr, hr) = aux r in
            (qpl && pr &&  hl= (hr + 1) || pl && qpr && hl = hr, pl && pr && hr = hl, 1+(max hr hl))
    in
    let (qp, p, h) = (aux t) in qp;;

```

```ocaml
let rec pow2 n =
    if n = 0 then 1
    else (let x = (pow2 n/2) in)
        if n mod 2 = 0 then x*x else 2*x*x);;
```