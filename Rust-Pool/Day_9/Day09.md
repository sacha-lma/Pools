# DAY 09 — GÉNÉRIQUES & SMART POINTERS

> *Le jour « Linked lists » de la piscine C*, qui reposait sur `void *` (effacement
> de type) et des pointeurs de fonction. En Rust, on construit une liste chaînée
> **générique** `<T>` avec `Option<Box<Node<T>>>`, et les fonctions « apply » se
> font avec des closures. On découvre aussi pourquoi la liste chaînée est un cas
> d'école pour comprendre `Box`, `Rc` et `RefCell`.

## Préliminaires

- **Langage** : Rust. `unsafe` interdit.
- La structure imposée du jour :

```rust
pub struct Node<T> {
    pub data: T,
    pub next: Option<Box<Node<T>>>,
}

pub struct List<T> {
    pub head: Option<Box<Node<T>>>,
}
```

> `Option<Box<Node<T>>>` remplace le pointeur `next` du C : `None` est le
> « pointeur nul » sûr, `Some(box)` un nœud possédé sur le tas. Aucune fuite, pas
> de déréférencement de nul possible.

## Task 01 — new & push_front

```rust
impl<T> List<T> {
    pub fn new() -> List<T>
    pub fn push_front(&mut self, data: T)
}
```

## Task 02 — len

```rust
impl<T> List<T> {
    pub fn len(&self) -> usize
}
```

## Task 03 — from_slice

Construit une liste à partir d'une slice clonable. L'ordre de la liste (de la
tête vers la fin) correspond à l'ordre de la slice.

```rust
impl<T: Clone> List<T> {
    pub fn from_slice(items: &[T]) -> List<T>
}
```

## Task 04 — reverse

Inverse la liste sur place. À faire en *déplaçant* les nœuds (pas en clonant les
données).

```rust
impl<T> List<T> {
    pub fn reverse(&mut self)
}
```

> Exercice clé : tu vas manipuler `Option::take()` pour « retirer » un nœud et le
> déplacer. C'est l'occasion de comprendre comment Rust déplace des `Box` sans
> jamais laisser de pointeur pendouiller.

## Task 05 — apply_on_nodes

Applique une closure à la donnée de chaque nœud.

```rust
impl<T> List<T> {
    pub fn apply_on_nodes<F: FnMut(&mut T)>(&mut self, f: F)
}
```

> Remplace le `int (*f)(void *)` du C par un `FnMut(&mut T)` typé.

## Task 06 — find

Renvoie une référence vers la première donnée satisfaisant un prédicat.

```rust
impl<T> List<T> {
    pub fn find<F: Fn(&T) -> bool>(&self, pred: F) -> Option<&T>
}
```

## Task 07 — delete_matching

Supprime tous les nœuds dont la donnée satisfait le prédicat.

```rust
impl<T> List<T> {
    pub fn delete_matching<F: Fn(&T) -> bool>(&mut self, pred: F)
}
```

## Task 08 — concat

Met les nœuds de `other` à la suite de `self`, **en les déplaçant** (interdit de
créer de nouveaux nœuds ou de cloner). `other` devient vide.

```rust
impl<T> List<T> {
    pub fn concat(&mut self, other: List<T>)
}
```

## Task 09 — Rc & RefCell (mini-cours, bonus)

La liste simplement chaînée se fait bien avec `Box`. Mais une structure
*partagée* (plusieurs propriétaires) ou *cyclique* (liste doublement chaînée,
graphe) ne peut pas l'être : `Box` impose un propriétaire unique.

Lis et expérimente :

```rust
use std::rc::Rc;
use std::cell::RefCell;

// Rc<T>      : plusieurs propriétaires partagés (compteur de références).
// RefCell<T> : mutabilité vérifiée à l'exécution (emprunts dynamiques).
// Rc<RefCell<T>> : le combo courant pour de la donnée partagée et mutable.
```

Construis une petite structure `Shared = Rc<RefCell<i32>>`, clone-la, modifie la
valeur via un clone, et vérifie que tous les clones voient le changement. Écris
en commentaire pourquoi `Box` ne permettrait pas ça.

> Tu n'as pas besoin de `unsafe` ni de gérer un compteur à la main : `Rc` le fait.
> C'est la réponse idiomatique de Rust au partage que tu faisais avec des
> pointeurs bruts en C.
