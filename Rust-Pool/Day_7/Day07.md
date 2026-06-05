# DAY 07 — STRUCTS, ENUMS & PATTERN MATCHING

> *Le jour « Structures » de la piscine C.* On y ajoute ce que le C n'a pas :
> les **enums à données**, le **pattern matching** (`match`), les méthodes
> (`impl`) et les traits dérivés (`#[derive(...)]`).

## Préliminaires

- **Langage** : Rust. `unsafe` interdit.
- Dérive `Debug` sur tes types pour pouvoir les afficher en test avec `{:?}`.
- Un `enum` Rust est une *union étiquetée sûre* : chaque variante peut porter des
  données différentes. C'est l'outil central du jour.

## Task 01 — struct Point

Définis une struct `Point { x: i32, y: i32 }` dérivant `Debug, Clone, PartialEq`,
avec :

```rust
impl Point {
    pub fn new(x: i32, y: i32) -> Point
    pub fn manhattan(&self, other: &Point) -> u32   // distance de Manhattan
}
```

> `&self` = méthode qui emprunte l'objet (comme `this` en lecture). Pas de
> pointeur explicite à gérer.

## Task 02 — struct Rectangle

```rust
pub struct Rectangle { pub width: u32, pub height: u32 }

impl Rectangle {
    pub fn area(&self) -> u32
    pub fn is_square(&self) -> bool
    pub fn can_hold(&self, other: &Rectangle) -> bool
}
```

## Task 03 — enum Shape

Définis un enum portant des données et calcule l'aire avec `match`.

```rust
pub enum Shape {
    Circle { radius: f64 },
    Rectangle { width: f64, height: f64 },
    Triangle { base: f64, height: f64 },
}

pub fn area(shape: &Shape) -> f64
```

> Le `match` doit être *exhaustif* : si tu ajoutes une variante, le compilateur
> t'obligera à la traiter. C'est une sécurité que le `switch` du C n'offre pas.

## Task 04 — enum Direction

```rust
pub enum Direction { North, East, South, West }

impl Direction {
    pub fn turn_right(&self) -> Direction
    pub fn opposite(&self) -> Direction
}
```

## Task 05 — Option : safe_div

Renvoie `a / b`, ou `None` si `b == 0`. Aucune valeur sentinelle.

```rust
pub fn safe_div(a: i32, b: i32) -> Option<i32>
```

## Task 06 — struct WordInfo (équivalent info_param)

C'est l'adaptation de la tâche `info_param` du jour 09 du C. Définis :

```rust
#[derive(Debug, PartialEq)]
pub struct WordInfo {
    pub length: usize,
    pub word: String,
    pub words: Vec<String>,   // résultat de my_str_to_word_array sur word
}

pub fn params_to_struct(args: &[String]) -> Vec<WordInfo>
```

Pour chaque argument, construis un `WordInfo` (sa longueur, une copie du mot, et
son découpage en sous-mots). Renvoie le `Vec`.

> En C, tu allouais un tableau de structures terminé par un champ nul. En Rust,
> un `Vec<WordInfo>` possède ses éléments et se libère seul. Pas de sentinelle.

## Task 07 — show_word_info

Affiche le contenu d'une slice de `WordInfo` : pour chaque entrée, le mot, sa
longueur, puis ses sous-mots (un par ligne).

```rust
pub fn show_word_info(infos: &[WordInfo])
```

## Task 08 — enum Tree (préparation)

Définis un arbre binaire d'entiers comme enum récursif et compte ses nœuds.

```rust
pub enum Tree {
    Leaf,
    Node(Box<Tree>, i32, Box<Tree>),
}

pub fn node_count(tree: &Tree) -> usize
```

> Tu retrouveras cette structure au jour 13, généralisée. Note comme l'enum
> exprime « soit une feuille, soit un nœud » de façon naturelle — bien plus clair
> qu'un pointeur potentiellement nul en C.
