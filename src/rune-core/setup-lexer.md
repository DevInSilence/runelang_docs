## Setup lexer

If you followed the project setup, you should have a library called `rune-core`. We are going to work on the setup of the lexer in this chapter. The lexer is responsible to convert a provided source code into a stream of tokens. The are different methods to implement a lexer e.g., using a lexer-parser generator, utilizing regular expressions, or writing a custom lexer. We are going to write a custom lexer for this project.

Just to talk about the other two namend methods:

1. Regex is a great approach it gives you a lot of flexibility and it is pretty easy to implement. The downside is that it can be rather slow. Wouldn't be a problem for this project.
2. Lexer-parser generators are great... No there are libraries which work quiet well like ANTLR and then there are libraries which are just a pain to work with like `lex` and `yacc`.

It comes down to your personal preference which method you want to use. In this book we are going to write a custom lexer-parser.

### Module

First we need a new module for the lexer:
runelang/rune-core/lib.rs

```rust
pub mod lexer;
```

### Location

The location is a simple struct which holds a `column` and a `line` number. For future purposes we are also going to add `start_index` and `end_index` which are used for error reporting. We are also going to hold a reference to the `filename`.

Create a new module called `location` in the `rune-core` crate: \
`runelang/rune-core/src/lexer/mod.rs`

```rust
pub mod location;
```

Inside here we can create the location struct: \
`runelang/rune-core/src/lexer/location.rs`

```rust
#[derive(Debug)]
pub struct Location {
    pub start_index: usize,
    pub end_index: usize,
    pub line: usize,
    pub column: usize,
    pub filename: &'static str,
}

impl Location {
    pub fn new(filename: &'static str, index: usize, column: usize, line: usize) -> Self {
        Self {
            start_index: index,
            end_index: index,
            line,
            column,
            filename,
        }
    }

    pub fn new_with_range(
        filename: &'static str,
        start_index: usize,
        column: usize,
        line: usize,
        end_index: usize,
    ) -> Self {
        Self {
            start_index,
            end_index,
            line,
            column,
            filename,
        }
    }
}
```

### TokenKind

Next we need a enum which holds the different kinds of `TokenKind`s. We are going to add the following kinds:

- `EOF`: End of file
- `Number`: Number
- `Identifier`: Identifier

Create a new module called `token_kind` in the `rune-core` crate: \
`runelang/rune-core/src/lexer/mod.rs`

```diff
pub mod location;
+ pub mod token_kind;
```

Inside here we can create the token kind enum: \
`runelang/rune-core/src/lexer/token_kind.rs`

```rust
#[derive(Debug)]
pub enum TokenKind {
    // GENERAL
    TkEof,

    // VALUES
    TkIdent,
    TkNumber,
}
```

### Token

The token struct holds the `TokenKind`, the `value` of the token, and the `location` of the token.

Create a new module called `token` in the `rune-core` crate: \
`runelang/rune-core/src/lexer/mod.rs`

```diff
pub mod location;
pub mod token_kind;
+ pub mod token;
```

Inside here we can create the token struct: \
`runelang/rune-core/src/lexer/token.rs`

```rust
use super::{location::Location, token_kind::TokenKind};

#[derive(Debug)]
pub struct Token {
    pub kind: TokenKind,
    pub loc: Location,
    pub lexeme: String,
}

impl Token {
    pub fn new(kind: TokenKind, loc: Location, lexeme: String) -> Self {
        Self { kind, loc, lexeme }
    }
}
```

### Lexer

The lexer is the main component of the lexer. It reads the source code and generates a stream of tokens. We are going to implement just the structure of the lexer in this chapter.

Create a new module called `lexer` in the `rune-core` crate: \
`runelang/rune-core/src/lexer/mod.rs`

```diff
pub mod location;
pub mod token_kind;
pub mod token;
+ pub mod lexer;
```

Inside here we can create the lexer struct: \
`runelang/rune-core/src/lexer/lexer.rs`

```rust
use super::token::Token;

#[allow(unused)] // just to silence the warning while we're working on this
pub struct Lexer {
    source: &'static str,
    index: usize,
    line: usize,
    column: usize,
    filename: &'static str,
    tokens: Vec<Token>,
}

impl Lexer {
    pub fn new(source: &'static str, filename: &'static str) -> Self {
        Self {
            source,
            index: 0,
            line: 1,
            column: 1,
            filename,
            tokens: Vec::new(),
        }
    }
}
```

### Source code

You can find the source code for this chapter under the [`setupLexer`](https://github.com/DevInSilence/runelang/releases/tag/setupLexer)