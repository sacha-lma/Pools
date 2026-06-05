# MY_PRINTLN! — UNE MACRO DÉCLARATIVE

> *L'équivalent de `my_printf`.* En C, `printf` est une fonction variadique. En
> Rust, **le vrai `println!` EST une macro** : c'est ainsi que le langage résout
> proprement le « nombre variable d'arguments hétérogènes ». Ce projet final
> t'apprend `macro_rules!`, la macro déclarative de Rust — le pendant idiomatique
> et type-sûr de la variadique du C.

## Préliminaires

- **Langage** : Rust. `unsafe` interdit. Projet bibliothèque : `cargo new --lib my_println`.
- **`println!`, `print!`, `format!`, `write!` de la std sont interdits.** Tu
  construis la sortie toi-même (réutilise `mini_format` et `my_putchar`).
- Tests obligatoires (les macros se testent comme n'importe quel code : on
  vérifie ce qu'elles produisent).

## Rappel : pourquoi une macro ?

Une fonction Rust a une signature fixe : elle ne peut pas accepter
`("x = %d", 5)` *puis* `("%s/%s", a, b)` avec des types différents et un nombre
variable. Une **macro** opère avant la compilation, sur la syntaxe : elle peut
accepter un nombre variable d'arguments et générer le bon code typé pour chacun.
C'est exactement ce que fait la `println!` de la std.

## Task 01 — la macro `my_format!`

Écris une macro qui prend une chaîne de format et un nombre variable
d'arguments, et qui produit une `String`. Elle s'appuie sur ton `mini_format` (ou
sur une logique équivalente) en construisant le slice d'`Arg` automatiquement.

Squelette à compléter :

```rust
#[macro_export]
macro_rules! my_format {
    // cas sans argument
    ($fmt:expr) => {
        $crate::mini_format($fmt, &[])
    };
    // cas avec un ou plusieurs arguments
    ($fmt:expr, $($arg:expr),+ $(,)?) => {
        $crate::mini_format($fmt, &[ $( $crate::to_arg(&$arg) ),+ ])
    };
}
```

Tu auras besoin d'une fonction (ou d'un trait) `to_arg` qui transforme une
valeur en `Arg` :

```rust
pub trait IntoArg {
    fn into_arg(&self) -> Arg<'_>;
}
// implémente IntoArg pour i64, i32, &str, String, char...
```

(adapte la macro pour appeler `(&$arg).into_arg()`).

Exemples attendus :

```rust
my_format!("hello")                 == "hello";
my_format!("%d + %d", 2, 3)         == "2 + 3";
my_format!("hi %s", "Ada")          == "hi Ada";
my_format!("%c!", 'x')              == "x!";
```

> `$($arg:expr),+` est la *répétition* : « un ou plusieurs `expr` séparés par des
> virgules ». `$(,)?` autorise une virgule finale. C'est la grammaire de
> `macro_rules!`.

## Task 02 — la macro `my_print!`

Comme `my_format!`, mais écrit le résultat sur la sortie standard (via ta brique
`my_putchar` ou `std::io::Write`), sans retour ligne.

```rust
#[macro_export]
macro_rules! my_print { /* ... */ }
```

## Task 03 — la macro `my_println!`

Comme `my_print!`, mais ajoute un `\n` final. Doit aussi fonctionner sans
argument (`my_println!()` affiche juste un retour ligne).

```rust
my_println!();                       // affiche "\n"
my_println!("score: %d", 42);        // affiche "score: 42\n"
```

## Task 04 — la macro `my_eprintln!`

Idem `my_println!` mais sur la sortie d'erreur. Utile pour la convention
d'erreurs de toute la piscine.

## Task 05 — flags (bonus)

Étends `mini_format` (et donc tes macros) pour gérer quelques options de
formatage, à la manière de `printf` :

- `%x` : entier en hexadécimal minuscule.
- `%b` : entier en binaire.
- largeur minimale avec remplissage d'espaces, ex. `%5d` → `"   42"`.
- remplissage par zéros, ex. `%05d` → `"00042"`.

> Tu réimplémentes une partie de la machinerie de formatage de Rust. C'est
> exactement le périmètre que `my_printf` couvrait en C, mais cette fois la
> sécurité de types est garantie par la macro et l'enum `Arg`.

## Pour aller plus loin

Une fois ceci maîtrisé, regarde comment écrire le trait `std::fmt::Display` pour
tes propres types : c'est la vraie porte d'entrée du formatage idiomatique en
Rust (`format!("{}", mon_objet)`), et la suite logique de ce projet.
