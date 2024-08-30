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

The location is a simple struct which holds a column and a line number. For future purposes we are also going to add index and endindex which are used for error reporting. We are also going to hold a reference to the filename.

Create a new module called `location` in the `rune-core` crate:
runelang/rune-core/src/lexer/mod.rs

```rust
pub mod location;
```

Inside here we can create the location struct:
runelang/rune-core/src/lexer/location.rs

```rust
#[derive(Debug)]
pub struct Location {
    pub index: usize,
    pub end_index: usize,
    pub line: usize,
    pub column: usize,
}

impl Location {
  pub fn new(index: usize,column: usize, line: usize) -> Self {
    Self {
        index,
        end_index: index,
        line,
        column,
    }
  }

  pub fn new_with_range(index: usize,column: usize, line: usize, end_index: usize) -> Self {
    Self {
        index,
        end_index,
        line,
        column,
    }
  }
}
```
