# RUSH 1 — CARRÉS ASCII

> *Le premier rush de la piscine C : dessiner des carrés.* On le garde, mais en
> Rust : tes fonctions **construisent et renvoient une `String`** (testable
> facilement), plutôt que d'écrire directement à l'écran. On gère les erreurs
> avec `Result`.

## Préliminaires

- **Langage** : Rust. `unsafe` interdit. Projet binaire : `cargo new rush1`.
- **Travail de groupe.** Chaque membre doit pouvoir expliquer tout le code ; la
  note du groupe se base sur l'explication la plus faible. Découpez le travail
  mais relisez-vous mutuellement.
- En cas de taille invalide (`x` ou `y` valant 0), la fonction renvoie une
  `String` vide et le programme écrit `Invalid size` sur la sortie d'erreur (avec
  le retour ligne interprété), puis termine avec le code 84.

## But

Écrire des fonctions qui produisent un carré (rectangle) de dimensions `x` × `y`
sous forme de `String` (lignes séparées par `\n`, chaque ligne terminée par
`\n`). Le `main` lit deux entiers sur la ligne de commande et affiche le
résultat. Toutes les variantes doivent être faites.

```rust
fn main() {
    // parse les deux arguments en usize
    // appelle rush(x, y) et affiche le résultat, ou erreur -> exit(84)
}
```

Signature commune à chaque variante :

```rust
pub fn rush(x: usize, y: usize) -> String
```

## Variante 1 — `rush1`

Coins en `o`, bords horizontaux en `-`, bords verticaux en `|`.

`rush(5, 3)` :

```
o---o
|   |
o---o
```

## Variante 2 — `rush2`

Coins en `/` et `\` (haut-gauche `/`, haut-droite `\`, bas-gauche `\`,
bas-droite `/`), bords en `*`.

`rush(5, 3)` :

```
/***\
*   *
\***/
```

## Variante 3 — `rush3`

Coin haut-gauche `A`, autres coins du haut `A`/`C`… (reprends le motif Epitech) :
haut `A...A` n'est pas exigé à l'identique. Spécification retenue :
coins du haut en `A`, coins du bas en `C`, bords en `B`.

`rush(5, 3)` :

```
ABBBA
B   B
CBBBC
```

## Variante 4 — `rush4`

Haut-gauche `A`, haut-droite `C`, bas-gauche `A`, bas-droite `C`, bords `B`.

`rush(5, 3)` :

```
ABBBC
B   B
ABBBC
```

## Variante 5 — `rush5`

Haut-gauche `A`, haut-droite `C`, bas-gauche `C`, bas-droite `A`, bords `B`.

`rush(5, 3)` :

```
ABBBC
B   B
CBBBA
```

## Cas limites communs (à tester pour chaque variante)

- `rush(1, 1)` : un seul caractère (le coin approprié).
- `rush(5, 1)` : une seule ligne de bord supérieur.
- `rush(1, 5)` : une seule colonne.
- `rush(0, n)` ou `rush(n, 0)` : `String` vide.

## Conseils Rust

- Construis chaque ligne avec un `String` et `push`/`push_str`, ou via des
  itérateurs (`"-".repeat(x - 2)`).
- Une seule fonction interne `fn border_char(col, row, x, y) -> char` qui décide
  du caractère selon la position évite la duplication entre variantes.
- Écris des tests `assert_eq!(rush(5, 3), "o---o\n|   |\no---o\n");` pour chaque
  variante : c'est tout l'intérêt d'avoir choisi de renvoyer une `String`.
