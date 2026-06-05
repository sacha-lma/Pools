# Piscine Rust

> Une piscine d'apprentissage intensif inspirée de la piscine C d'Epitech,
> mais repensée pour enseigner les **spécificités de Rust** : ownership,
> borrowing, traits, génériques, gestion d'erreurs avec `Result`, macros...
>
> Ce dépôt est une adaptation pédagogique indépendante. Il n'est ni affilié à
> Epitech ni officiel.

## Philosophie

La piscine C apprend à manipuler la mémoire « à la main » (`malloc`, pointeurs,
`void *`, codes d'erreur). En Rust, ces mécanismes existent autrement : la
mémoire est gérée par le système d'**ownership**, la généricité passe par les
**traits**, et les erreurs sont des **valeurs** (`Result`). Chaque jour a donc
été retravaillé pour viser le bon concept Rust plutôt que son équivalent C.

| Jour C | Jour Rust | Concept C remplacé | Concept Rust visé |
|---|---|---|---|
| 03 — First C | 03 — Premier programme | `my_putchar`, types | Types, `char` vs `u8`, shadowing, boucles |
| 04 — Pointers | 04 — Ownership & Borrowing | pointeurs, `&`/`*` | moves, `&`, `&mut`, slices |
| 05 — Recursivity | 05 — Récursivité | retour `0` en cas d'erreur | `Option`, arithmétique vérifiée |
| 06 — Pointers are back | 06 — Strings & Slices | `char *`, `my_strcpy` | `String`/`&str`, UTF-8, `cargo test` |
| 07 — Libmy, Arguments | 07 — Vec, Itérateurs & CLI | tableaux, `argv` | `Vec`, `std::env::args`, `.iter()` |
| 08 — Compilation, Allocation | 08 — Modules, Crates & Tas | `malloc`/`free`, lib | `Box`/`Vec`, modules, crate lib |
| 09 — Structures | 09 — Structs, Enums & Match | `struct`, `typedef` | `enum`, `impl`, pattern matching, `#[derive]` |
| 10 — Sorting | 10 — Traits, Closures & Iterators | pointeurs de fonction | closures, `Fn`/`FnMut`, adaptateurs |
| 11 — Linked lists | 11 — Génériques & Smart pointers | `void *`, liste brute | `Option<Box<T>>`, `<T>`, `Rc`/`RefCell` |
| 12 — File descriptors | 12 — Gestion d'erreurs & I/O | `open/read`, code 84 | `Result`, `?`, `std::fs`, `Box<dyn Error>` |
| 13 — Trees / Graphics | 13 — Enums récursifs : arbres | arbre via pointeurs | enum récursif, `Box`, + bonus concurrence |
| Rush 1 — Carrés | Rush 1 — Carrés ASCII | — | fonctions, `String`, `Result` |
| Rush 2 — Quelle langue ? | Rush 2 — Quelle langue ? | — | `HashMap`, itérateurs, I/O |
| mini_printf | mini_format | `va_args` | enum d'arguments, slices, `match` |
| my_printf | my_println! | `va_args` + flags | macro déclarative `macro_rules!` |

## Prérequis & installation

Installe la toolchain Rust avec **rustup** :

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustc --version   # doit afficher une version stable récente
cargo --version
```

Outils recommandés (formatage et linter) :

```bash
rustup component add rustfmt clippy
cargo fmt        # formate le code
cargo clippy     # signale les non-idiomes
```

## Structure d'un projet

Contrairement au C où tu livres des `.c` épars, en Rust tout vit dans un projet
Cargo. Pour chaque jour, crée un projet :

```bash
cargo new --lib day03      # crate bibliothèque (la plupart des jours)
cd day03
```

Disposition type :

```
day03/
├── Cargo.toml
└── src/
    └── lib.rs        # tes fonctions publiques (pub fn ...)
└── tests/
    └── tests.rs      # tes tests d'intégration (cargo test)
```

Tu écris tes fonctions dans `src/lib.rs` (ou dans des modules) et tu les
exposes avec `pub`. Les tests — les tiens et ceux de correction — appellent ces
fonctions. **Comme en C tu ne livrais pas ton `main`, ici la logique vit dans la
bibliothèque, pas dans un `main`.**

Pour les jours qui produisent un exécutable (Rush 1/2, jour 12...), utilise
`cargo new day12` (binaire) avec un `src/main.rs`.

## Conventions globales (à respecter chaque jour)

- **Édition** : `edition = "2021"` au minimum (2024 si disponible) dans `Cargo.toml`.
- **Erreurs** : les messages d'erreur s'écrivent sur la sortie d'erreur avec
  `eprintln!`. En cas d'erreur, le programme se termine avec le code **84** ;
  sinon **0**. On utilise `std::process::exit(84)` ou on remonte une erreur
  depuis `main() -> Result<(), ...>`.
- **`unsafe` est interdit** sur toute la piscine, sauf mention explicite. Tout
  l'intérêt est d'apprendre Rust *sûr*.
- **`.unwrap()` / `.expect()`** : tolérés dans les tests, mais à éviter dans le
  code livré dès le jour 12 (gestion d'erreurs). Avant, c'est acceptable.
- **Tests** : chaque fonction doit être testée. Lance `cargo test`. À partir du
  jour 06, l'écriture de tests fait partie de la notation.
- **Contraintes par jour** : comme la piscine C interdit la libc sauf
  `write/malloc/free`, chaque sujet précise ce qui est autorisé. Le but est de
  forcer la compréhension des fondamentaux avant d'ouvrir la std en grand.

## Comment travailler

1. Lis les *Préliminaires* du jour (langage, autorisations, livraison).
2. Fais les tâches dans l'ordre : chacune introduit une notion.
3. Écris un test par tâche avant de passer à la suivante.
4. Passe `cargo clippy` : il t'apprend les idiomes Rust gratuitement.

Bonne piscine.
