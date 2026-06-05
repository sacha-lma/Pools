# RUSH 2 — QUELLE LANGUE EST-CE ?

> *Le deuxième rush de la piscine C.* On le garde tel quel — il est parfait pour
> Rust : il combine `HashMap`, itérateurs, parsing d'arguments, gestion d'erreurs
> et formatage. Le but : deviner automatiquement la langue d'un texte.

## Préliminaires

- **Langage** : Rust. `unsafe` interdit. Projet binaire : `cargo new rush2`.
- **Travail de groupe** (mêmes règles que le Rush 1).
- Erreurs sur `stderr`, code de sortie 84 en cas d'erreur, 0 sinon.
- On ne considère que les caractères `a..z` et `A..Z` (pas d'accents, pas de
  ponctuation). Le projet est progressif : **ne commence une étape que si la
  précédente est totalement finie et testée.**

## Architecture conseillée

Mets la logique dans une bibliothèque (`src/lib.rs`) testable, et un `main.rs`
fin qui lit les arguments et appelle la lib. Lance `cargo test` à chaque étape.

## Étape 1 — Occurrences d'une lettre

Le programme reçoit un texte et une lettre, et affiche le nombre d'occurrences
de cette lettre (majuscule ou minuscule selon ce qui est demandé).

```rust
pub fn count_letter(text: &str, letter: char) -> usize
```

```
$> cargo run -- "Just because I don't care doesn't mean I don't understand!" a
a:4
$> cargo run -- "..." A
A:4
```

Respecte exactement ce format de sortie.

## Étape 2 — Plusieurs lettres

Le programme affiche le nombre d'occurrences de plusieurs lettres, dans l'ordre
où elles sont passées (chaque lettre, telle quelle).

```rust
pub fn count_letters(text: &str, letters: &[char]) -> Vec<(char, usize)>
```

```
$> cargo run -- "I hope I didn't brain my damage." a i
a:3
i:4
```

> Une `HashMap<char, usize>` construite en un passage sur le texte permet de
> répondre à toutes les lettres ensuite. Construis-la avec
> `for c in text.chars().filter(|c| c.is_ascii_alphabetic())`.

## Étape 3 — Fréquences

Ajoute, pour chaque lettre demandée, sa fréquence en pourcentage (2 décimales)
parmi toutes les lettres `a..z`/`A..Z` du texte.

```rust
pub fn frequency(text: &str, letter: char) -> f64   // en pourcentage
```

```
$> cargo run -- "Good things do not end with 'ium'. ..." a q e i | cat -e
a:3 (6.25%)$
q:0 (0.00%)$
e:4 (8.33%)$
i:6 (12.50%)$
```

> Format : `{:.2}` pour deux décimales. Attention à la division par zéro si le
> texte ne contient aucune lettre.

## Étape 4 — Détection de la langue

À partir d'une table de fréquences de lettres par langue (constante dans ton
code), et d'un algorithme de ton cru (par exemple, distance entre le vecteur de
fréquences du texte et celui de chaque langue), affiche la langue estimée à la
fin de la sortie. Ne considère que **English, French, German, Spanish**.

```rust
pub enum Language { English, French, German, Spanish }

pub fn detect_language(text: &str) -> Language
```

```
$> cargo run -- "Good things do not end with 'ium'. ..." a q e i | cat -e
a:3 (6.25%)$
q:0 (0.00%)$
e:4 (8.33%)$
i:6 (12.50%)$
=> English
```

> Problème volontairement ambigu : beaucoup de cas sont limites. Une distance
> euclidienne ou de Manhattan entre les 26 fréquences suffit pour un premier
> résultat correct. Documente ton choix d'algorithme.

## Conseils Rust

- `HashMap` (dans `std::collections`) pour compter en un passage.
- Les fréquences d'une langue tiennent dans un `[f64; 26]` constant.
- Calcule l'indice d'une lettre avec `c.to_ascii_lowercase() as u8 - b'a'`.
- Teste l'étape 4 sur des phrases connues de chaque langue.
