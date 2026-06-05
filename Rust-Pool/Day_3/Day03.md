# DAY 03 — RÉCURSIVITÉ

> Identique en esprit à la piscine C, mais on profite de Rust pour signaler les
> erreurs avec `Option` plutôt qu'en renvoyant `0`.

## Préliminaires

- **Langage** : Rust. `unsafe` interdit.
- Quand un calcul est impossible ou déborde, renvoie `None` (et non une valeur
  sentinelle comme `0`). C'est plus honnête et c'est l'idiome Rust.
- Pense aux méthodes d'arithmétique vérifiée : `checked_mul`, `checked_add`...
  qui renvoient `Option`.

## Task 01 — factorial_it

Factorielle, version itérative. Renvoie `None` si `nb < 0` ou si le résultat
déborde.

```rust
pub fn factorial_it(nb: i64) -> Option<u64>
```

## Task 02 — factorial_rec

Même chose, version récursive.

```rust
pub fn factorial_rec(nb: i64) -> Option<u64>
```

## Task 03 — power_it

`nb` élevé à la puissance `p`, itératif. `None` si `p < 0` ou overflow.

```rust
pub fn power_it(nb: i64, p: i32) -> Option<i64>
```

## Task 04 — power_rec

Même chose, récursif.

```rust
pub fn power_rec(nb: i64, p: i32) -> Option<i64>
```

## Task 05 — square_root

Racine carrée *entière* : renvoie `Some(r)` si `r * r == nb`, sinon `None`.

```rust
pub fn square_root(nb: u64) -> Option<u64>
```

## Task 06 — is_prime

`true` si `nb` est premier, `false` sinon (0 et 1 ne sont pas premiers).

```rust
pub fn is_prime(nb: u64) -> bool
```

## Task 07 — find_prime_sup

Plus petit nombre premier supérieur ou égal à `nb`.

```rust
pub fn find_prime_sup(nb: u64) -> u64
```

## Task 08 — count_valid_queens_placements

Calcule récursivement le nombre de façons de placer `n` reines sur un échiquier
`n x n` sans qu'aucune ne puisse en prendre une autre.

```rust
pub fn count_valid_queens_placements(n: u32) -> u64
```

Exemples : `1 -> 1`, `2 -> 0`, `3 -> 0`, `4 -> 2`, `5 -> 10`, `8 -> 92`.

> Tu peux représenter l'état avec quelques `u32` en *bitmask* (colonnes,
> diagonales). Bel exercice pour combiner récursion et opérations binaires.

## Task 09 — ackermann (bonus)

Fonction d'Ackermann–Péter `A(m, n)`, purement récursive. Renvoie `None` en cas
d'overflow.

```rust
pub fn ackermann(m: u64, n: u64) -> Option<u64>
```
