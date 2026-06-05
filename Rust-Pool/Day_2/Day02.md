# DAY 02 — OWNERSHIP & BORROWING

> *Le jour « Pointers » de la piscine C.* En Rust, on ne manipule pas d'adresses
> à la main : on **emprunte** (`&`, `&mut`) et on **déplace** (move). Ce jour
> remplace l'arithmétique de pointeurs par le système d'ownership.

## Préliminaires

- **Langage** : Rust. `unsafe` interdit.
- Macros d'affichage autorisées à partir d'aujourd'hui (`println!`) pour les
  tests, mais tes fonctions doivent respecter les signatures imposées.
- Concept clé : une valeur a un unique *propriétaire*. La passer par valeur la
  *déplace* ; pour la prêter, on utilise une référence.

## Task 01 — swap

Échange le contenu de deux entiers. En C tu prenais `int *a, int *b`. Ici, tu
empruntes en mutable.

```rust
pub fn swap(a: &mut i32, b: &mut i32)
```

> La std fournit `std::mem::swap` : réimplémente-la toi-même sans l'appeler.

## Task 02 — my_strlen

Renvoie le nombre de *caractères* d'une chaîne empruntée.

```rust
pub fn my_strlen(s: &str) -> usize
```

> `&str` est une *vue empruntée* sur une chaîne : ni copie, ni propriété
> transférée. C'est l'équivalent sûr d'un `char const *` + longueur.

## Task 03 — my_swap_first_last

Dans une slice d'entiers, échange le premier et le dernier élément, sur place.

```rust
pub fn my_swap_first_last(arr: &mut [i32])
```

> Une slice `&mut [i32]` donne un accès mutable emprunté à une zone contiguë,
> sans en être propriétaire. C'est l'équivalent sûr de `(int *array, int size)`.

## Task 04 — my_rev_array

Inverse une slice d'entiers sur place.

```rust
pub fn my_rev_array(arr: &mut [i32])
```

## Task 05 — sum

Renvoie la somme des éléments d'une slice, *sans* prendre possession de la
donnée (lecture seule).

```rust
pub fn sum(arr: &[i32]) -> i64
```

> Pourquoi `&[i32]` et non `Vec<i32>` ? Prendre une slice empruntée permet
> d'accepter aussi bien un tableau, un `Vec`, qu'une sous-tranche. C'est l'API
> idiomatique. Renvoyer `i64` évite l'overflow.

## Task 06 — my_sort_int_array

Trie une slice d'entiers en ordre croissant, sur place (tri de ton choix, ne
pas appeler `.sort()`).

```rust
pub fn my_sort_int_array(arr: &mut [i32])
```

## Task 07 — give_ownership

Cette tâche illustre le *move*. Écris une fonction qui **prend possession**
d'une `String`, la modifie en ajoutant `!`, et la **rend** (transfert de
propriété en sortie).

```rust
pub fn shout(mut s: String) -> String
```

Dans un test, montre qu'après l'appel `let s2 = shout(s);`, utiliser `s`
provoque une erreur de compilation (la valeur a été déplacée). Commente cette
ligne et explique pourquoi dans un commentaire.

## Task 08 — borrow_vs_move

Écris deux fonctions :

```rust
pub fn count_borrow(s: &String) -> usize   // emprunte, n'altère pas la propriété
pub fn into_len(s: String) -> usize        // consomme la String
```

Dans un test, appelle `count_borrow(&s)` plusieurs fois (autorisé : emprunts
multiples en lecture), puis `into_len(s)` une seule fois. Explique en
commentaire la règle : *autant d'emprunts immuables qu'on veut, OU un seul
emprunt mutable, jamais les deux en même temps*.
