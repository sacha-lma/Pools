# DAY 05 — VEC, ITÉRATEURS & ARGUMENTS

> *Le jour « Libmy, Arguments » de la piscine C.* On découvre `Vec<T>` (le
> tableau dynamique de Rust, qui remplace les tableaux + `realloc`), les bases
> des itérateurs, et la lecture des arguments du programme.

## Préliminaires

- **Langage** : Rust. `unsafe` interdit.
- `Vec`, `String`, itérateurs : tout est autorisé maintenant — ce sont les
  idiomes à apprendre.
- Les arguments se récupèrent avec `std::env::args()` qui renvoie un itérateur
  de `String` (le premier élément est le nom du programme).

## Task 01 — my_str_to_word_array

Découpe une chaîne en mots (séparateurs = caractères non alphanumériques).
Renvoie un `Vec<String>`.

```rust
pub fn my_str_to_word_array(s: &str) -> Vec<String>
```

> En C tu renvoyais un `char **` terminé par `NULL`, avec des allocations à
> libérer. En Rust, un `Vec<String>` connaît sa taille et se libère seul.

## Task 02 — my_show_word_array

Affiche un mot par ligne (chaque ligne se termine par `\n`).

```rust
pub fn my_show_word_array(words: &[String])
```

> Prends une slice `&[String]` en paramètre : ça accepte aussi bien un `Vec` que
> n'importe quelle sous-tranche, sans copier.

## Task 03 — my_sort_word_array

Trie les mots par ordre ASCII (sur place). N'utilise pas `.sort()` : implémente
le tri en échangeant les éléments du `Vec`.

```rust
pub fn my_sort_word_array(words: &mut Vec<String>)
```

## Task 04 — concat_words

Concatène tous les mots d'une slice en une seule `String`, séparés par `sep`.

```rust
pub fn concat_words(words: &[String], sep: &str) -> String
```

> Compare ta version manuelle avec `words.join(sep)`.

## Task 05 — convert_base

Convertit le nombre représenté par `nbr` (exprimé en base `base_from`) vers la
base `base_to`, et renvoie le résultat en `String`. Le nombre tient dans un
`i64`. Renvoie `None` si une base est invalide.

```rust
pub fn convert_base(nbr: &str, base_from: &str, base_to: &str) -> Option<String>
```

## Task 06 — my_compute_average

Renvoie la moyenne d'une slice de `f64`, ou `None` si elle est vide.

```rust
pub fn my_compute_average(values: &[f64]) -> Option<f64>
```

> Première vraie occasion d'enchaîner des itérateurs :
> `values.iter().sum::<f64>() / values.len() as f64`.

## Task 07 — print_args

Exécutable (`src/main.rs`) : affiche chaque argument de la ligne de commande sur
sa propre ligne, **sans** le nom du programme.

```rust
fn main() {
    // utilise std::env::args().skip(1)
}
```

Exemple :

```
$> cargo run -- test arg2 arg3
test
arg2
arg3
```

## Task 08 — sum_args

Exécutable : additionne tous les arguments interprétés comme des entiers et
affiche le total. Si un argument n'est pas un entier valide, écris un message
sur la sortie d'erreur et termine avec le code 84.

```rust
fn main() {
    // .parse::<i64>() renvoie un Result : sers-t'en pour détecter l'erreur
}
```
