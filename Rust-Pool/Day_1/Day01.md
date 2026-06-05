# DAY 01 — PREMIER PROGRAMME RUST

## Préliminaires

- **Langage** : Rust.
- Tu livres tout ton `src/`, sans fichiers inutiles (`target/`, binaires...).
  Ajoute un `.gitignore` contenant `target/`.
- **Autorisé pour produire de la sortie** : uniquement ta fonction
  `my_putchar`, construite sur `std::io::Write`. Les macros `print!`,
  `println!`, `write!`, `format!`, ainsi que les types `String`, `Vec` et les
  tableaux (`[...]`) sont **interdits** aujourd'hui.
- Le but : manipuler les types primitifs, `char`, `u8`, les boucles et les
  fonctions, sans confort.

```rust
// Brique de base à mettre dans src/lib.rs, autorisée partout aujourd'hui :
use std::io::Write;

pub fn my_putchar(c: char) {
    let mut buf = [0u8; 4];
    let s = c.encode_utf8(&mut buf);
    std::io::stdout().write_all(s.as_bytes()).unwrap();
}
```

> En C, `char` est un entier 8 bits. En Rust, `char` est un *scalaire Unicode*
> de 4 octets, distinct de `u8`. Fais bien la différence tout au long du jour.

## Task 01 — my_putchar

Écris `my_putchar` (voir ci-dessus). Elle affiche un caractère.

```rust
pub fn my_putchar(c: char)
```

## Task 02 — my_print_alphabet

Affiche l'alphabet en minuscules sur une seule ligne, suivi d'un retour ligne :
`abcdefghijklmnopqrstuvwxyz`.

```rust
pub fn my_print_alphabet()
```

> Astuce : on peut itérer `for c in 'a'..='z'` (intervalle inclusif de `char`).

## Task 03 — my_print_reverse_alphabet

Affiche l'alphabet en minuscules à l'envers : `zyxwvutsrqponmlkjihgfedcba`.

```rust
pub fn my_print_reverse_alphabet()
```

## Task 04 — my_print_numbers

Affiche les chiffres de `0` à `9` : `0123456789`.

```rust
pub fn my_print_numbers()
```

## Task 05 — my_is_even

Affiche `1` si le nombre est pair, `0` sinon.

```rust
pub fn my_is_even(nb: i32)
```

## Task 06 — my_putnbr

Affiche un entier signé (en base 10), sans utiliser la conversion en chaîne.
Gère les négatifs et `i32::MIN`.

```rust
pub fn my_putnbr(nb: i32)
```

> Le cas `i32::MIN` est piégeux : `-i32::MIN` déborde. En Rust, l'overflow
> *panique* en mode debug (ce ne sera pas silencieux comme en C). Réfléchis à
> passer par `i64`, ou gère le dernier chiffre à part.

## Task 07 — my_print_comb

Affiche, en ordre croissant, toutes les combinaisons de 3 chiffres *différents*
et croissants, séparées par `, ` :
`012, 013, ..., 789`. Pas de virgule après la dernière.

```rust
pub fn my_print_comb()
```

## Task 08 — my_print_comb2

Affiche toutes les combinaisons de deux nombres à deux chiffres `(ab, cd)` tels
que `ab <= cd`, au format `ab cd`, séparées par `, ` :
`00 01, 00 02, ..., 98 99`.

```rust
pub fn my_print_comb2()
```

## Task 09 — my_putnbr_base

Affiche un entier `nb` dans une base donnée par sa chaîne de symboles.
Aujourd'hui les `&str` sont autorisés *uniquement* en lecture pour cette tâche.

```rust
pub fn my_putnbr_base(nb: i32, base: &str)
```

Exemple : `my_putnbr_base(42, "0123456789")` affiche `42` ;
`my_putnbr_base(42, "0123456789ABCDEF")` affiche `2A`.
