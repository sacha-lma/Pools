# DAY 10 — GESTION D'ERREURS & ENTRÉES/SORTIES

> *Le jour « File descriptors » de la piscine C* (`open`, `read`, `write`,
> `close`, code de retour 84). En Rust, les fichiers passent par `std::fs`/
> `std::io`, et **les erreurs sont des valeurs** : `Result<T, E>`, l'opérateur
> `?`, et `Box<dyn Error>`. On remplace les codes d'erreur dispersés par un flux
> d'erreurs propre et typé.

## Préliminaires

- **Langage** : Rust. `unsafe` interdit.
- À partir d'aujourd'hui, **bannis `unwrap()`/`expect()` du code livré** (ils
  restent tolérés dans les tests). Remonte les erreurs avec `?`.
- Convention de retour : `main() -> Result<(), Box<dyn std::error::Error>>` ; en
  cas d'erreur fatale, écris sur `stderr` et termine avec le code 84.

### Mini-cours : code d'erreur → Result

| En C | En Rust |
|---|---|
| `fd = open(...); if (fd < 0) return 84;` | `let f = File::open(path)?;` |
| `read(fd, buf, n)` | `f.read_to_string(&mut s)?` |
| oubli de `close` | fermeture automatique en fin de portée |
| `return 84;` partout | `return Err(...)` propagé par `?` |

## Task 01 — Result : parse_or

Renvoie l'entier parsé depuis `s`, ou `default` si le parsing échoue.

```rust
pub fn parse_or(s: &str, default: i32) -> i32
```

> Découvre `Result::unwrap_or` — mais d'abord, écris la version avec `match` sur
> le `Result` pour bien voir les deux variantes `Ok`/`Err`.

## Task 02 — l'opérateur `?`

Écris une fonction qui parse deux chaînes en `i32` et renvoie leur somme. Toute
erreur de parsing est propagée.

```rust
pub fn add_strs(a: &str, b: &str) -> Result<i32, std::num::ParseIntError>
```

> `let x = a.parse::<i32>()?;` : le `?` renvoie immédiatement l'erreur si présente.
> C'est le remplaçant idiomatique des `if (err) return 84;` en cascade.

## Task 03 — une erreur personnalisée

Définis ton type d'erreur et implémente les traits attendus pour qu'il
fonctionne avec `?` et `Box<dyn Error>`.

```rust
#[derive(Debug)]
pub enum MyError {
    Empty,
    NotANumber(String),
}

// impl std::fmt::Display for MyError { ... }
// impl std::error::Error for MyError {}

pub fn checked_sum(args: &[String]) -> Result<i64, MyError>
```

`checked_sum` additionne les arguments ; renvoie `Empty` si la slice est vide,
`NotANumber(s)` au premier argument non entier.

## Task 04 — read_file

Lis tout le contenu d'un fichier texte et renvoie-le en `String`. Propage toute
erreur d'I/O.

```rust
use std::fs;

pub fn read_file(path: &str) -> Result<String, std::io::Error>
```

> `fs::read_to_string(path)` fait tout, y compris l'ouverture **et** la fermeture.
> Plus de `open`/`close` à apparier à la main.

## Task 05 — count_lines

Renvoie le nombre de lignes d'un fichier. Réutilise `read_file`.

```rust
pub fn count_lines(path: &str) -> Result<usize, std::io::Error>
```

## Task 06 — write_lines

Écrit chaque chaîne d'une slice sur sa propre ligne dans un fichier (le crée ou
l'écrase). Propage les erreurs.

```rust
pub fn write_lines(path: &str, lines: &[String]) -> Result<(), std::io::Error>
```

## Task 07 — my_cat

Exécutable (`src/main.rs`) qui reproduit un `cat` minimal : pour chaque chemin
passé en argument, affiche le contenu du fichier sur la sortie standard. Si un
fichier est introuvable, écris l'erreur sur `stderr` et termine avec le code 84.

```rust
fn main() -> Result<(), Box<dyn std::error::Error>> {
    // std::env::args().skip(1) pour les chemins
    // std::process::exit(84) en cas d'erreur fatale
}
```

Exemple :

```
$> cargo run -- fichier1.txt fichier2.txt
... contenu de fichier1.txt ...
... contenu de fichier2.txt ...
```

## Task 08 — robust_average

Lit un fichier où chaque ligne est censée contenir un nombre, et renvoie leur
moyenne. Ignore les lignes vides ; renvoie une erreur claire si une ligne non
vide n'est pas un nombre, ou si aucun nombre n'est trouvé.

```rust
pub fn robust_average(path: &str) -> Result<f64, Box<dyn std::error::Error>>
```

> Combine I/O fichier, parsing, itérateurs et erreurs propagées. C'est la
> synthèse de la « vraie » gestion d'erreurs en Rust.
