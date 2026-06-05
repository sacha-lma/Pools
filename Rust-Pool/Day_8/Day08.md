# DAY 08 — TRAITS, CLOSURES & ITERATEURS

> *Le jour « Sorting » de la piscine C*, qui utilisait des **pointeurs de
> fonction** (`int (*cmp)(...)`). En Rust, l'équivalent — en plus puissant — ce
> sont les **closures** et les bornes de traits `Fn`/`FnMut`, et toute la chaîne
> des **itérateurs**.

## Préliminaires

- **Langage** : Rust. `unsafe` interdit.
- Une closure se passe comme n'importe quelle valeur. Pour l'accepter en
  paramètre, on borne par un trait : `F: Fn(&T) -> bool`, `F: FnMut(...)`, etc.
- N'appelle pas les méthodes de tri/itération de la std *à la place* de ce qui
  est demandé : ici on apprend à les **réimplémenter** puis on compare.

## Task 01 — apply

Applique une fonction à chaque élément d'une slice mutable, sur place.

```rust
pub fn apply<F: FnMut(&mut i32)>(arr: &mut [i32], f: F)
```

Exemple : `apply(&mut v, |x| *x *= 2)` double chaque élément.

> `FnMut` car la closure peut capturer un état mutable. C'est l'équivalent
> typé-sûr du pointeur de fonction du C, sans cast ni `void *`.

## Task 02 — my_map

Renvoie un nouveau `Vec` en appliquant `f` à chaque élément (sans modifier
l'entrée).

```rust
pub fn my_map<T, U, F: Fn(&T) -> U>(input: &[T], f: F) -> Vec<U>
```

> Réimplémente `Iterator::map` + `collect`. Puis fais la version d'une ligne avec
> `input.iter().map(f).collect()` et compare.

## Task 03 — my_filter

Renvoie un `Vec` ne contenant que les éléments satisfaisant le prédicat. `T`
doit être clonable (`T: Clone`).

```rust
pub fn my_filter<T: Clone, F: Fn(&T) -> bool>(input: &[T], pred: F) -> Vec<T>
```

## Task 04 — my_fold

Replie une slice en une valeur unique en partant d'un accumulateur.

```rust
pub fn my_fold<T, A, F: Fn(A, &T) -> A>(input: &[T], init: A, f: F) -> A
```

Exemple : `my_fold(&v, 0i64, |acc, x| acc + *x as i64)` = somme.

## Task 05 — my_advanced_sort

Trie une slice selon un comparateur fourni par closure (renvoie un `Ordering`).
C'est la transposition directe de `my_advanced_sort_word_array`.

```rust
use std::cmp::Ordering;

pub fn my_advanced_sort<T, F: Fn(&T, &T) -> Ordering>(arr: &mut [T], cmp: F)
```

Exemple : trier des mots par longueur :
`my_advanced_sort(&mut v, |a, b| a.len().cmp(&b.len()))`.

> En C, `cmp` était un `void *`-friendly pointeur de fonction. En Rust, la
> closure capture éventuellement son environnement *et* reste typée.

## Task 06 — Trait Describe

Définis un trait et implémente-le pour deux types.

```rust
pub trait Describe {
    fn describe(&self) -> String;
}
```

Implémente `Describe` pour `i32` (renvoie `"entier: N"`) et pour `String`
(renvoie `"texte de N caractères"`). Écris ensuite une fonction générique qui
accepte n'importe quoi d'implémentant le trait :

```rust
pub fn announce<T: Describe>(item: &T) -> String
```

> C'est le polymorphisme de Rust. En C, tu simulais ça avec des `void *` et des
> pointeurs de fonction ; ici, c'est vérifié à la compilation.

## Task 07 — count_if

Compte les éléments satisfaisant un prédicat, en chaînant des itérateurs (une
seule expression, pas de boucle `for`).

```rust
pub fn count_if<T, F: Fn(&T) -> bool>(input: &[T], pred: F) -> usize
```

## Task 08 — pipeline (bonus)

Sur un `&[i32]`, calcule en **une seule chaîne d'itérateurs** la somme des
carrés des nombres pairs.

```rust
pub fn sum_even_squares(input: &[i32]) -> i64
```

> Objectif : `input.iter().filter(...).map(...).sum()`. Goûte à la lisibilité du
> style fonctionnel de Rust.
