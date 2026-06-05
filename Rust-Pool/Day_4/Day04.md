# DAY 04 — STRINGS & SLICES

> *Le jour « Pointers are back » de la piscine C* (les fonctions sur chaînes).
> En Rust, les chaînes sont en UTF-8 et il existe deux types fondamentaux :
> `String` (possédée, redimensionnable) et `&str` (vue empruntée). Ce jour
> introduit aussi l'écriture de tests, comme le jour 06 du C introduisait
> Criterion.

## Préliminaires

- **Langage** : Rust. `unsafe` interdit.
- **Tests obligatoires** à partir d'aujourd'hui. Écris-les dans `tests/` ou dans
  un module `#[cfg(test)]`, et lance `cargo test`.

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn strlen_basic() {
        assert_eq!(my_strlen("hello"), 5);
    }
}
```

- Attention : indexer une chaîne par octet (`s[0]`) **ne compile pas** en Rust,
  car un caractère peut faire plusieurs octets. On itère avec `.chars()` ou
  `.bytes()`, et on tranche avec `s.get(a..b)`.

## Task 01 — my_strlen

Nombre de caractères Unicode (pas d'octets).

```rust
pub fn my_strlen(s: &str) -> usize
```

> Compare `"café".chars().count()` (4) et `"café".len()` (5 octets). C'est tout
> l'enjeu de l'UTF-8.

## Task 02 — my_strupcase

Renvoie une *nouvelle* `String` où chaque lettre minuscule devient majuscule.

```rust
pub fn my_strupcase(s: &str) -> String
```

> En C tu modifiais en place un buffer alloué d'avance. En Rust, tu construis et
> renvoies une `String` : la propriété est transférée à l'appelant, pas de
> `free` à faire.

## Task 03 — my_strlowcase

Inverse de la précédente.

```rust
pub fn my_strlowcase(s: &str) -> String
```

## Task 04 — my_strcmp

Comparaison lexicographique. Renvoie un nombre négatif, nul, ou positif (comme
`strcmp`). Idiome Rust possible : renvoyer `std::cmp::Ordering`.

```rust
pub fn my_strcmp(a: &str, b: &str) -> i32
```

## Task 05 — my_rev_string

Renvoie la chaîne inversée (par caractères, pas par octets).

```rust
pub fn my_rev_string(s: &str) -> String
```

## Task 06 — is_palindrome

`true` si la chaîne est un palindrome (par caractères).

```rust
pub fn is_palindrome(s: &str) -> bool
```

## Task 07 — count_words

Nombre de mots, un mot étant une suite de caractères non blancs.

```rust
pub fn count_words(s: &str) -> usize
```

> `s.split_whitespace().count()` fait le travail — mais commence par le faire à
> la main avec `.chars()` pour comprendre, puis compare avec la version std.

## Task 08 — capitalize

Met en majuscule la première lettre de chaque mot (le reste en minuscule).
Exemple : `"hello   WORLD"` → `"Hello   World"` (les espaces sont préservés).

```rust
pub fn capitalize(s: &str) -> String
```

## Task 09 — my_str_isnum

`true` si la chaîne ne contient que des chiffres (et est non vide).

```rust
pub fn my_str_isnum(s: &str) -> bool
```
