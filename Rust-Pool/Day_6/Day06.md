# DAY 06 — MODULES, CRATES & LE TAS

> *Le jour « Compilation, Allocation » de la piscine C* — celui du `malloc`.
> **C'est précisément ici qu'on change de monde.** En Rust, on n'appelle jamais
> `malloc`/`free` : on alloue sur le tas via `Box`, `Vec`, `String`, et la
> libération est automatique quand la valeur sort de portée (RAII). Ce jour
> remplace l'allocation manuelle par les types possédés et introduit le système
> de **modules** et de **crates** (l'équivalent Rust de ta `libmy`).

## Préliminaires

- **Langage** : Rust. `unsafe` interdit (donc aucun `std::alloc` manuel).
- Organise ton code en modules. Ta « libmy » devient une **crate bibliothèque**.
- Rappel mémoire : `Box<T>` met *une* valeur sur le tas ; `Vec<T>` une séquence
  redimensionnable ; `String` du texte. Aucun `free` à écrire.

### Mini-cours : malloc → Rust

| En C | En Rust |
|---|---|
| `int *p = malloc(sizeof(int));` | `let p = Box::new(0i32);` |
| `int *t = malloc(n * sizeof(int));` | `let t = vec![0i32; n];` |
| `t = realloc(t, ...)` | `v.push(x)` (le `Vec` grandit seul) |
| `free(p);` | rien : libéré à la fin de portée |
| double `free` / fuite | **impossible** : garanti par le compilateur |

## Task 01 — boxed_int

Alloue un entier sur le tas et renvoie la `Box`. Puis écris une fonction qui en
lit la valeur.

```rust
pub fn boxed_int(value: i32) -> Box<i32>
pub fn unbox(b: Box<i32>) -> i32
```

> `*b` déréférence la `Box` comme `*p` déréférençait ton pointeur — mais sans
> risque de pointeur nul ni de fuite.

## Task 02 — my_range

Renvoie un `Vec<i32>` contenant les entiers de `start` (inclus) à `end` (exclu).
Si `start >= end`, renvoie un `Vec` vide.

```rust
pub fn my_range(start: i32, end: i32) -> Vec<i32>
```

> Équivalent du `malloc` d'un tableau d'entiers en C, mais le `Vec` connaît sa
> taille et se libère seul.

## Task 03 — my_grow

Construis un `Vec` en partant de vide et en faisant `push` `n` fois la valeur
`v`. Réserve la capacité d'avance avec `Vec::with_capacity` pour éviter les
réallocations.

```rust
pub fn my_grow(v: i32, n: usize) -> Vec<i32>
```

> `with_capacity` est le pendant idiomatique du dimensionnement explicite que tu
> ferais avec `malloc`. Observe la différence entre *longueur* (`.len()`) et
> *capacité* (`.capacity()`).

## Task 04 — Structurer ta crate

Crée la disposition suivante dans ta crate bibliothèque :

```
src/
├── lib.rs          // pub mod text; pub mod num;
├── text.rs         // fonctions sur les chaînes (réutilise le jour 06)
└── num.rs          // fonctions numériques (réutilise le jour 05)
```

Dans `lib.rs` :

```rust
pub mod text;
pub mod num;
```

Expose au moins une fonction par module (`pub fn`) et appelle-les depuis un test
via `use ta_crate::text::...`. **Objectif : comprendre les chemins de modules,
la visibilité `pub`, et l'arborescence d'une crate.**

## Task 05 — my_print_combn

Affiche, en ordre croissant, tous les nombres composés de `n` chiffres
*différents* et croissants (le plus petit nombre seulement). `my_print_combn(3)`
donne le même résultat que `my_print_comb` du jour 03.

```rust
pub fn my_print_combn(n: u32)
```

## Task 06 — flatten

Reçoit un `Vec<Vec<i32>>` et renvoie un unique `Vec<i32>` concaténant tout, dans
l'ordre.

```rust
pub fn flatten(v: Vec<Vec<i32>>) -> Vec<i32>
```

> Prendre `v` par valeur la *consomme* : tu peux donc déplacer les sous-vecteurs
> dedans sans copie. Compare avec une version qui prend `&[Vec<i32>]` et doit
> cloner.

## Task 07 — recursive_box (bonus)

Définis un type récursif simple en utilisant `Box` (sans `Box`, le compilateur
refuse car la taille serait infinie) :

```rust
pub enum Chain {
    Link(i32, Box<Chain>),
    End,
}

pub fn chain_len(c: &Chain) -> usize
```

> Pourquoi `Box` est obligatoire ici ? Parce qu'une valeur ne peut pas se
> contenir elle-même : l'indirection sur le tas casse la récursion de taille.
> C'est l'avant-goût du jour 11 (listes & arbres).
