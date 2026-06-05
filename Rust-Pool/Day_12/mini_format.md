# MINI_FORMAT — UN PETIT FORMATEUR

> *L'équivalent de `mini_printf`.* En C, `printf` apprend les `va_args`
> (arguments variadiques non typés). **Rust n'a pas de variadique façon C** : la
> manière idiomatique de manipuler « une liste hétérogène d'arguments » est un
> **enum** d'arguments + un slice. Ce projet prépare `my_println!` (les macros),
> tout comme `mini_printf` préparait `my_printf`.

## Préliminaires

- **Langage** : Rust. `unsafe` interdit. Projet bibliothèque : `cargo new --lib mini_format`.
- Macros d'affichage de la std (`format!`, `write!`) **interdites** pour produire
  le résultat : tu construis la `String` toi-même, caractère par caractère. Tu
  peux utiliser `String::push`/`push_str` et tes fonctions des jours précédents.
- Tests obligatoires.

## Le type d'argument

On remplace les `...` du C par un type explicite :

```rust
pub enum Arg<'a> {
    Int(i64),
    Str(&'a str),
    Char(char),
}
```

> `<'a>` est un *lifetime* : il dit que la variante `Str` emprunte une chaîne qui
> doit vivre au moins aussi longtemps que l'`Arg`. C'est la sécurité de Rust qui
> remplace le danger des pointeurs non suivis du C.

## Task 01 — mini_format

Écris une fonction qui parcourt `format`, recopie les caractères normaux, et
remplace chaque spécificateur par l'argument correspondant (consommés dans
l'ordre). Gère les spécificateurs : `%d`, `%i`, `%s`, `%c`, et `%%` (un `%`
littéral).

```rust
pub fn mini_format(format: &str, args: &[Arg]) -> String
```

Exemples :

```rust
mini_format("hello", &[])                    == "hello";
mini_format("%d + %d = %d", &[Arg::Int(2), Arg::Int(3), Arg::Int(5)])
                                             == "2 + 3 = 5";
mini_format("name: %s", &[Arg::Str("Ada")])  == "name: Ada";
mini_format("100%%", &[])                    == "100%";
mini_format("%c%c", &[Arg::Char('h'), Arg::Char('i')]) == "hi";
```

Règles :

- `%d` et `%i` attendent un `Arg::Int` ; `%s` un `Arg::Str` ; `%c` un `Arg::Char`.
- Si le type de l'argument ne correspond pas au spécificateur, ou s'il manque un
  argument, c'est une erreur — voir Task 02.
- Le `%` final sans lettre est une erreur.

## Task 02 — mini_format_checked

Version qui ne *panique* jamais : renvoie une erreur en cas de mauvais usage
(spécificateur inconnu, type incompatible, arguments manquants ou en trop).

```rust
#[derive(Debug, PartialEq)]
pub enum FormatError {
    UnknownSpecifier(char),
    TypeMismatch,        // ex: %d avec un Arg::Str
    MissingArgument,
    TooManyArguments,
}

pub fn mini_format_checked(format: &str, args: &[Arg]) -> Result<String, FormatError>
```

> C'est l'esprit Rust : pas de comportement indéfini comme le `printf` du C quand
> les types ne collent pas. L'erreur est une valeur que l'appelant traite.

## Task 03 — print via std::io (bonus)

Ajoute une fonction qui écrit le résultat sur la sortie standard et renvoie le
nombre de caractères écrits (comme `printf` renvoie le nombre d'octets), en
propageant les erreurs d'I/O.

```rust
pub fn mini_print(format: &str, args: &[Arg]) -> Result<usize, Box<dyn std::error::Error>>
```

## Task 04 — un trait pour étendre les types (bonus)

Plutôt qu'un enum figé, définis un trait `Formattable` que des types peuvent
implémenter, et accepte `&[&dyn Formattable]`. Implémente-le pour `i64`, `&str`,
`char`, puis pour un type à toi (ex. un `Point`).

```rust
pub trait Formattable {
    fn format_into(&self, out: &mut String);
}
```

> Tu touches ici au *dynamic dispatch* (`dyn`), l'autre face du polymorphisme
> Rust (l'enum, lui, est du *static dispatch*). Bon pont vers `Display`/`Debug`.
