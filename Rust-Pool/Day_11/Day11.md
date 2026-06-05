# DAY 11 — ARBRES & ENUMS RÉCURSIFS

> *Le jour « ED » de la piscine C* (arbres binaires, arbres rouge-noir), bâtis en
> C avec des structures et des pointeurs `void *`. En Rust, l'arbre est un
> **enum récursif** générique + `Box`, et on parcourt avec le **pattern
> matching**. En bonus : un premier contact avec la **concurrence** (threads,
> `Arc`, `Mutex`), spécificité phare de Rust (« fearless concurrency »).

## Préliminaires

- **Langage** : Rust. `unsafe` interdit.
- Structure imposée :

```rust
pub enum BTree<T> {
    Empty,
    Node {
        left: Box<BTree<T>>,
        value: T,
        right: Box<BTree<T>>,
    },
}
```

> `Empty` remplace le pointeur nul ; `Node` possède ses sous-arbres via `Box`.
> Le `match` sur `BTree` rend les parcours naturels et exhaustifs.

## Task 01 — new & leaf

```rust
impl<T> BTree<T> {
    pub fn new() -> BTree<T>                 // Empty
    pub fn leaf(value: T) -> BTree<T>        // Node avec deux sous-arbres vides
}
```

## Task 02 — size & height

```rust
impl<T> BTree<T> {
    pub fn size(&self) -> usize       // nombre de nœuds
    pub fn height(&self) -> usize     // hauteur (Empty = 0)
}
```

## Task 03 — insert (BST)

Insère une valeur en respectant la propriété d'arbre binaire de recherche.
Nécessite `T: Ord`.

```rust
impl<T: Ord> BTree<T> {
    pub fn insert(&mut self, value: T)
}
```

> Compare avec le C : pas de fonction `cmp` à passer par pointeur ; la borne
> `T: Ord` garantit que `T` sait se comparer, vérifié à la compilation.

## Task 04 — contains

```rust
impl<T: Ord> BTree<T> {
    pub fn contains(&self, value: &T) -> bool
}
```

## Task 05 — in_order

Renvoie un `Vec<&T>` des valeurs en parcours infixe (gauche, racine, droite).
Pour un BST, c'est l'ordre croissant.

```rust
impl<T> BTree<T> {
    pub fn in_order(&self) -> Vec<&T>
}
```

## Task 06 — apply (parcours avec closure)

Applique une closure à chaque valeur, en parcours infixe.

```rust
impl<T> BTree<T> {
    pub fn for_each<F: FnMut(&T)>(&self, f: F)
}
```

> Astuce : tu peux écrire un helper interne prenant `f: &mut F` pour le passer
> récursivement aux deux sous-arbres.

## Task 07 — min & max

Renvoie la valeur minimale (resp. maximale) d'un BST non vide, ou `None`.

```rust
impl<T: Ord> BTree<T> {
    pub fn min(&self) -> Option<&T>
    pub fn max(&self) -> Option<&T>
}
```

## Task 08 — enum Expr (calculatrice)

Modélise une expression arithmétique avec un enum récursif et évalue-la.

```rust
pub enum Expr {
    Num(f64),
    Add(Box<Expr>, Box<Expr>),
    Sub(Box<Expr>, Box<Expr>),
    Mul(Box<Expr>, Box<Expr>),
    Div(Box<Expr>, Box<Expr>),
}

pub fn eval(expr: &Expr) -> Option<f64>   // None si division par zéro
```

> `(2 + 3) * 4` se construit en imbriquant les variantes. Le `match` exhaustif
> rend l'évaluation limpide. C'est un arbre… qui est aussi un programme.

## Task 09 — Bonus : concurrence sans peur

Calcule la somme d'un grand `Vec<i64>` en la répartissant sur plusieurs threads,
puis en combinant les résultats. Utilise `std::thread`, et `Arc`/`Mutex` (ou des
`JoinHandle` qui renvoient une valeur).

```rust
pub fn parallel_sum(data: &[i64], threads: usize) -> i64
```

Indices :

```rust
use std::sync::{Arc, Mutex};
use std::thread;
// Arc<T>  : Rc thread-safe (partage entre threads).
// Mutex<T>: accès mutuellement exclusif à une donnée partagée.
```

> Le compilateur **refuse** de partager une donnée non thread-safe entre threads :
> les data races sont des erreurs de compilation, pas des bugs à l'exécution.
> C'est ce que Rust appelle *fearless concurrency*. Rien à voir avec les
> `pthread` + verrous oubliés du C.

## Bonus graphique (optionnel)

Si tu veux l'équivalent du jour graphique CSFML, ajoute la crate `minifb` dans
`Cargo.toml` et ouvre une fenêtre 800×600 dans laquelle tu dessines des pixels
dans un `Vec<u32>` (un `u32` par pixel, `0x00RRGGBB`). Affiche un dégradé, puis
anime-le. C'est purement optionnel et hors notation.
